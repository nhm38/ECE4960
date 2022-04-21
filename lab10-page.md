---
layout: page
title: Lab 10
subtitle: Virtual Robot Simulator
show_sidebar: false
---

**Date: April 2022**

The simulator features a map of our classroom with a virtual robot in the map. The linear and angular velocities of the virtual robot can be changed, and the distance from a sensor on the front of the virtual robot can be collected. These functionalities are controlled with the Commander class.

### Open Loop Control

{% include youtube.html video="XQIwzdv9bwU" %}

To execute a square, the robot moves with a linear velocity of 0.1 m/s and an angular velocity of 0 rad/s for 2 seconds. ```cmdr.set_vel(0.1, 0)```

Then the robot turns with a linear velocity of 0 m/s and an angular velocity of 90 deg/s for 1 seconds. ```cmdr.set_vel(0, math.radians(87.5))```

Through trial and error, I determined that due to delays, turning 87.5 degrees (converted to radians) would results in a 90 degree turn in the simulator. I repeated this sequence for 4 times to create a square. 

{% include youtube.html video="EYCkgusiqdA" %}

The controls are not perfectly repeatable and the exact same shape is not executed everytime. As seen in the plotting in the videos, the odometry does not match the ground truth well.



### Closed Loop Control
Design a simple controller in your Jupyter notebook to perform a closed-loop obstacle avoidance.
By how much should the virtual robot turn when it is close to an obstacle?
At what linear speed should the virtual robot move to minimize/prevent collisions? Can you make it go faster?
How close can the virtual robot get to an obstacle without colliding?
Does your obstacle avoidance code always work? If not, what can you do to minimize crashes or (may be) prevent them completely?


{% include youtube.html video="_n7_P5aUJ-Y" %}


To avoid obstacles, I have the robot first backup when it senses it's a certain distance from a wall then turn 60 degrees. When the robot at 0.5 m/s and tries to detect wall from 0.1 m out, the robot crashes.


{% include youtube.html video="WMkLFR6fe2o" %}


{% include youtube.html video="DQXYe8bamNY" %}


I reworked by code to get the pose data and plot it more frequently to attempt to improve the odometry. I saw some improvements, but the odometry still wasn't perfect.

I wrote a python function called readData() that gets the pose and distance sensor data, plots both of the pose data, and returns the odometry pose, ground truth pose, and distance arrays.

```
def readData():
    pose, gt_pose = cmdr.get_pose()    
    cmdr.plot_odom(pose[0], pose[1])
    cmdr.plot_gt(gt_pose[0], gt_pose[1])
    dist = cmdr.get_sensor()
    return pose, gt_pose, dist
```


{% include youtube.html video="727AiS1XOmk" %}


The distance sensor only detects directly in front of the center of the robot. If an obstacle is only partially in the path of the robot, there is a chance the distance sensor will not detect it and instead pick up on something else behind the obstalce. In this case, the robot will run into the obstacle.

