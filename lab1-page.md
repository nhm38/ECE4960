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
1. Installed the Arduino Core for Apollo3 on the Arduino IDE
    - Instructions are [here](https://learn.sparkfun.com/tutorials/artemis-development-with-arduino?_ga=2.30055167.1151850962.1594648676-1889762036.1574524297&_gac=1.19903818.1593457111.Cj0KCQjwoub3BRC6ARIsABGhnyahkG7hU2v-0bSiAeprvZ7c9v0XEKYdVHIIi_-J-m5YLdDBMc2P_goaAtA4EALw_wcB)
    - Hooked up the Artemis Nano to my laptop using the USB-C cable

2. Blink Example

{% include youtube.html video="mfgcybbQz18" %}

3. Serial Monitor Example

{% include youtube.html video="8p_e_DGtcAQ" %} 

4. Analog Read/Temperature Example
    - I changed the example code to only output the raw temperature values for ease of reading
    - I commented out the other print statments and added the following code
```
Serial.printf("Temperature: %d\n", temp_raw);
```
{% include youtube.html video="We0yZWZzr20" %} 

You can see the temperature increase when I wrap my hand around the Artemis Nano and then decrease when it's placed up against the cold window (it was 3&deg;F outside!). 

5. Pulse Density Microphone Example

{% include youtube.html video="j4tkVlPPoTI" %} 

I used an online tone generator on my phone to play a frequnecy at 440 Hz. You can see the microphone picks up a values that's close to that frequency.
