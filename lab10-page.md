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

VIDEO 1
{% include youtube.html video="XQIwzdv9bwU" %}

To execute a square, the robot moves with a linear velocity of 0.1 m/s and an angular velocity of 0 rad/s for 2 seconds. `cmdr.set_vel(0.1, 0)`
Then the robot turns with a linear velocity of 0 m/s and an angular velocity of 90 deg/s for 1 seconds. `cmdr.set_vel(0, math.radians(87.5))` Through trial and error, I determined that due to processor delays, turning 87.5 degrees (converted to radians) would results in a 90 degree turn in the simulator.
I repeated this sequence for 4 times. 

VIDEO 2
{% include youtube.html video="EYCkgusiqdA" %}

The controls are not perfectly repeatable and the exact same shape is not executed everytime.



### Closed Loop Control
Design a simple controller in your Jupyter notebook to perform a closed-loop obstacle avoidance.
By how much should the virtual robot turn when it is close to an obstacle?
At what linear speed should the virtual robot move to minimize/prevent collisions? Can you make it go faster?
How close can the virtual robot get to an obstacle without colliding?
Does your obstacle avoidance code always work? If not, what can you do to minimize crashes or (may be) prevent them completely?


The distance sensor only detects directly in front of the center of the robot. If an obstacle is only partially in the path of the robot, there is a chance the distance sensor will not detect it and instead pick up on something else behind the obstalce. In this case, the robot will run into the obstacle.
