---
layout: page
title: Lab 8
subtitle: Stunts!
show_sidebar: false
---

**Date: April 2022**

### Task B PID Stunt
I started the car XXX meters from the wall. I never got the Kalman Filter to work well so I estimated the distance to start the drift to be XXX mm ( m) through several tests. Because the car is accelerating somewhat unpredictably as it starts from rest and move towards the car, extrapolating the TOF data wasn’t super accurate, so I decided not to use this approach either.

video

### Open loop Stunt
My stunt is to drive up a ramp (made from a binder), make a jump, land, pause, then do a spin to show the car survived!

I started the front of the car 20 inches from the beginning of the ramp.

![Open Loop Stunt Setup img](img/Stunt Setup.JPG) 

{% include youtube.html video="4xegUksMIC0" %}


{% include youtube.html video="43PPz8sH_cU" %}


{% include youtube.html video="wJ60XtU8zdg" %}

Here's the code:
```
  static int spd = 145;

  // Stop
  analogWrite(MD1_xIN1, 0);
  analogWrite(MD1_xIN2, 0);
  analogWrite(MD2_xIN1, 0);
  analogWrite(MD2_xIN2, 0);
  delay(4000);

  //Forward
  analogWrite(MD1_xIN1, spd);
  analogWrite(MD1_xIN2, 0);
  analogWrite(MD2_xIN1, 0);
  analogWrite(MD2_xIN2, 1.3 * spd);
  delay(1000);

  // Stop
  analogWrite(MD1_xIN1, 100);
  analogWrite(MD1_xIN2, 100);
  analogWrite(MD2_xIN1, 100);
  analogWrite(MD2_xIN2, 100);
  delay(1000);

  //Spin CCW
  analogWrite(MD1_xIN1, spd);
  analogWrite(MD1_xIN2, 0);
  analogWrite(MD2_xIN1, spd);
  analogWrite(MD2_xIN2, 0);
  delay(2500);
```

### Blooper ?
