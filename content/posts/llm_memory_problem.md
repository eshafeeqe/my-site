+++
title = "The Hardware Behind Large Language Models: The Hidden Memory Challenge"
date = 2025-01-15
draft = false
description = ""
tags = ["LLM", "memory"]
categories = ["general"]
math = true
+++

Training GPT-3 requires managing tens of terabytes of memory—but not where you'd expect.

Large language models may be the most disruptive technology of our era. While most people think of LLMs purely as software or machine learning systems, I'll argue that their impact stems equally from the hardware engineering behind them. The algorithmic innovations are crucial, but there's an equally impressive engineering story written in silicon, interconnects, and distributed systems.

It's easy to say "LLMs require massive GPU clusters," but let's ground this in concrete numbers by examining GPT-3.

GPT-3 uses a transformer architecture with the following specifications:

- 96 layers (transformer blocks)
- 96 attention heads
- Hidden dimension (d_model): 12,288
- Feed-forward dimension (d_ff): 49,152

Each transformer layer contains two main components:

**1. Multi-Head Attention Block**

- Four projection matrices (Q, K, V, Output): 4 × (12,288 × 12,288) = 4 × 150.99M
- **Total per layer: ~604M parameters**

**2. Feed-Forward Network (MLP)**

- First linear layer: 12,288 × 49,152 = 603.98M
- Second linear layer: 49,152 × 12,288 = 603.98M
- **Total per layer: ~1.21B parameters**

**Per Layer Total**: 604M + 1.21B = ~1.81B parameters

**All 96 Layers**: 1.81B × 96 = ~174B parameters

**Memory Footprint in FP16**: 174B parameters × 2 bytes = **348 GB**

This seems manageable—we could split the model across multiple GPUs. An 8× H100 cluster with 80GB VRAM each gives us 640GB total. Easy, right?

Not so fast. This 348GB is just the model weights. During training, you also need memory for gradients (another 348GB) and optimizer states (typically 2× parameters for Adam, adding 696GB). That's already over 1.3TB.

But here's the surprising part: **these aren't even the main memory bottleneck in LLM training.**

**A Simple Example Reveals the Real Problem**

Before diving into GPT-3, let's build intuition with a simple example. Consider this tiny 2-layer neural network:

```
x → W₁ → ReLU → h → W₂ → ReLU → y
```

**What's Needed for Computing Gradients During Backprop?**

For **W₂**, the gradient is computed as:

```
∂L/∂W₂ = ∂L/∂(W₂h) · hᵀ
```

For **W₁**, the gradient is computed as:

```
∂L/∂W₁ = ∂L/∂(W₁x) · xᵀ
```

Let's analyse what we need for these computations:

- To compute ∂L/∂W₂, we need **h** (the output from Layer 1)
- To compute ∂L/∂W₁, we need **x** (the original input)
- To compute the ReLU derivatives, we need **W₁x** and **W₂h** (the pre-activation values)

We have the weights W₁ and W₂. We have gradients flowing back from the loss. But **h**, **W₁x**, and **x**—those are intermediate values we computed during the forward pass. We must save them during the forward pass to use them during backpropagation.

This is extra memory we need: every activation computed going forward must be stored in memory to compute gradients going backward. Notice something different compared to other memory requirements? **This scales with batch size.** While model weights, gradients, and optimizer states remain constant regardless of batch size, activation memory grows linearly with the number of training examples processed simultaneously. Lets calculate the Activation Memory for GPT-3.

**Calculating Activation Memory for GPT-3**

Let's calculate for **Batch Size = 1** and **Sequence Length = 2,048**.

There are two main components:

**Part A: Linear Layer Activations (MLP + QKV)**

**Single tensor size**: B × S × d_model = 1 × 2,048 × 12,288 × 2 bytes ≈ **50 MB**

**Per transformer layer, we store:**

1. **Attention block inputs**:
    - Inputs to Q, K, V, and Output projections: 4 × 50 MB = **200 MB**
2. **MLP block inputs**:
    - Input to first layer (up-projection): 50 MB
    - Input to second layer (down-projection): 1 × 2,048 × 49,152 × 2 bytes ≈ **200 MB**
3. **Layer Norms**: 2 × 50 MB ≈ **100 MB**

**Total per layer**: 200 + 50 + 200 + 100 = **550 MB**

**Part B: Attention Score Matrix (The Memory Killer)**

The attention mechanism computes a score for every token against every other token. This creates a matrix that grows quadratically with sequence length—doubling the sequence length quadruples the attention memory.

- **Shape**: Batch × Heads × Seq × Seq
- **Size**: 1 × 96 × 2,048 × 2,048 = 402,653,184 elements
- **Memory**: 402M × 2 bytes ≈ **805 MB**

This quadratic scaling is why long-context models face such severe memory constraints—a 32K context length would require 256× more attention memory than a 2K context.

**Total Activation Memory Per Layer**: 550 MB + 805 MB ≈ **1.36 GB**

**For All 96 Layers**: 1.36 GB × 96 ≈ **130 GB per single training example**

With a batch size of 512, the required activation memory reaches **tens of terabytes**.

**Why Hardware Engineering Matters**

Activation memory is one of the major bottlenecks that makes training large language models a significant hardware engineering challenge. The solutions include:

**Gradient Checkpointing**: Store only a subset of activations and recompute the rest during backpropagation, trading compute for memory.

**Pipeline Parallelism**: Split the model across GPUs, with each handling different layers to distribute the memory burden.

**Tensor Parallelism**: Split individual weight matrices across GPUs to handle models that don't fit on any single device.

**Flash Attention**: Reorder attention computations to avoid materializing the full attention matrix, dramatically reducing memory usage.

These aren't optimizations—they're requirements. Without them, training models at GPT-3 scale would be physically impossible with current hardware.

The next generation of LLMs will push these boundaries even further. Models with trillions of parameters, longer context windows and larger batch sizes will demand continued innovation in both hardware architecture and systems engineering.