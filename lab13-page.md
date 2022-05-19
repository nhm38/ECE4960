---
layout: page
title: Lab 13
subtitle: Path Planning & Execution
show_sidebar: false
---

**Date: May, 2022**

### First Attempt: Dead Reckoning 

I want to try to use dead reckoning with the accelerometer (even though in class we talked about these disadvantages: )
And use PID on the orientation control for both the turning and making sure i drive in a straight line.

Follow a series of command: drive straight X1 distance, turn Y1 degrees, repeat through commands list until last point.

drive straight -- find the distance using the accelerometer and dead reckoning
turn -- find the angle using gyroscope, control with PID already implemented

![Sad DR](img/lab13/DeadReckoning.png)
### Second Attempt: Feedback Control with PID

### Third Attempt: Feedback Control with PID (no angular control for straight line movement)

### Path Plan

![Map & Plan](img/lab13/Path Plan.png)

### Easy to implement Localization as in Lab 12. I wanted to focus more on Navigation. 
