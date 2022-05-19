---
layout: page
title: Lab 13
subtitle: Path Planning & Execution
show_sidebar: false
---

**Date: May, 2022**

So far in this class, I have implemented PID feedback control, mapping, and localization algorithms. The goal of this lab is to use whatever bits and pieces of knowledge and the skills learned in this class to get the real robot to visit 9 waypoints. 

### First Attempt: Dead Reckoning 
Towards the beginning of this class we talked about dead reckoning in lecture. Dead reckoning is the process of estimating your current position by using a previous position and incorporating velocity and/or angular orientation data over time. Therefore, the IMU sensors are potentially a good tool to perform dead reckoning. I wanted to try to use dead reckoning with the accelerometer even though in class we talked about the downsides, because we hadn't implemented it in class yet. Dead reckoning is extremely suseptible to accumulation errors especially with a noisy sensor like the accelerometer. I definitely saw this issue pop up and this ultimately was the reason I decided to move onto a new method for executing the path.

My original idea was to use PID feedback control on the orientation for making turns and ensuring I was driving in a straight line (not drifting). 


![Sad DR](img/lab13/DeadReckoning.png)

```
double acc_LPF[] = {0, 0};
double vel[] = {0, 0};
double pos[] = {0, 0};
const int n = 1;


acc = (myICM.accX() / 1000 * 9.807); //convert units from mg > g > m/s^2

// Low Pass Filter Accelertation
const float a = 0.15;
acc_LPF[n] = a * acc + (1 - a) * acc_LPF[n - 1];
acc_LPF[n - 1] = acc_LPF[n];

// Velocity
vel[n] = vel[n - 1] + acc_LPF[n - 1] * dt;

// Distance/Position
pos[n] = pos[n - 1] + vel[n - 1] * dt;

// Update
acc_LPF[n - 1] = acc_LPF[n];
vel[n - 1] = vel[n];
pos[n - 1] = pos[n];
```

I also set up the built in digital low pass filter to see if that would improve the data. I messed abournd with some of the settings and saw some improvemnts in the acceleration data but nothing too significant.

```
ICM_20948_dlpcfg_t myDLPcfg;    // Configuration structure for the desired sensors (accel & gyro)

myDLPcfg.a = acc_d111bw4_n136bw; // (ICM_20948_ACCEL_CONFIG_DLPCFG_e)
myDLPcfg.g = gyr_d361bw4_n376bw5; // (ICM_20948_GYRO_CONFIG_1_DLPCFG_e)

myICM.setDLPFcfg((ICM_20948_Internal_Acc | ICM_20948_Internal_Gyr), myDLPcfg);

// Choose whether or not to use DLPF
ICM_20948_Status_e accDLPEnableStat = myICM.enableDLPF(ICM_20948_Internal_Acc, true);
ICM_20948_Status_e gyrDLPEnableStat = myICM.enableDLPF(ICM_20948_Internal_Gyr, true);
```

Ultimately, I could not overcome the sensor noise and error accumulation.


### Second Attempt: Feedback Control with PID
My PID controller worked super well in previous labs. It generally reliable and super fast, especially the angular orientation control.

### Third Attempt: Feedback Control with PID (no angular control for straight line movement)

### Path Plan

![Map & Plan](img/lab13/Path Plan.png)

### Easy to implement Localization as in Lab 12. I wanted to focus more on Navigation. 
