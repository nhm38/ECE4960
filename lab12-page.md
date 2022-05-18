---
layout: page
title: Lab 12
subtitle: Bayes Filter Localization (Real)
show_sidebar: false
---

**Date: May, 2022**


### Localization Test in Simulation
Before implementing on the real robot, I verified the Bayes Filter simulation worked again. The green path is the truth pose, the blue path is the localization, and the red path is the odometry which is just as bad as always. HOwever, as you can see, the belief of the localization is very similar to the truth pose.


![Simulator Loc test](img/lab12/sim test.png)


### Implementing the perform_observation_loop function of the RealRobot class
The perform observation loop function does a 360 degree turn with the real robot similar to what was done in the simulator.  The robot collects 18 sensor readings as defined in the observation count variable in the world.yaml file. Therefore, I start by creating two numpy column arrays with 18 elements. The next three variables are used to handle and process the data from the Artemis. Then I send the command to actaully take the measurements. I reused the code from Lab 9, the mapping lab. Becuase I want 18 measurement and 360/18 = 20, I know I need to increase the angle by 20 degrees after each measurement. I pass 20 with the map command, so the Artemis knows to increase the yaw/angle setpoint by 20 degrees each time. 
In the previous labs, I was never able to reliably get notification handlers to work due to issues with Windows 11 and using a VM. However, I was able to develop a very reliable method for my purposes. On Artemis, after the robot executes all the commands while collecting the data into arrays, I loop through the arrays and package all the corresponding data to be sent as a string. Between each loop I have a delay to allow time for the information to be sent over bluetooth. After all the collected data is sent, I send a string that says "done". On the python side, as you can see, I have a while loop the constantly receives the data from the Artemis and appends it to an array with all the data. When it gets the "done" string, the while loop is exited. Then I can process the data however I need to. This method has worked well for me in past labs and in this lab. 

I get rid of the "done" string from my data array, becuase I don't want to return that to the rest of the localization algorithm. Sometimes because of the method I use to transmit the data, duplicates of the data are received, so first of the three for loops determines how many instances there are of repeated data. The next for loop actually gets rid of the repeated data while taking into account the changing size of the array so I don't get any index out of bound errors. The last for loop splits the Anglur position data and the distance measurement data which I separated on the Artemis side with a comma. Then the data is appropriately converted and added to the two array which are returned by the function.


```
def perform_observation_loop(self, rot_vel=120):
    """
    Perform the observation loop behavior on the real robot, where the robot does  
    a 360 degree turn in place while collecting equidistant (in the angular space) sensor
    readings, with the first sensor reading taken at the robot's current heading. 
    The number of sensor readings depends on "observations_count"(=18) defined in world.yaml.

    Keyword arguments:
        rot_vel -- (Optional) Angular Velocity for loop (degrees/second)
                    Do not remove this parameter from the function definition, even if you don't use it.
    Returns:
        sensor_ranges   -- A column numpy array of the range values (meters)
        sensor_bearings -- A column numpy array of the bearings at which the sensor readings were taken (degrees)
                           The bearing values are not used in the Localization module, so you may return a empty numpy array
    """

    sensor_ranges = np.zeros((18,1))
    sensor_bearings = np.zeros((18,1))

    count = 0

    data = []
    s = " "

    ble.send_command(CMD.MAP, "20") # |increment on set pt (deg) |

    while not (s == "done"):
        s = ble.receive_string(ble.uuid['RX_STRING'])
        data.append(s)    

    print(data)
    del data[-1]

    # Find how many duplicate measurements there are in the received data
    for i in range(0, len(data)-1):
        if data[i] == data[i+1]:
            count=count+1

    # Get rid of duplicate data
    for j in range(0, len(data)-1-count):
        if data[j] == data[j+1]:        
            del data[j]

    # Split the Yaw data from the ToF data, convert ToF units
    for k in range(0,18):
        vals = data[k].split(",")
        sensor_ranges[k] = float(vals[1])/1000
        sensor_bearings[k] = float(vals[0])

    return sensor_ranges, sensor_bearings
```


### Results
I placed the robot on each of the four marked poses. After running the update step of the Bayes Filter using the sensor measurement data to localize with a uniform prior on the pose, these are the results.

Qs:
How close is the localized pose w.r.t to the ground truth?

Visualize your results.

Discuss your results.

Repeat for every marked position.

Does the robot localize better in certain poses? If so, why?

#### (-3 ft, -2 ft) // (-0.914 m, 0.610 m)
Update Step Time: 0.005 secs

Belief: (-0.914, -0.610, 170.000)

![Plot 1](img/lab12/plot_-3_-2.png)

#### (0 ft, 3 ft) // (0 m, 0.914 m)
Update Step Time: 0.006 secs

Belief: (0.000, 0.914, 150,000)

![Plot 2](img/lab12/plot_0_3.png)

#### 5 ft, -3 ft) // (1.524 m, -0.914 m)
Update Step Time: 0.011 secs
Belief: (-1.219, 0.000, -30.000)

![Plot 3](img/lab12/plot_5_-3.png)


Update Step Time: 0.015 secs
Belief: (1.219, -0.914, -10.000)

![Plot 4](img/lab12/plot_4_-3.png)

#### (5 ft, 3 ft) // (1.524 m, 0.914 m)
Update Step Time: 0.004 secs
Belief: (1.524, 0.914, -10.000)

![Plot 5](img/lab12/plot_5_3.png)

