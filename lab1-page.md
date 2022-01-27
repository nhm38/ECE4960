---
layout: page
title: Lab 1
subtitle: Artemis
show_sidebar: false
---

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

![blink ex video](https://user-images.githubusercontent.com/98340266/151292144-77544abc-2cc7-46c7-872c-c3a6bcf32efe.mp4)



3. Serial Monitor Example

![serial ex](https://user-images.githubusercontent.com/98340266/151291882-91457c74-6c64-406c-bb5a-efd955dd0a85.mp4)



4. Analog Read/Temperature Example
    - I changed the example code to only output the raw temperature values for ease of reading
    - I commented out the other print statments and added the following code
```
Serial.printf("Temperature: %d\n", temp_raw);
```

![temperature ex](https://user-images.githubusercontent.com/98340266/151291895-f44fc38d-c176-4885-a475-a60c07037c91.mp4)

You can see the temperature increase when I wrap my hand around the Artemis Nano and then decrease when it's placed up against the cold window (it was 3&deg;F outside!). 


5. Pulse Density Microphone Example

![microphone ex](https://user-images.githubusercontent.com/98340266/151292161-b8c59eb3-3694-46fc-8972-6b8daca4a644.mp4)

I used an online tone generator on my phone to play a frequnecy at 440 Hz. You can see the microphone picks up a values that's close to that frequency.
