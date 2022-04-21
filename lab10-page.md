---
layout: page
title: Lab 10
subtitle: Virtual Robot Simulator
show_sidebar: false
---

**Date: April 2022**

Include a brief description of the simulator and its functionalities in your lab report.

### Open Loop Control
Make your robot follow a set of velocity commands to execute a “square” loop anywhere in the map.
Plot and analyze the ground truth and odometry of the robot.
What is the duration of a velocity command?
Does the robot always execute the exact same shape?

### Closed Loop Control
Design a simple controller in your Jupyter notebook to perform a closed-loop obstacle avoidance.
By how much should the virtual robot turn when it is close to an obstacle?
At what linear speed should the virtual robot move to minimize/prevent collisions? Can you make it go faster?
How close can the virtual robot get to an obstacle without colliding?
Does your obstacle avoidance code always work? If not, what can you do to minimize crashes or (may be) prevent them completely?
