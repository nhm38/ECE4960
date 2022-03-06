---
layout: page
title: Lab 5
subtitle: Motor Drivers & Open Loop Control
show_sidebar: false
---

**Date: February 23rd, 2022**

# Objectives
1. Connect both motor drivers to the RC car motors
2. Use the Artemis Nano to control the mtion of the RC car (open loop)

# Components
- 1 x RC car
- 1 x 3.7V 650mAh Li-Po Rechargeable Battery
- 1 x 3.7V Lipo Battery 850mAh Rechargeable Battery
- 2 x DRV8833 Dual H-Bridge Motor Driver 
- 1 x Artemis Nano

# Procedure
1. According to the motor driver datasheet, the power supply voltage can range between 2.7 V and 10.8 V, and the battery we are using for the motor driver operates in that range (850mAh,3.7 V). I hookedup the first motor driver as follows: ![Motor Driver wiring img](img/lab5/motor_driver_1.jpg) I coupled AIN1 and BIN1 in parallel and AIN2 and BIN2 in parallel. I did the same thing for the respective outputs as well. I put the signal wire through the first pin and soldered it in place. Then I bent the wire and looped it through the other pin and soldered in in place to the put the two pins in parallel. Because the area is so small, I covered the wiring with hot glue to prevent contact. ![Motor Driver Back img](img/lab5/motor_driver_back.JPG) 
2. I attached the first motor driver to pins 4 and A5 on the Artemis, becuase they are both PWM capable. I will attach the second motor driver to pins A6 and A7 on the Artemis, becuase they are also PWM capable. These 4 pins are all next to each other on the board. Next, I wrote code to control the PWM for the motor driver: ![PWM code img](img/lab5/Rising_Falling_PWM_code_IMG.png) First, I checked that the Artemis Nano was actually outputing a PWM signal. I connected the oscilloscope probe to the motor driver input 1 (the green wire). At this point, since I had not yet wired in the second motor driver, I used a breadboard to make it easy to ensure all components were grounded together. Using some additional wires, I grounded the motor driver, the Artemis Nano, and the oscilloscope probe. This is the setup: ![chanel 1 setup img](img/lab5/chanel1_PWM.JPG) This is the output from the Artemis Nano: **VIDEO**. Once I saw that, I added a second channel to the oscilloscope. I connected the probe to respectively output (the light blue wire) and grounded it on the breadboard as well. This is the second setup: ![channel 2 setup img](img/lab5/chanel2_motor_driver_output.JPG) With this setup, the output was this: **VIDEO**.
3. I took apart the RC car ![car img](img/lab5/gutted_robot.JPG) I soldered the two outputs of the motor driver to the leads of one of the car motors.
4. I controlled the motor based on this logic.

  | xIN1    | xIN2    | Function            |
  |---------|---------|---------------------|
  | PWM     | 0 (LOW) | Forward, fast decay |
  | 0 (LOW) | PWM     | Reverse, fast decay |
I set ```analogWrite(MD1_xIN1, 45);``` and ```analogWrite(MD1_xIN2, 0);```, then ```analogWrite(MD1_xIN1, 0);``` and ```analogWrite(MD1_xIN2, 45);``` to go in the opposite direction. I found that at 80, the wheels moved super fast (almost concerningly in this situation). At 15 the wheels didn't move at all, and there was a buzzing noise. At 25 there was some twitching, and there was still buzzing. At 35 the wheels would spin if I gave them an initial push, and there was still some buzzing. At 45, the wheels spun on their own. **VIDEOS**
5. I carefully mateched the positive and negatice terminals of the 650mAh battery and the JST connector, soldered, and added heat shrink for each branch. I used a permanent connection in this case, becuase the Artemis shouldn't draw too much power and can recharge the battery while plugged into a USB ![Battery Charging Artemis img](img/lab5/artemis_bat_charging.JPG)
