---
title: "Real-time Monocular SLAM and Flicker-Free Segmentation Model"
hideMeta: true
draft: false
hiddenInList: true
---
The main challenge in developing a **monocular SLAM system for sports videos** was that traditional keypoint detectors (such as SIFT and FAST) performed poorly due to the low-texture and repetitive nature of sports scenes. This led us to explore **deep learning–based feature detectors** such as SuperPoint, DISK, and DarkFeat. Initial evaluations showed that these deep models performed significantly better for feature tracking; however, achieving **real-time performance** was a major bottleneck.

Many of these research models are difficult to convert to TensorRT because of their complex architectures and non-standard mathematical operations. To overcome this, we optimized the **inference pipeline** by selectively porting the neural network components to TensorRT and implementing the mathematical operations using **CUDA libraries** (cuSOLVER, cuSPARSE, CUB). Through these optimizations, we achieved a **5 ms inference time** for the DISK [1] detector while maintaining robust tracking performance.

Another key challenge was adapting the detectors to **varying lighting conditions**, as illumination can change drastically during live sporting events. We found that **RoMa** [2] performed well in such scenarios, and we ported it to TensorRT and CUDA. By removing redundant operations and optimizing precision modes (FP16, FP8, INT8), we improved its runtime from **800 ms to 50 ms**, achieving a **16× speedup**.

Subsequently, I worked on developing a **video-based segmentation model** for detecting on-screen graphics in broadcast videos. A major limitation was the **lack of labeled data**, so we leveraged programmable graphics templates available from [overlays.uno](https://overlays.uno/). Using a headless browser setup (via Playwright), we collected RGB and alpha channel data from these layouts. We curated around **80 unique labels** and generated **20 color variations** for each by randomizing hue values in HSV space. Combined with additional augmentations, this dataset enabled training a segmentation model that generalized well to unseen broadcast graphics.

Later, I focused on improving the segmentation model for LED overlays used in live sports feeds. Since even minor pixel misclassifications could cause visible flicker and audience distraction, we needed to evaluate segmentation stability over time—something not well captured by conventional frame-based IoU metrics. To support this analysis, we developed an observability tool using the Dash framework (https://dash.plotly.com/) to visualize segmentation outputs and ground truth data. We selected Dash instead of Gradio or Streamlit because it provides finer control over layout, interactivity, and data handling, even though it has a steeper learning curve. This flexibility allowed us to build customized visualizations for identifying class-wise weaknesses and refining loss weighting strategies.

To assess **temporal consistency**, we designed a **video-based evaluation metric** using high-quality optical flow to detect flicker artifacts in segmentation outputs. Creating densely labeled video datasets for frame-by-frame evaluation is highly labor-intensive and impractical at scale, especially for long sports broadcasts. To overcome this limitation, we used optical flow to estimate pixel-level motion between consecutive frames and identify inconsistent segmentation regions without requiring dense manual labels. This approach allowed us to **quantify perceptual stability** and **rank models based on flicker intensity**, providing a scalable and practical measure aligned with real viewer experience.

**References:**  
[1] _DISK: Learning Local Features with Policy Gradient_ — [https://arxiv.org/abs/2006.13566](https://arxiv.org/abs/2006.13566)  
[2] _RoMa: Robust Dense Feature Matching_ — [https://arxiv.org/abs/2305.15404](https://arxiv.org/abs/2305.15404)

## Demo videos and images

### RoMa relocalization
<img 
  src="https://r2.eshafeeqe.space/works/bv/image%20(1).png" 
  alt="Example image" 
  width="100%" 
  style="max-height: 400px; object-fit: contain;" 
/>

### RoMa vs DISK tracking
<video width="100%" style="max-height: 400px; object-fit: contain;" controls>
  <source src="https://r2.eshafeeqe.space/works/bv/comparison.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

### Frame relocalization
<video width="100%" style="max-height: 400px; object-fit: contain;" controls>
  <source src="https://r2.eshafeeqe.space/works/bv/Frameaccuraterelocalise.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

### Basketball full view
<video width="100%" style="max-height: 400px; object-fit: contain;" controls>
  <source src="https://r2.eshafeeqe.space/works/bv/Fullscreentile%20Basketball%201.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

### Graphics Segmentation 
<video width="100%" style="max-height: 400px; object-fit: contain;" controls>
  <source src="https://r2.eshafeeqe.space/works/bv/Bahrain%20Darts%20Masters%202025%20Day%202%20Processed.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

### Data tool
<img 
  src="https://r2.eshafeeqe.space/works/bv/image%20(2).png" 
  alt="Example image" 
  width="100%" 
  style="max-height: 400px; object-fit: contain;" 
/>

### Player Segmentation 
<video width="100%" style="max-height: 400px; object-fit: contain;" controls>
  <source src="https://r2.eshafeeqe.space/works/bv/Efl%2025%2026%20Ch%20Pne%20Mid%20Cam1%20%2000%2033%2010-Unet-Ktanh.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

### Optical flow qa analysis
<img 
  src="https://r2.eshafeeqe.space/works/bv/image.png" 
  alt="Example image" 
  width="100%" 
  style="max-height: 400px; object-fit: contain;" 
/>

### Optical flow temporal consistency 
<video width="100%" style="max-height: 400px; object-fit: contain;" controls>
  <source src="https://r2.eshafeeqe.space/works/bv/4Panel%20Visualization.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>