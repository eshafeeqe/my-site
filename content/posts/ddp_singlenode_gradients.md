+++
title = "Understanding DDP: How Gradients are Synchronised Across GPUs?"
date = 2025-01-15
draft = false
description = ""
tags = ["introduction", "blog"]
categories = ["general"]
+++


As deep learning models scale, training on a **single GPU** becomes too slow. To understand how DDP speeds this up, we first look at the basic building block: **one training iteration on one GPU**. In this loop, a batch goes through forward → loss → backward → optimizer step. A minimal PyTorch example looks like this:

```python 
for inputs, targets in train_dl:
    # Forward pass
    outputs = model(inputs)
    loss = loss_func(outputs, targets)

    # Backward pass + weight update
    loss.backward()
    optimizer.step()
    optimizer.zero_grad()

    # Optional: track metrics
    metrics.update(outputs, targets)
```

At first glance the training loop looks simple, but there is a lot happening internally. The input moves through the model **layer by layer**, with each layer’s operations (matrix multiplies, convolutions, activations) executed in parallel. After the final layer, the **loss** is computed. During the backward pass, gradients are propagated **from the output layer back to the earlier layers**, again sequential across layers but parallel within each layer. Once all gradients are available, the optimizer **updates all parameters in parallel**. This sequence—forward pass, loss computation, backward pass, and weight update—forms the core of training on a single GPU.

As datasets grow and models become larger, both the time per training iteration and the total number of required iterations increase. This leads to a bottleneck where each epoch takes significantly longer, slowing down overall training progress and delaying model improvement. To reduce this training time, we need a way to **increase throughput without changing the underlying training algorithm**. This is exactly where **Distributed Data Parallel (DDP)** becomes essential. 

DDP allows multiple GPUs to work on different portions of the dataset **in parallel**, each computing its own forward pass, backward pass, and gradients. These gradients are then **synchronised and averaged** across all GPUs, ensuring every replica of the model stays consistent. 

Let's see main differences for DDP 

```python 
# Init process group
dist.init_process_group(backend="nccl")
rank = dist.get_rank()
local_rank = int(os.environ["LOCAL_RANK"])
torch.cuda.set_device(local_rank)

model = nn.parallel.DistributedDataParallel(
        model, device_ids=[local_rank]
    )
    
# Dataset + DistributedSampler
sampler = DistributedSampler(dataset)
train_dl = DataLoader(dataset, batch_size=32, sampler=sampler)
    
for inputs, targets in train_dl:

	# Move batch to this GPU
    inputs = inputs.cuda(local_rank, non_blocking=True)
    targets = targets.cuda(local_rank, non_blocking=True)
    
    # Forward pass
    outputs = model(inputs)
    loss = loss_func(outputs, targets)

    # Backward pass + weight update
    loss.backward()
    optimizer.step()
    optimizer.zero_grad()

    # Optional: track metrics
    metrics.update(outputs, targets)

dist.destroy_process_group()

```

Although the training loop stays almost the same, DDP introduces a few key changes:
#### **1. Initialize the distributed environment**

Each GPU runs its own process, so you must create a process group and select the GPU for that process:

`dist.init_process_group("nccl") local_rank = int(os.environ["LOCAL_RANK"]) torch.cuda.set_device(local_rank)`
#### **2. Wrap the model with DistributedDataParallel**

This enables automatic gradient averaging across GPUs during `loss.backward()`.\
`model = DistributedDataParallel(model, device_ids=[local_rank])`
#### **3. Use a DistributedSampler**

Each process gets a unique shard of the dataset:\
`sampler = DistributedSampler(dataset)`\
`train_dl = DataLoader(dataset, sampler=sampler)`
#### **4. Move batches to the correct GPU**

Each process sends data to its assigned GPU:
`inputs = inputs.cuda(local_rank) targets = targets.cuda(local_rank)`
#### **5. Backward pass now triggers gradient synchronisation**

You write the same code:
`loss.backward() optimizer.step()`
But under the hood, **DDP performs an all-reduce** to average gradients across all GPUs before the optimizer step.
#### **6. Cleanup at the end**
`dist.destroy_process_group()`

Distributing the data to each GPU and doing forward pass is very evident from the code changes, but gradient computation and synchronisation during backward pass is the real magic happening that is where the limit of DDP is testing. During backward pass each layer compute the gradient loss with respect to sub batch data.  To make the gradient synchronisation process easier to understand, let’s simplify the situation.  Imagine we have **4 GPUs** (A, B, C, D), and each GPU has computed gradients for a layer with just **4 parameters**. We'll call these gradients:

- GPU A computed: `[a1, a2, a3, a4]`
- GPU B computed: `[b1, b2, b3, b4]`
- GPU C computed: `[c1, c2, c3, c4]`
- GPU D computed: `[d1, d2, d3, d4]`

Our goal is for **every GPU to end up with the averaged gradient**: `[(a1+b1+c1+d1)/4, (a2+b2+c2+d2)/4, ...]` for all parameters.

The naive approach would be to send all gradients to one GPU, compute the average, then broadcast back—but this creates a bottleneck. Instead, DDP uses the **All-Reduce** algorithm, which efficiently distributes both the computation and communication across all GPUs.

Before we dive into the details, here's the complete process visualised:

{{< youtube QbHK7n0Wnfc >}}
### Phase 1: Reduce-Scatter (Rounds 0-3)

