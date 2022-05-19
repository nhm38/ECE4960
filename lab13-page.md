---
layout: page
title: Lab 13
subtitle: Path Planning & Execution
show_sidebar: false
---

**Date: May, 2022**

So far in this class, I have implemented PID feedback control, mapping, and localization algorithms. The goal of this lab is to use whatever bits and pieces of knowledge and the skills learned in this class to get the real robot to visit 9 waypoints. 

### First Attempt: Dead Reckoning 
Towards the beginning of this class we talked about dead reckoning in lecture. Dead reckoning is the process of estimating your current position by using a previous position and incorporating velocity and/or angular orientation data over time. Therefore, the IMU sensor is potentially a good tool to perform dead reckoning. I wanted to try to use dead reckoning with the accelerometer even though in class we talked about its downsides because we hadn't implemented it in class yet. Dead reckoning is extremely suseptible to accumulation errors especially with a noisy sensor like the accelerometer. I definitely saw this issue pop up and this ultimately was the reason I decided to move onto a new method for executing the path.

My original idea was to use PID feedback control on the orientation for making turns and ensuring I was driving in a straight line (not drifting). Since the robot would be moving in a straight line, I thought doing dead reckoning would have less accuracy issues than doing dead reckoning along a curved path.

The following are parts of the code I wrote to compute the dead reckoning. I essentailly integrate the acceleration to get the velocity and then integrate the velocity to get the position.

```
double acc_LPF[] = {0, 0};
double vel[] = {0, 0};
double pos[] = {0, 0};
const int n = 1;


dt = (micros() - prev_t) / 1000000.0;
prev_t = micros();


acc = (myICM.accX() / 1000 * 9.807); //convert units from mg > g > m/s^2

// Low Pass Filter Accelertation
const float a = 0.15;
acc_LPF[n] = a * acc + (1 - a) * acc_LPF[n - 1];

// Velocity
vel[n] = vel[n - 1] + acc_LPF[n - 1] * dt;

// Distance/Position
pos[n] = pos[n - 1] + vel[n - 1] * dt;

// Update
acc_LPF[n - 1] = acc_LPF[n];
vel[n - 1] = vel[n];
pos[n - 1] = pos[n];
```

The blue curve is **acceleration**, the red curve is **velocity**, and the greed curve is **position**. The velocity and position curves are in units of cm/s and cm for ease of visualization.

![Sad DR](img/lab13/DeadReckoning.png)

I pushed the robot forward once, pulled the robot backwards once, and then pushed the robot forward again. You can see the 3 bumps in the velocity data and the corresponding changes in the acceleration data.

I set up the sensor's built-in digital low pass filter to see if that would improve the data. I messed abournd with some of the settings and saw some improvemnts in the acceleration data but nothing too significant. The acceleration curves look smoother, but the velocity curve (and postion curve) are still accumulating a lot of error.

```
ICM_20948_dlpcfg_t myDLPcfg;    // Configuration structure for the desired sensors (accel & gyro)

myDLPcfg.a = acc_d111bw4_n136bw; // (ICM_20948_ACCEL_CONFIG_DLPCFG_e)
myDLPcfg.g = gyr_d361bw4_n376bw5; // (ICM_20948_GYRO_CONFIG_1_DLPCFG_e)

myICM.setDLPFcfg((ICM_20948_Internal_Acc | ICM_20948_Internal_Gyr), myDLPcfg);

// Choose whether or not to use DLPF
ICM_20948_Status_e accDLPEnableStat = myICM.enableDLPF(ICM_20948_Internal_Acc, true);
ICM_20948_Status_e gyrDLPEnableStat = myICM.enableDLPF(ICM_20948_Internal_Gyr, true);
```

![Sad DR](img/lab13/PostDLPF.png)


Another fix I tried was to just get rid of the steadily increasing part of the velocity curve. You can see the bumps in the previous two graph were I move the robot, so it did have some non-zero velocity. I estimated the slope of the non-peaked portions of the graph. Then if the change in velocity was less than or equivalent to the slope I set the velocity equal to zero.

```
if ( abs(vel[n] - vel[n - 1]) <= 0.001) {
  vel[n] = 0;
}
```
This didn't end up working as intended. When the acceleration starts to decrease, so the peak of the velocity bumps, the velocity is small enough to be in the range of the slope. This causes weird behaviors.

![Sad DR](img/lab13/PostVelFix.png)


Ultimately, I could not overcome the sensor noise and error accumulation. I probably shouldn't have spent so much time trying to get this to work, but I was really interested in the implementation. I think with a better IMU sensor and accelerations that were larger and not so breif, it could be possible to effectively implement dead reckoning as a reasonble means to determine position in this type of situation.


### Second Attempt: Feedback Control with PID
My PID controller worked super well in previous labs. It generally reliable and super fast, especially the angular orientation control.

The sampling rate was too slow when also doing PID contorl on the angular orientation in the loop, so I wasn't able to get ToF data frequently enough to stop at an accurate position.

### Third Attempt: Feedback Control with PID (no angular control for straight line movement)
In previous labs my system functioned fine moving in a straight line by just multiplying the left wheel speed by a calibration factor. To speed up the loop execution speed I eliminated the PID control on angular orientation and only did position control with respect to the ToF data. With this method I was able to stop more accurately and was able to travel distances I specified.

### Path Plan

![Map & Plan](img/lab13/Path Plan.png)

### Easy to implement Localization as in Lab 12. I wanted to focus more on Navigation. 
