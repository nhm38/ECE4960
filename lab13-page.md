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

I also set up the built in digital low pass filter to see if that would improve the data.

```
ICM_20948_dlpcfg_t myDLPcfg;    // Configuration structure for the desired sensors (accel & gyro)

myDLPcfg.a = acc_d111bw4_n136bw; // (ICM_20948_ACCEL_CONFIG_DLPCFG_e)
myDLPcfg.g = gyr_d361bw4_n376bw5; // (ICM_20948_GYRO_CONFIG_1_DLPCFG_e)

myICM.setDLPFcfg((ICM_20948_Internal_Acc | ICM_20948_Internal_Gyr), myDLPcfg);

// Choose whether or not to use DLPF
ICM_20948_Status_e accDLPEnableStat = myICM.enableDLPF(ICM_20948_Internal_Acc, true);
ICM_20948_Status_e gyrDLPEnableStat = myICM.enableDLPF(ICM_20948_Internal_Gyr, true);
```
### Second Attempt: Feedback Control with PID

### Third Attempt: Feedback Control with PID (no angular control for straight line movement)

### Path Plan

![Map & Plan](img/lab13/Path Plan.png)

### Easy to implement Localization as in Lab 12. I wanted to focus more on Navigation. 
