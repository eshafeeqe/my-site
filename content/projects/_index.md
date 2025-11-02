---
title: "Projects"
description: "My career focuses on the intersection of computer vision, robotics, and machine learning. I specialize in developing and deploying robust algorithms for 3D reconstruction, state estimation, and reinforcement learning. My work spans from foundational academic research to building production-ready ML systems. Below are highlights of my projects to the field."
hideMeta: true
---

## Featured Projects

### Quadrupedal Robots
At Antikythera Robotics, I developed and simulated Reinforcement Learning (RL) controllers for walking, self-righting, and balancing on a quadruped robot. This project involved architecting a real-time state estimation framework using an Extended Kalman Filter (EKF) to fuse data from 12 actuators, 2 IMUs, and a stereo camera.

*   **Tech Stack:** Python, C++, ROS, PyBullet, Drake, MuJoCo, PyTorch, Stable-Baselines3
*   **[See the project details »](/projects/quadruped-robotics-rl/)** 

### Real-time Monocular SLAM and Flicker-Free Segmentation Model
As a Research Programmer at Broadcast Virtual, I developed and deployed a monocular SLAM system that enables logo placement on a wide range of broadcast cameras without requiring additional sensors. This work involved porting keypoint detectors (SuperPoint, DISK) to TensorRT and implementing custom CUDA-based matchers to achieve real-time performance. I also prototyped a segmentation model for detecting on-screen graphics in broadcast videos and contributed to R&D efforts aimed at making LED replacement segmentation models flicker-free.

*   **Tech Stack:** C++, Python, PyTorch, TensorRT, CUDA, MLflow, Docker, Kubernetes
*   **[See the project details »](/projects/monocular-slam-broadcast/)** 

### Tennis Video Analysis Platform

This project involves the development and deployment of a production-ready tennis video analysis platform that automatically extracts rallies, classifies shots, and provides AI-driven coaching feedback. The system uses audio signal processing to detect ball impact sounds for rally segmentation and applies computer vision models for pose estimation and shot classification, distinguishing forehand, backhand, and serve actions. The platform is designed with a distributed worker architecture, where isolated processing nodes communicate with the main server through HTTP APIs, enabling secure and scalable video processing. An integrated Google Gemini–based coaching system analyzes gameplay and generates contextual YouTube tutorial recommendations linked to specific technique issues identified in the footage.

*   **Tech Stack:**: Python, FastAPI, Next.js, PostgreSQL, RabbitMQ, Celery, Docker, MoveNet, OpenVINO, Librosa, FFmpeg, Google Gemini 2.5
*   **[See the project details »](/projects/tennis-video-analysis/)** 