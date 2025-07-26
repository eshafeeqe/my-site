---
title: "Research and Publications"
description: "My research focuses on the intersection of computer vision, robotics, and machine learning. I specialize in developing and deploying robust algorithms for 3D reconstruction, state estimation, and reinforcement learning. My work spans from foundational academic research to building production-ready ML systems. Below are highlights of my projects, publications, and contributions to the field."
---

## Featured Research Projects

Here are some of the key projects I've worked on. Each project represents a deep dive into solving complex technical challenges.

### PhD Thesis: Non-rigid 3D Reconstruction of the Human Body in Motion
My PhD thesis at QUT and CSIRO Data61 introduced a novel method to decouple camera and object motion for high-accuracy 3D reconstruction. This work also included a skeleton-driven optimization framework to correct tracking failures for rapidly moving subjects.

*   **Tech Stack:** C++, CUDA, Python, TensorFlow, PyTorch, OpenCV, PCL
*   **Links:** <a href="https://eprints.qut.edu.au/205095/1/Shafeeq_Elanattil_Thesis.pdf" target="_blank" rel="noopener noreferrer"><b> Thesis Link</b></a>

### Reinforcement Learning for Quadruped Robotics
At Antikythera Robotics, I developed and simulated Reinforcement Learning (RL) controllers for walking, self-righting, and balancing on a quadruped robot. This project involved architecting a real-time state estimation framework using an Extended Kalman Filter (EKF) to fuse data from 12 actuators, 2 IMUs, and a stereo camera.

*   **Tech Stack:** Python, C++, ROS, PyBullet, Drake, MuJoCo, PyTorch, Stable-Baselines3
*   **[Learn more about this project »](/research/quadruped-robotics-rl/)**

### Real-time Monocular SLAM System
As a Senior Research Programmer at Broadcast Virtual, I led the development and deployment of a monocular SLAM system to enable logo placement on a wide range of cameras without extra sensors. This involved porting keypoint detectors (SuperPoint, DISK) to TensorRT and implementing custom CUDA matchers for real-time performance.

*   **Tech Stack:** C++, Python, PyTorch, TensorRT, CUDA, MLflow, Docker, Kubernetes
*   **[See the project details »](/research/monocular-slam-broadcast/)**

---

## Publications

### Conferences
*   **SKELETON DRIVEN NON-RIGID MOTION TRACKING AND 3D RECONSTRUCTION**  
    *<b>Shafeeq Elanattil</b>, Peyman Moghadam, Simon Denman, Sridha Sridharan, Clinton Fookes.*  
    *Digital Image Computing: Techniques and Applications (DICTA) 2018.*  
    [<a href="https://eshafeeqe.github.io/homepage/dicta_website/" target="_blank"><b>project page</b></a>]  [<a href="https://arxiv.org/pdf/1810.03774.pdf" target="_blank"><b>PDF</b></a>]
     [<a href="https://eshafeeqe.github.io/homepage/dicta_website/DICTA_Poster.pdf" target="_blank"><b>poster</b></a>]

*   **NON-RIGID RECONSTRUCTION WITH A SINGLE MOVING RGB-D CAMERA**  
    *<b>Shafeeq Elanattil</b>, Peyman Moghadam, Simon Denman, Sridha Sridharan, Clinton Fookes, Mark Cox.*  
    *International Conference on Pattern Recognition (ICPR) 2018.*  
    [<a href="https://eshafeeqe.github.io/homepage/icpr_website/" target="_blank"><b>Project Page</b></a>]  [<a href="https://arxiv.org/pdf/1805.11219.pdf" target="_blank"><b>PDF</b></a>] [<a href="https://youtu.be/jkJCyTn0me0" target="_blank"><b>Video</b></a>] 
     
    <a href="https://arxiv.org/pdf/1805.12443.pdf"><img class="thumbs" src="https://eshafeeqe.github.io/homepage/icpr_website/" style=""></a>
*   **NEW FEATURE DETECTION MECHANISM FOR EXTENDED KALMAN FILTER BASED MONOCULAR SLAM WITH 1-POINT RANSAC**  
    *<b>Shafeeq Elanattil</b>, Agniva Sengupta.*  
    *Mining Intelligence and Knowledge Exploration (MIKE) 2015.*  
    [<a href="https://arxiv.org/pdf/1805.12443.pdf" target="_blank"><b>PDF</b></a>]

---

## Datasets

I believe in contributing to the open research community. Here are the datasets I have created and made public.

*   **Synthetic Data for Non-Rigid 3D Reconstruction using a Moving RGB-D Camera**  
    A dataset created for our ICPR 2018 publication, now integrated into the **SLAMBench 3.0** benchmark suite for evaluating SLAM algorithms.  
    **[<a href="https://data.csiro.au/collection/csiro:34677v2" target="_blank"><b>Link to Dataset</b></a>]**

*   **Synthetic Human Model Dataset for Skeleton Driven Non-Rigid Motion Tracking**  
    A comprehensive dataset for evaluating the reconstruction of fast, articulated human motion, created to validate the work in my DICTA 2018 publication.  
    **[<a href="https://data.csiro.au/collections/#collection/CIcsiro:38398v1/DItrue" target="_blank"><b>Link to Dataset</b></a>]**