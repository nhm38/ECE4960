---
layout: page
title: Lab 1
subtitle: Artemis Nano
show_sidebar: false
---

**Date: January 26th, 2022**

# Objectives
1. Set up the SparkFun RedBoard Artemis Nano
2. Complete some basic tasks to get familiar with and ensure proper functionality of the Artemis Nano

# Components
- 1 x SparkFun RedBoard Artemis Nano
- 1 x USB-C to USB-C cable

# Procedure
- I installed the Arduino Core for Apollo3 on the Arduino IDE
    - Instructions for the installation are [here](https://learn.sparkfun.com/tutorials/artemis-development-with-arduino?_ga=2.30055167.1151850962.1594648676-1889762036.1574524297&_gac=1.19903818.1593457111.Cj0KCQjwoub3BRC6ARIsABGhnyahkG7hU2v-0bSiAeprvZ7c9v0XEKYdVHIIi_-J-m5YLdDBMc2P_goaAtA4EALw_wcB)
    - Then I connected the Artemis Nano to my laptop using the USB-C cable

- **Blink Example**
    - I uploaded File > Examples > 01.Basics > Blink from the Arduino IDE to the Artemis Nano which turns the LED on and off for 1 second.

{% include youtube.html video="mfgcybbQz18" %}

- **Serial Monitor Example**
    - I uploaded File > Examples > Apollo3 > Example04_Serial which prints 9 lines of formatted strings, and then I typed a test phrase into the serial monitor to demonstrate the "echo" function.

{% include youtube.html video="8p_e_DGtcAQ" %} 

- **AnalogRead/Temperature Example**
    - I uploaded File > Examples > Apollo3 > Example02_AnalogRead.
    - I changed the example code to only output the raw ADC (analog to digital converter) counts from the temperature sensor for ease of reading the output
    - I commented out the other print statments and added the following code
```
Serial.printf("Temperature: %d\n", temp_raw);
```
{% include youtube.html video="We0yZWZzr20" %} 

You can see the temperature increase when I put my hand around the Artemis Nano and then decrease when it's placed up against the cold window (it was 3&deg;F outside!). 

- **Pulse Density Microphone Example**
    - I uploaded File > Examples > PDM > Example1_MicrophoneOutput.

{% include youtube.html video="j4tkVlPPoTI" %} 

I used an [online tone generator](https://onlinetonegenerator.com/) on my phone to play a frequency at 440 Hz. You can see the microphone picks up a value that's close to that frequency.