The All-Reduce operation consists of two phases. First is **Reduce-Scatter**, where each GPU becomes responsible for averaging one portion of the gradients.

#### **Round 0: Initial State**

Each GPU starts with its own locally computed gradients:

- **GPU A**: `[a1, a2, a3, a4]`
- **GPU B**: `[b1, b2, b3, b4]`
- **GPU C**: `[c1, c2, c3, c4]`
- **GPU D**: `[d1, d2, d3, d4]`

#### **Round 1: First Exchange**

In a ring topology, GPUs exchange specific chunks with their neighbours:

- GPU C sends `c1` to GPU D and receives `d1` from GPU D, resulting in `c1 + d1`
- GPU B sends `b4` to GPU C and receives `c4` from GPU C, resulting in `b4 + c4`
- Similar exchanges happen between other GPU pairs

After Round 1:

- **GPU A**: `[a1, a2, a3+b3, a4]`
- **GPU B**: `[b1, b2, b3, b4+c4]`
- **GPU C**: `[c1+d1, c2, c3, c4]`
- **GPU D**: `[d1, a2+d2, d3, d4]`

#### **Round 2: Second Exchange**

The partially reduced chunks continue to accumulate:

- **GPU A**: `[a1, a2, a3+b3, a4+b4+c4]`
- **GPU B**: `[b1+c1+d1, b2, b3, b4+c4]`
- **GPU C**: `[c1+d1, a2+c2+d2, c3, c4]`
- **GPU D**: `[d1, a2+d2, a3+b3+d3, d4]`

Notice how each position is accumulating values from more GPUs.

#### **Round 3: Third Exchange**

After this round, each GPU holds the **complete sum** for its assigned chunk:

- **GPU A**: `[a1+b1+c1+d1, a2, a3+b3, a4+b4+c4]`
- **GPU B**: `[b1+c1+d1, a2+b2+c2+d2, b3, b4+c4]`
- **GPU C**: `[c1+d1, a2+c2+d2, a3+b3+c3+d3, c4]`
- **GPU D**: `[d1, a2+d2, a3+b3+d3, a4+b4+c4+d4]`

Now each GPU is responsible for one fully-reduced chunk:

- GPU A owns the sum for parameter 0: `a1+b1+c1+d1`
- GPU B owns the sum for parameter 1: `a2+b2+c2+d2` 
- GPU C owns the sum for parameter 2: `a3+b3+c3+d3`
- GPU D owns the sum for parameter 3: `a4+b4+c4+d4`

### Phase 2: All-Gather (Rounds 4-6)

Now that each GPU has the complete sum for one chunk, we need to **distribute these sums to all GPUs**. This is the All-Gather phase.

#### **Round 4: First Gather**

GPUs begin sharing their fully-reduced chunks:

- **GPU A**: `[a1+b1+c1+d1, a2+b2+c2+d2, a3+b3, a4+b4+c4]`
- **GPU B**: `[b1+c1+d1, a2+b2+c2+d2, a3+b3+c3+d3, b4+c4]`
- **GPU C**: `[c1+d1, a2+c2+d2, a3+b3+c3+d3, a4+b4+c4+d4]`
- **GPU D**: `[a1+b1+c1+d1, a2+d2, a3+b3+d3, a4+b4+c4+d4]`

#### **Round 5: Second Gather**

More complete sums are propagated:

- **GPU A**: `[a1+b1+c1+d1, a2+b2+c2+d2, a3+b3+c3+d3, a4+b4+c4]`
- **GPU B**: `[b1+c1+d1, a2+b2+c2+d2, a3+b3+c3+d3, a4+b4+c4+d4]`
- **GPU C**: `[a1+b1+c1+d1, a2+c2+d2, a3+b3+c3+d3, a4+b4+c4+d4]`
- **GPU D**: `[a1+b1+c1+d1, a2+b2+c2+d2, a3+b3+d3, a4+b4+c4+d4]`

#### **Round 6: Final State**

After the final exchange, **all GPUs have identical, fully-reduced gradients**:

- **All GPUs**: `[a1+b1+c1+d1, a2+b2+c2+d2, a3+b3+c3+d3, a4+b4+c4+d4]`

At this point, each GPU can divide by the number of GPUs (4) to get the averaged gradients, and the optimizer can proceed with the weight update.

### Why This Matters

This **Reduce-Scatter + All-Gather** approach is elegant because:

1. **No single bottleneck**: Every GPU participates equally in both computation and communication
2. **Bandwidth efficient**: For N GPUs and a gradient vector of size M, only `2(N-1)/N × M` data needs to be transferred per GPU (compared to `2M` for naive broadcast)
3. **Overlapping with computation**: In practice, DDP doesn't wait for all layers to finish—it starts the all-reduce for earlier layers while later layers are still computing gradients (known as gradient bucketing)

This is the "magic" that happens during `loss.backward()` when using DistributedDataParallel. The training loop looks identical to single-GPU training, but under the hood, this sophisticated communication pattern ensures all GPUs stay synchronised.

## When DDP Isn't Enough

While DDP  handles gradient synchronisation on a single node, it has fundamental limitations:

- **Memory constraints**: The entire model must fit on each GPU's memory
- **Communication overhead**: Beyond 8-16 GPUs, the all-reduce becomes expensive
- **Single-node limit**: Most machines max out at 8 GPUs
