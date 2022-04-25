---
layout: page
title: Lab 11
subtitle: Bayes Filter (Simulator)
show_sidebar: false
---

**Date: April 20, 2022**

Add in my code explanations

### Compute Control
```
def compute_control(cur_pose, prev_pose):
    """ Given the current and previous odometry poses, this function extracts
    the control information based on the odometry motion model.

    Args:
        cur_pose  ([Pose]): Current Pose
        prev_pose ([Pose]): Previous Pose 

    Returns:
        [delta_rot_1]: Rotation 1  (degrees)
        [delta_trans]: Translation (meters)
        [delta_rot_2]: Rotation 2  (degrees)
    """
    dx = cur_pose[0] - prev_pose[0]
    dy = cur_pose[1] - prev_pose[1]
    ang = math.degrees( math.atan2(dy, dx) ) # atan2 return from -pi to pi --> convert to deg
    
    delta_rot_1 = mapper.normalize_angle(ang - prev_pose[2])
    delta_trans = math.hypot(dx, dy)
    delta_rot_2 = mapper.normalize_angle(prev_pose[2] - ang)
    
    return delta_rot_1, delta_trans, delta_rot_2
```

### Odometry Motion Model
```
def odom_motion_model(cur_pose, prev_pose, u):
    """ Odometry Motion Model

    Args:
        cur_pose  ([Pose]): Current Pose
        prev_pose ([Pose]): Previous Pose
        u (rot1, trans, rot2) (float, float, float): A tuple with control data in the format 
                                                   format (rot1, trans, rot2) with units (degrees, meters, degrees)


    Returns:
        prob [float]: Probability p(x'|x, u)
    """
    exp_u = compute_control(cur_pose, prev_pose)
        
    p_r1 = loc.gaussian(mapper.normalize_angle(exp_u[0]-u[0]), 0, loc.odom_rot_sigma)
    p_t = loc.gaussian(mapper.normalize_angle(exp_u[1]-u[1]), 0, loc.odom_trans_sigma)
    p_r2 = loc.gaussian(mapper.normalize_angle(exp_u[2]-u[2]), 0, loc.odom_rot_sigma)

    prob = p_r1*p_t*p_r2
    
    return prob
```

### Prediction Step
```
def prediction_step(cur_odom, prev_odom):
    """ Prediction step of the Bayes Filter.
    Update the probabilities in loc.bel_bar based on loc.bel from the previous time step and the odometry motion model.

    Args:
        cur_odom  ([Pose]): Current Pose
        prev_odom ([Pose]): Previous Pose
    """
    u = compute_control(cur_odom, prev_odom)
    
    for x_prev in range(mapper.MAX_CELLS_X):
        for y_prev in range(mapper.MAX_CELLS_Y):
            for a_prev in range(mapper.MAX_CELLS_A):
                
                if (loc.bel[x_prev,y_prev,a_prev] > 0.00001):
                    
                    for x in range(mapper.MAX_CELLS_X):
                        for y in range(mapper.MAX_CELLS_Y):
                            for a in range(mapper.MAX_CELLS_A):
                                cur_pose = mapper.from_map(x,y,a)
                                prev_pose = mapper.from_map(x_prev,y_prev,a_prev)
                                loc.bel_bar[x,y,a] = loc.bel_bar[x,y,a] + odom_motion_model(cur_pose, prev_pose, u) * loc.bel[x_prev,y_prev,a_prev]
                                
    loc.bel_bar = loc.bel_bar / np.sum(loc.bel_bar)
```

### Sensor Model
```
def sensor_model(obs):
    """ This is the equivalent of p(z|x).


    Args:
        obs ([ndarray]): A 1D array consisting of the true observations for a specific robot pose in the map 

    Returns:
        [ndarray]: Returns a 1D array of size 18 (=loc.OBS_PER_CELL) with the likelihoods of each individual sensor measurement
    """
    prob_array = np.zeros((1, mapper.OBS_PER_CELL))
    
    measurements = loc.obs_range_data
        
    for i in range(mapper.OBS_PER_CELL):
        prob_array[0,i] = loc.gaussian(measurements[i], obs[i], loc.sensor_sigma)
                
    return prob_array
```

### Update Step
```
def update_step():
    """ Update step of the Bayes Filter.
    Update the probabilities in loc.bel based on loc.bel_bar and the sensor model.
    """ 
    for x in range(mapper.MAX_CELLS_X):
        for y in range(mapper.MAX_CELLS_Y):
            for a in range(mapper.MAX_CELLS_A):
                sense_prob = np.prod( sensor_model(mapper.get_views(x,y,a)) ) 
                loc.bel[x,y,a] = sense_prob*loc.bel_bar[x,y,a]
                
    loc.bel = loc.bel / np.sum(loc.bel)
```    

### Running the Bayes Filter
```
for t in range(0, traj.total_time_steps):
    print("\n\n-----------------", t, "-----------------")
    
    prev_odom, current_odom, prev_gt, current_gt = traj.execute_time_step(t)
        
    # Prediction Step
    prediction_step(current_odom, prev_odom)
    loc.print_prediction_stats(plot_data=True)
    
    # Get Observation Data by executing a 360 degree rotation motion
    loc.get_observation_data()
    
    # Update Step
    update_step()
    loc.print_update_stats(plot_data=True)
```

### The Bayes Filter in Action
I sped the by 4 times so you don't have to watch a 2 minute video.

{% include youtube.html video="lLkfZRtz12M" %}


Trajectory stats
  
