---
title: "Quadrupedal Robots"
hideMeta: true
draft: false
hiddenInList: true
---

During this project, our main challenge was addressing the **sim-to-real gap** between the physical quadruped robot and its simulation environment. We initially explored **reinforcement learning (RL)** algorithms for locomotion tasks such as walking and turning. However, we found that these RL-based controllers did not transfer well to the real robot due to discrepancies between simulation and real-world dynamics.

The commonly used simulators (PyBullet, Gazebo) model the robot as a multi-rigid-body system but do not accurately represent the **motor dynamics** within the robot. To address this, we conducted experiments focused on **parameter estimation** and explored **deep learning–based motor modeling**, inspired by [1]. The idea was to use an **RNN-based network** to capture motor dynamics and integrate it with the simulator. From our experiments, we observed that this approach did not generalize well for smaller electric motors.

We successfully replicated the motor model and evaluated its accuracy through **inverted pendulum balancing experiments**. However, reinforcement learning controllers continued to produce unpredictable behaviors, so we transitioned to a **trajectory-following–based control** approach. This method led to working prototypes in both simulation and on the physical robot.

In the later stages of the project, I focused on **state estimation** using multiple sensors and experimented with different **Kalman Filter variants**. We found that stereo sensors alone provided reliable state estimates, while **factor graph–based state estimation** achieved higher accuracy at the cost of increased computational requirements.


**Reference:**
[1] _Learning Agile and Dynamic Motor Skills for Legged Robots_ — [https://arxiv.org/abs/1901.08652](https://arxiv.org/abs/1901.08652)

## Demo Videos

### Quadruped Walking in Pybullet Simulation
<video width="100%" style="max-height: 400px; object-fit: contain;" controls>
  <source src="https://r2.eshafeeqe.space/works/ak/akr_with_mm_output.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

### Quadruped Walking in Gazebo simulation
<video width="100%" style="max-height: 400px; object-fit: contain;" controls>
  <source src="https://r2.eshafeeqe.space/works/ak/AQN2iHqg2bEelt2k2QzbTEqTO9ooRBD4hY0oeqBeyQbI4RK_IwWmpsUCtRuvfQTgN-79hGwZomiFnSwG6ZP42_7NLPx12duZy-JH5CT3lXGe3YCBKOZGEi4L6YJupw.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

### Single-leg sim-to-real experiment
<video width="100%" style="max-height: 400px; object-fit: contain;" controls>
  <source src="https://r2.eshafeeqe.space/works/ak/AQN1NV7ZpeYa-7L6W9pBpACLYgYMHXu_1krY1CvmCsHp50g44er4_s04BC8gKpQjlNWvbyKnir-ux8Iie4vvmTwdvzrR6-T9ogrTUkJ4UzRWE7gkB0vhlFJ5zKfvNA.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

### Trajectory tracking real robot
<video width="100%" style="max-height: 400px; object-fit: contain;" controls>
  <source src="https://r2.eshafeeqe.space/works/ak/310487724_6130519273642797_3561071300938507044_n.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

### Real robot walking
<video width="100%" style="max-height: 400px; object-fit: contain;" controls>
  <source src="https://r2.eshafeeqe.space/works/ak/312137181_5643376042423450_501520876576457775_n.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>
