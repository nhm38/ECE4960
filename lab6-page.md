---
layout: page
title: Lab 6
subtitle: PID Control -- Task B: Drift
show_sidebar: false
---

**Date: March 2022**

In this lab I will go through the steps to implement PID control in the car.

### Maximum Angular Speed
video
{% include youtube.html video="ETLwUZWXBHg" %}

I tested the car's max angular speed by ramping up then down the PWM value while driving one side of the wheels foward and the other side in reverse. Initially the max degrees per second (dps) was set to 1000 thinkkng that would be high enough to capture the data. In the first test you can see the saturation of the sensor at 1000. I increased the max dps to 2000 which is the maximum detectable angular rate of this sensor.

![ang spd 1 img](img/lab6/Ang Spd t1.png) ![ang spd 2 img](img/lab6/Ang Spd t2.png) ![ang spd 3 img](img/lab6/Ang Spd t3.png) 

### Data Collection Frequency

### Deadband

### Integrator Wind up

### Low Pass Filter

### Derivative Kick

### Tuning the PID controller

### No Drift

### Drift
