---
layout: page
title: Lab 12
subtitle: Bayes Filter Localization (Real)
show_sidebar: false
---

**Date: April 20, 2022**


### Localization Test in Simulation
The green path is the truth pose, the blue path is the localization, and the red path is the odometry which is just as bad as always.
![Simulator Loc test](img/lab12/sim test.png)


### Using a uniform prior on the pose
Run (only) the update step using the sensor measurement data to localize your robot.

Go through the notebook lab12_real.ipynb and implement the member function perform_observation_loop of class RealRobot (re-use code from previous labs to implement this).

### Place your robot in one of the four marked poses and run the update step of the Bayes filter once.
How close is the localized pose w.r.t to the ground truth?

Visualize your results.

Discuss your results.

Repeat for every marked position.

Does the robot localize better in certain poses? If so, why?

### Marked Poses
(-3 ft ,-2 ft ,0 deg)

(0 ft,3 ft,0 deg)

(5 ft,-3 ft,0 deg)

(5 ft,3 ft,0 deg)

