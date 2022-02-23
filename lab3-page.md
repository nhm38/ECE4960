---
layout: page
title: Lab 3
subtitle: TOF and IMU sensors
show_sidebar: false
---

**Date: February 9nd, 2022**

# Objectives
1. Figure out the wiring between the TOF sensors, IMU, and Artemis Nano and solder the connections
2. Get the sensors functioning/Test the sensors

# Components
- 1 x SparkFun RedBoard Artemis Nano
- 1 x USB-C to USB-C cable
- 2 x VL53L1X Time of Flight sensors
- 1 x ICM-20948 Inertial Measurement Unit sensor
- Stranded wires
- Soldering iron + solder

# Procedure
- I installed the SparkFun VL53L1X Time of Flight Sensor library & the SparkFun 9DOF IMU Breakout ICM 20948 Arduino Library in the Arduino IDE.
- Thinking about their respective positioning on the RC car, I did all the soldering first.

![Dasiy Chain img](img/lab3/Lab3-daisy-chain.JPG)

- I used the QWIIC-to-cable connecter from the Artemis Nano to the IMU
- Then I daisy chained the first TOF sensor to the IMU
- Finally, I daisy chained the first TOF to the second TOF
    - The second TOF has the XSHUT pit connect to the Artemis nano pin A3.
        
![Soldered Components img](img/lab3/Lab3-soldered-components.jpg)

- I uploaded File->Examples->Apollo3->Example05_Wire_I2C
    - VL53L1X I2C device address: 0x52 = 82 in decimal = 0b1010010 in binary
    - ICM-20948 I2C address: AD0 = 0  0x68 = 104 in decimal = 0b1101000 in binary
        - AD0_VAL is 1 by default unless the ADR jumper is connected. Connecting the I2C ADR jumper changes the default address of the IMU and AD0_VAL becomes 0. In this case the internal jumper is connected, so AD0_VAL should be 0.
        - Becuase I connect the IMU first, I was able to properly detect its address before I connected the 2 TOF sensors

{% include youtube.html video="bHrGxIOisgo" %}

I ran into the issue with printing the addresses when all the sensors are connected.

![Addresses img](img/lab3/Lab3-addresses-all-detected.png)

- I used .setDistanceModeShort(), the short distance mode
    - Short distance mode is less affected by the ambient light conditions, but its maximum ranging distance is
more limited

### VL53L1X TOF Sensor

### Testing Setup

![TOF test setup img](img/lab3/Lab3-tof-test.JPG)

You can see the TOF senor is half a foot from the black and red box, and in the background the Serial Monitor is printing the distance is 0.50 ft.

- I uploaded File->Examples->SparkFun VL53L1X 4m Laser Distance Sensor->Example1_ReadDistance
    - I changed the sketch to also get the ranging time and the presecribed inter-measurement period
    - I removed the vacuum tape from the sensor before taking any measurements
    - The TOF was set at the end of the tape measure, and I moved the black and red box towards and away from the sensor in 1 inch increments
    - 3 trials were performed

### Range, accuracy, repeatability

![TOF data graph](img/lab3/tof_data_IMG.png)

- The sensor has good repeatability even in the short distance mode (where the timing budget can be decreased more than in the other modes)
- The data is linear between 0.5 feet and 2 feet which means the expected and actual measurements match very well. In this section the measurements are therefore accurate.
- The x-offset of the graph shows the minimum discernable distance in practice is around an inch
- I found the maximum distance measured in short distance mode to be 7.71 ft (2.35 m) when the actual distance was 8 ft. After going past 8ft, the measured distance registered as 0.00 ft. The TOF sensor is not accurate at this distance.

### Ranging time
- I used the .getIntermeasurementPeriod() method
  - The specified inter-measurement period (in milliseconds) determines how frequently the sensor takes a measurement
  - If it's shorter than the timing budget, the sensor will start a new measurement as soon as the previous one finishes
- According to the datasheet, the default timing budget is 100 ms
  - It's possible to decrease it to 20 ms only in short distance mode, but this affects repeatability
  - Increasing the timing budget increases the maximum range the device can measure and improves the repeatability error, but the power consumption increases as well
- I also manually calculated the elapsed time using millis() and taking the difference of the time just after ranging starts and just after ranging stops

{% include youtube.html video="dusu5DqSAVc" %}

As I take measurements as different distances, the calculated time and period are constant

### Different colors and textures
I used my hand which has an irregular shape, a lamp to change the amount of ambient light, and a bright green notebook which has a different surface texture and color to test the TOF sensor. I didn’t see any significant changes to the results.

### ICM 20948 IMU Sensor
- I uploaded File -> Examples -> SparkFun 9DoF IMU Breakout – ICM 20948 – Arduino Library -> Arduino -> Example1_Basics

![IMU works img](img/lab3/Lab3-imu-functioning.png)

- Next, I changed the sketch to only output the data I needed for each test
  - I also looked at the difference between the raw IMU data vs the scaled IMU data.

### Raw IMU Data
![IMU works img](img/lab3/Lab3-imu-raw-data.png)

### Scaled IMU Data
![IMU works img](img/lab3/Lab3-imu-scaled-data.png)

- The scaled data looks less noisy

This video shows the x, y, and z components from the accelerometer and gyroscope.
{% include youtube.html video="hk7wbxJ4Oho" %}

The accleration ouputs are in units of milli g's. That is why the y acceleration (the green line) is offset from zero by 1000. You can see the 6 different line plots responsd appropriately to the movement of the sensor.

{% include youtube.html video="J-Pxo03Zcy4" %}
The accelerometer is pretty accurate. Both the pitch and roll changed by -90 and +90 degrees according to the tilting of the sensor.

### Frequency Response

{% include youtube.html video="w4fQdkF16sE" %}

I saved the data from the Serial Monitor and used the following MATLAB script to plot the time and frequency domains.
![matlab img](img/lab3/lab3-fft-matlab.png)

![Time Plot img](img/lab3/lab3_time_domain_IMG.png)

![Freq Plot img](img/lab3/lab3_freq_domain_IMG.png)

I was tapping at about 240 beats per minute (bpm) which is equivalent to about 4 Hz. Around 4 Hz there is a peak with an amplitude of 8. The sensor seems to be pretty noisy. I performed the following calculation to determine the Low Pass Filter parameters.

![Calculations img](img/lab3/lab3-alpha-calc.png)

I used this alpha in my code to find the pitch. Increasing f_c, the cutoff frequency, increases RC which in turn increases the value of alpha and makes a more aggressive low pass filter.

### Implementation of the Low Pass Filter

{% include youtube.html video="I-S_GIImWKY" %}

The low pass filter smooths out the data by removing some of the noise. I don't see significant lag with my filter. It follows the accelerometer pitch well.

Here are two close up images to clearly see how the low pass filter is changing the output:
![IMU LPF img](img/lab3/Lab3-LPF1.png)

![IMU LPF img](img/lab3/Lab3-LPF2.png)
There is less jaggedness due to noise and less response to big jumps.

### Gyroscope

{% include youtube.html video="7lgcWMfqQpw" %}

{% include youtube.html video="4Fj0Cngpv8k" %}
