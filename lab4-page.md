---
layout: page
title: Lab 4
subtitle: Characterize the RC Car
show_sidebar: false
---

**Date: February 16th, 2022**

# Objectives
1. Learn about the capabilities of the car and measure particular aspects of the car that will be useful in future labs

# Components
- 1 x RC car
- 1 x Manual Remote Controller
- 2 x 3.7V 650mAh Li-Po Rechargeable Batteries

# Procedure
## Test 1
### Dimensions of the RC car
This informantion is good for pose data, used to determine dynamics, and necessary for controls.

Wheel diameter: 3 1/8” = 7.9375cm (from the outer tread across to the outer tread)

Wheel thickness: 1 1/8” = 2.8575 cm

Body length: 6 3/8” = 16.1925 cm

Body width (@ the center): 3” = 7.62 cm

Wheel separation: 3.5” = 8.89 cm (distance from inner side of left wheel to inner side of right wheel)

Ground clearance: 9/16” = 1.4288 cm

![Ground clearance img](img/lab4/Lab4-clearance.JPG)

I used this scrap of paper to find the clearance. I changed the height of the tab and passed it under the car until it brushed the underside. Then I measured the length of the tab.


![RC car Dimensions img](img/lab4/Lab4-car-dim-top.png)
![RC car Dimensions img 2](img/lab4/Lab4-car-dim-side.png)

## Test 2
### Battery Life
The 650mAh Li-Po batteries provide about 8-10 minutes of moving time to the RC car. This is good to know for future operations. No function or action can take more than this amount of time to execute or perform tasks.

{% include youtube.html video="-s6r4KL3jfQ" %}

When the battery dies, it can no longer power the motors, so the wheels start to shutter and eventually stop moving altogether.

## Test 3
### Charging
The 650mAh Li-Po batteries take about 90 minutes to fully charge after being fully depleted (1-2hours).

## Test 4
### Drift
While attempting to manually drive in a straight line, the RC car tends to drift to the RC car'sits left. The car was driving on smooth hard wood floors. I checked the levelness of the surface using an app on my iPhone.

Surface Levelness at the starting point
![Start Surface Level](img/lab4/Lab4-start.jpg)

Surface Levelness at the middle point
![Middle Surface Level](img/lab4/Lab4-middle.jpg)

Surface Levelness at the end point
![End Surface Level](img/lab4/Lab4-end.jpg)

Overall, my floor is very level, so that will not influence the motion of the RC car.

### Driving Forward Slowly
{% include youtube.html video="Z0arA0mIfuk" %}

Driving slow (impluse control inputs by repeatedly pressing the drive forward button) caused a drift of 1 foot and 6 inches (45.72 cm) to the left 10 ft from the RC car's starting point.

This is a still taken from a different angle
![Slow Drift](img/lab4/Lab4-test4-slow.JPG)


{% include youtube.html video="L2lmgklOwTc" %}

Driving fast (continuously holding down the drive forward button) caused a drift of 1 foot and 8 7/8 inches (53.0225 cm) to the left 9 ft from the RC car's starting point.

This is a still taken from a different angle
![Fast Drift](img/lab4/Lab4-test4-fast.JPG)




