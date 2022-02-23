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

### TOF Sensors Testing Setup

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


- I uploaded File -> Examples -> SparkFun 9DoF IMU Breakout – ICM 20948 – Arduino Library -> Arduino -> Example1_Basics

![IMU works img](img/lab3/Lab3-imu-functioning.png)

- I changed the sketch to only output the data I needed for each test
  - I also did a test on the difference between the raw IMU data vs the scaled IMU data.

### Raw IMU Data
![IMU works img](img/lab3/Lab3-imu-raw-data.png)

### Scaled IMU Data
![IMU works img](img/lab3/Lab3-imu-scaled-data.png)

- The scaled data looks less noisy

9. Altered the sketch AGAIN -> use imu_test4. Check out the change in sensor values as you rotate, flip, and accelerate the board. Explain what you see in both acceleration and gyroscope data.

{% include youtube.html video="hk7wbxJ4Oho" %}

*** units is milli g ***
10. Accelerometer: Use the equations from class to convert accelerometer data into pitch and roll.
 - Show the output at {-90, 0, 90} degrees pitch and roll. Hint: You can use the surface and edges of your table as guides to ensure 90 degree tilt/roll.

{% include youtube.html video="J-Pxo03Zcy4" %}

  - How accurate is your accelerometer? You may want to do a two-point calibration (i.e. measure the output at either end of the range, and calculate the conversion factor such that the final output matches the expected output).

11. Try tapping the sensor and plot the frequency response. What frequency spectrum does the “unwanted noise” have?

{% include youtube.html video="w4fQdkF16sE" %}

![Time Plot img](img/lab3/lab3_time_domain_IMG.png)

![Freq Plot img](img/lab3/lab3_freq_domain_IMG.png)

12. Use these measurements to guide your choice of a complimentary low pass filter cut off frequency (recall implementation details from the lecture. Discuss how the choice of cut off frequency affects the output.

{% include youtube.html video="I-S_GIImWKY" %}

![IMU LPF img](img/lab3/Lab3-LPF1.png)

![IMU LPF img](img/lab3/Lab3-LPF2.png)

13. Gyroscope: Use the equations from class to compute pitch, roll, and yaw angles from the gyroscope.
  - Compare your output to the pitch and roll values from the accelerometer and the filtered response. Describe how they differ.

{% include youtube.html video="7lgcWMfqQpw" %}

- Try adjusting the sampling frequency to see how it changes the accuracy of your estimated angles.
- Use a complimentary filter to compute an estimate of pitch and roll which is both accurate and stable. Demonstrate its working range and accuracy, and that it is not susceptible to drift or quick vibrations.

{% include youtube.html video="4Fj0Cngpv8k" %}



