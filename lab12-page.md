---
layout: page
title: Lab 12
subtitle: Bayes Filter Localization (Real)
show_sidebar: false
---

**Date: May, 2022**


### Localization Test in Simulation
Before implementing on the real robot, I verified the Bayes Filter simulation worked again. The green path is the truth pose, the blue path is the localization, and the red path is the odometry which is just as bad as always. HOwever, as you can see, the belief of the localization is very similar to the truth pose.


![Simulator Loc test](img/lab12/sim test.png)


### Implementing the perform_observation_loop function of the RealRobot class


### Results
I placed the robot on each of the four marked poses. After running the update step of the Bayes Filter using the sensor measurement data to localize with a uniform prior on the pose, these are the results I got.

Qs:
How close is the localized pose w.r.t to the ground truth?

Visualize your results.

Discuss your results.

Repeat for every marked position.

Does the robot localize better in certain poses? If so, why?

#### (-3 ft, -2 ft) // (-0.914 m, 0.610 m)

![Plot 1](img/lab12/plot(-3,-2).png)

#### (0 ft, 3 ft) // (0 m, 0.914 m)

![Plot 2](img/lab12/(plot(0,3).png)

#### 5 ft, -3 ft) // (1.524 m, -0.914 m)

![Plot 3](img/lab12/plot(5,-3)_actual(-4,0).png)

![Plot 4](img/lab12/plot(4,-3).png)

#### (5 ft, 3 ft) // (1.524 m, 0.914 m)

![Plot 5](img/lab12/plot(5,3).png)

