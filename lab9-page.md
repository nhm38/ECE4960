---
layout: page
title: Lab 9
subtitle: Mapping
show_sidebar: false
---

**Date: April 2022**

### Orientation control
If you choose this option, create a PID controller that allows your robot to do on-axis turns in small, accurate increments.
Aim for at least 14 readings (roughly 20 degree increments) per 360 degree rotation.
This option is easier than the other if you have already completed Task B in Lab 6.
This option may also be best if you cannot get your robot to do slow reliable turns. Recall that the ToF sensor will report false outputs if the distance to the object changes too drastically during a reading, but when you sit still you guarantee that the TOF sensor is pointing towards a fixed point in space.
Please quantify and/or use graphs to document that your PID controller works well, and upload a video that shows if your robot turns (roughly) on axis.
Given the drift in your sensor, the size and accuracy of your increments, and how reliably your robot turns on axis, reason about the average and maximum error of your map if you were to do an on-axis turn in the middle of a 4x4m square, empty room.

### Read Out Distances
Consider whether your robot behavior is reliable enough to assume that the readings are spaced equally in angular space, or if you are better off trusting the orientation from integrated gyroscope values.
Sanity check individual turns by plotting them in polar coordinate plot. For simplicity assume that the robot is rotating in place. Do the measurements match up with what you expect?


### Merge and Plot your readings
Compute the transformation matrices and convert the measurements from the distance sensor to the inertial reference frame of the room (these will depend on how you mounted your sensors on the robot.)
Describe the matrices
Plot all of your TOF sensor readings in a single plot. Please assign different colors to data sets acquired from each turn.

### Convert to Line-Based Map
To convert this into a format we can use in the simulator, manually estimate where the actual walls/obstacles are based on your scatter plot. Draw lines on top of these, and save two lists containing the end points of these lines: (x_start, y_start) and (x_end, y_end). In the next lab, we will import these lists into the simulator.

Feel free to correct slight errors found discovered during post processing in this step, but be sure to explain what caused them and how/why you correct them.
