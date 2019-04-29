# Where Am I [![Udacity - Robotics NanoDegree Program](https://s3-us-west-1.amazonaws.com/udacity-robotics/Extra+Images/RoboND_flag.png)](https://www.udacity.com/robotics)



Building a mobile robot for localization task and Creating a ROS package that launches this robot model in a Gazebo world and utilizes packages like AMCL and the Navigation Stack, trying to achieve the best possible localization results by Exploring, adding, and tuning specific parameters corresponding to each package.

![robot1](https://github.com/mohamedsayedantar/udacity_bot/blob/master/images/R1.png)



## Abstract 

by building two mobile robots using Unified Robot Description Format which is an XML format for representing a robot model for Gazebo and RViz environment then by Creating a ROS package to launch these models in Gazebo and RViz and using packages like AMCL and the Navigation Stack for localizing the robot in a provided map, then by Exploring, adding, and tuning specific parameters corresponding to each package to achieve the best possible localization results.



## Introduction

Localization involves one question: Where is the robot now? Or, robo-centrically, where am I, keeping in mind that "here" is relative to some landmark (usually the point of origin or the destination) and that you are never lost if you don't care where you are.
Although a simple question, answering it isn't easy, as the answer is different depending on the characteristics of your robot. Localization techniques that work fine for one robot in one environment may not work well or at all in another environment. 
For example, localizations which work well in an outdoors environment may be useless indoors.

All localization techniques generally provide two basic pieces of information:
1. what is the current location of the robot in some environment?
2. what is the robot's current orientation in that same environment?

The first could be in the form of Cartesian or Polar coordinates or geographic latitude and longitude. The latter could be a combination of roll, pitch and yaw or a compass heading.

Consider a robot with an internal map of its environment. When the robot moves around, it needs to know where it is within this map. Determining its location and rotation (more generally, the pose) by using its sensor observations is known as robot localization.

![map](https://github.com/mohamedsayedantar/udacity_bot/blob/master/images/map.png)



## Background 
localization is a core element for the mobile robots, several approaches to localize robots pose like Kalman Filter, Markov Localization, Grid Localization, Monte Carlo Localization. 


Kalman filtering, also known as linear quadratic estimation (LQE), is an algorithm that uses a series of measurements observed over time, containing statistical noise and other inaccuracies, and produces estimates of unknown variables that tend to be more accurate than those based on a single measurement alone, by estimating a joint probability distribution over the variables for each timeframe. The filter is named after Rudolf E. Kálmán, one of the primary developers of its theory.


Monte Carlo localization (MCL), also known as particle filter localization, is an algorithm for robots to localize using a particle filter. Given a map of the environment, the algorithm estimates the position and orientation of a robot as it moves and senses the environment. The algorithm uses a particle filter to represent the distribution of likely states, with each particle representing a possible state, a hypothesis of where the robot is. The algorithm typically starts with a uniform random distribution of particles over the configuration space, meaning the robot has no information about where it is and assumes it is equally likely to be at any point in space. Whenever the robot moves, it shifts the particles to predict its new state after the movement. Whenever the robot senses something, the particles are resampled based on recursive Bayesian estimation, how well the actual sensed data correlate with the predicted state. Ultimately, the particles should converge towards the actual position of the robot.



during this project the AMCL package will be used to localize the robot in the provided map.

![MCl](https://github.com/mohamedsayedantar/udacity_bot/blob/master/images/MCL.png)


## Model Configuration
below the configuration made for Where Am I project

some of the following package installation instructions might be required
```
$ sudo apt-get install ros-kinetic-navigation
$ sudo apt-get install ros-kinetic-map-server
$ sudo apt-get install ros-kinetic-move-base
$ rospack profile
$ sudo apt-get install ros-kinetic-amcl
```

Let's start by navigating to the src directory and creating a new empty package.

```
$ cd /home/workspace/catkin_ws/src/
$ catkin_create_pkg udacity_bot
```

Next, create folders, `launch` and `worlds`, that will further define the structure of your package.

```
$ cd udacity_bot
$ mkdir launch
$ mkdir worlds
```

Let's create a simple world, with no objects or models that will be launched later in Gazebo.

```
$ cd worlds
$ nano udacity.world
```

Add the following to `udacity.world`

```xml
<?xml version="1.0" ?>

<sdf version="1.4">

  <world name="default">

    <include>
      <uri>model://ground_plane</uri>
    </include>

    <!-- Light source -->
    <include>
      <uri>model://sun</uri>
    </include>

    <!-- World camera -->
    <gui fullscreen='0'>
      <camera name='world_camera'>
        <pose>4.927360 -4.376610 3.740080 0.000000 0.275643 2.356190</pose>
        <view_controller>orbit</view_controller>
      </camera>
    </gui>

  </world>
</sdf>
```

Next, we will create a launch file. Launch files in ROS allow us to execute more than one node simultaneously, which helps avoid a potentially tedious task of defining and launching several nodes in separate shells or terminals.

```
$ cd ..
$ cd launch
$ nano udacity_world.launch
```

Add the following to your launch file.

```xml
<?xml version="1.0" encoding="UTF-8"?>

<launch>
  <include file="$(find udacity_bot)/launch/robot_description.launch"/>
  <arg name="world" default="empty"/> 
  <arg name="paused" default="false"/>
  <arg name="use_sim_time" default="true"/>
  <arg name="gui" default="true"/>
  <arg name="headless" default="false"/>
  <arg name="debug" default="false"/>

  <include file="$(find gazebo_ros)/launch/empty_world.launch">
    <arg name="world_name" value="$(find udacity_bot)/worlds/jackal_race.world"/>
    <arg name="paused" value="$(arg paused)"/>
    <arg name="use_sim_time" value="$(arg use_sim_time)"/>
    <arg name="gui" value="$(arg gui)"/>
    <arg name="headless" value="$(arg headless)"/>
    <arg name="debug" value="$(arg debug)"/>
  </include>

  <node name="urdf_spawner" pkg="gazebo_ros" type="spawn_model" respawn="false" output="screen" args="-urdf -param robot_description -model udacity_bot"/>
  <node name="rviz" pkg="rviz" type="rviz" respawn="false"/>
</launch>
```

### for Robot URDF

Let's first create a new folder in your package directory and an empty xacro file for the robot's URDF description.

```
$ cd /home/workspace/catkin_ws/src/udacity_bot/
$ mkdir urdf
$ cd urdf
$ nano udacity_bot.xacro
```
now you can build your robot model in the xacro file below my 2 robot models

the xacro file for the provided robot at 

https://github.com/mohamedsayedantar/udacity_bot/blob/master/urdf/udacity_bot.xacro

the xacro file for the new robot at 

https://github.com/mohamedsayedantar/udacity_bot/blob/master/urdf/udacity_bot2.xacro


Create a new launch file that will help load the URDF file.
```
$ cd /home/workspace/catkin_ws/src/udacity_bot/launch/
$ nano robot_description.launch
```

Copy the following into the above file.

```xml
<?xml version="1.0"?>
<launch>

  <!-- send urdf to param server -->
  <param name="robot_description" command="$(find xacro)/xacro --inorder '$(find udacity_bot)/urdf/udacity_bot.xacro'" />

  <node name="joint_state_publisher" pkg="joint_state_publisher" type="joint_state_publisher">
    <param name="use_gui" value="false"/>
  </node>

  <node name="robot_state_publisher" pkg="robot_state_publisher" type="robot_state_publisher" respawn="false" output="screen"/>

</launch>
```

for Gazebo Plugins the proper file for it
at https://github.com/mohamedsayedantar/udacity_bot/blob/master/urdf/udacity_bot.gazebo

this file include the following plugins for gazebo

- plugin for the camera sensor.
- plugin for the hokuyo sensor.
- plugin for controlling the wheel joints.

for the map create new folder named maps
```
$ cd /home/workspace/catkin_ws/src/udacity_bot/
$ mkdir maps
$ cd maps
```

You can then copy the two files `jackal_race.pgm` and `jackal_race.yaml` from the project repo into the “maps” folder.


to launch your robot model run the following 
```
$ cd /home/workspace/catkin_ws/
$ catkin_make
$ source devel/setup.bash
$ roslaunch udacity_bot udacity_world.launch
```

![robot2](https://github.com/mohamedsayedantar/udacity_bot/blob/master/images/R2.png)


Note : the previous instructions for the provided robot, the same made for the other robot model at `udacity_world2.launch`, `robot_description2.launch`, `udacity_bot2.gazebo` and `udacity_bot2.xacro`.

### AMCL

Adaptive Monte Carlo Localization (AMCL) dynamically adjusts the number of particles over a period of time, as the robot navigates around in a map. This adaptive process offers a significant computational advantage over MCL.

The ROS amcl package implements this variant and you will integrate this package with your robot to localize it inside the provided map.

by creating a new launch file.
```
$ cd /home/workspace/catkin_ws/src/udacity_bot/launch/
$ nano amcl.launch
```

the following my `amcl.launch` file
```
<?xml version="1.0"?>
<launch>

  <!-- Map server -->
  <arg name="map_file" default="$(find udacity_bot)/maps/jackal_race.yaml"/>
  <node name="map_server" pkg="map_server" type="map_server" args="$(arg map_file)" />

  <!-- Localization-->
  <node pkg="amcl" type="amcl" name="amcl" output="screen">
    <remap from="scan" to="udacity_bot/laser/scan"/>
    <param name="odom_frame_id" value="odom"/>
    <param name="odom_model_type" value="diff-corrected"/>
    <param name="base_frame_id" value="robot_footprint"/>
    <param name="global_frame_id" value="map"/>
    <param name="max_particles" value="200"/>
    <param name="min_particles" value="100"/>
    <param name="initial_pose_x" value="0.0"/>
    <param name="initial_pose_y" value="0.0"/>
    <param name="initial_pose_a" value="0.0"/>
    <param name="update_min_d" value="0.005"/>
    <param name="update_min_a" value="0.01"/>
    <param name="laser_model_type" value="likelihood_field"/>
    <param name="laser_likelihood_max_dist" value="2.0"/>
    <param name="laser_min_range" value="-1"/>
    <param name="laser_max_range" value="-1"/>
  </node>

<!-- Move base -->
  <node pkg="move_base" type="move_base" respawn="false" name="move_base" output="screen">
    <rosparam file="$(find udacity_bot)/config/costmap_common_params.yaml" command="load" ns="global_costmap" />
    <rosparam file="$(find udacity_bot)/config/costmap_common_params.yaml" command="load" ns="local_costmap" />
    <rosparam file="$(find udacity_bot)/config/local_costmap_params.yaml" command="load" />
    <rosparam file="$(find udacity_bot)/config/global_costmap_params.yaml" command="load" />
    <rosparam file="$(find udacity_bot)/config/base_local_planner_params.yaml" command="load" />
    <remap from="cmd_vel" to="cmd_vel"/>
    <remap from="odom" to="odom"/>
    <remap from="scan" to="udacity_bot/laser/scan"/>
    <param name="base_global_planner" type="string" value="navfn/NavfnROS" />
    <param name="base_local_planner" value="base_local_planner/TrajectoryPlannerROS"/>
  </node>
</launch>
```

add the configuration files for the move_base package.

```
$ cd /home/workspace/catkin_ws/src/udacity_bot
$ mkdir config
$ cd config
```

copy the files from this repo located `config` folder to your config folder.

### Launch Setup

we have the robot, the map, the localization and navigation nodes. Let’s launch it all and test it!
```
$ cd /home/workspace/catkin_ws/
$ roslaunch udacity_bot udacity_world.launch
```

then on another terminal 
```
$ roslaunch udacity_bot amcl.launch
```

Note : the configuration for the RViz located at `myRviz.rviz` file

then you will see some thing like that for the two robots

#### robot 1
![robot1](https://github.com/mohamedsayedantar/udacity_bot/blob/master/images/R12.png)
![robot1](https://github.com/mohamedsayedantar/udacity_bot/blob/master/images/R13.png)

#### robot 2
![robot2](https://github.com/mohamedsayedantar/udacity_bot/blob/master/images/R11.png)
![robot2](https://github.com/mohamedsayedantar/udacity_bot/blob/master/images/R14.png)


### parameters 

1. for `local_costmap_params.yaml` 
    - update_frequency: 20.0  -> to reduce command warnings 
    - publish_frequency: 25.0  -> to reduce command warnings 
    - width: 3.5  -> to force the local planing to maximum 3.5 meters to improve the movement of the robot
    - height: 3.5  -> the same for the width
    - resolution: 0.05  -> agood resolution.
    
2. for `global_costmap_params.yaml`
    - pdate_frequency: 15.0  ->to reduce command warnings 
    - publish_frequency: 20.0  ->to reduce command warnings 
    - width: 500.0
    - height: 500.0
    - resolution: 0.05

3. for `base_local_planner_params.yaml`
    - sim_time: 7.5  -> proper for the robot action 

4. for `costmap_common_params.yaml` the following parameters are proper for this robot and environment
    - obstacle_range: 1
    - raytrace_range: 5
    - transform_tolerance: 3.5
    - inflation_radius: 0.7

5. for `amcl.launch`
    - min_particles set to 100  -> to prevent any value of particles lower than 100
    - max_particles set to 200  -> increasing it almost increase the accuracy
    - update_min_d set to 0.005  -> to increase the rate of refresh or update data lead to faster conversion 
    - update_min_a set to 0.01  -> the same as d
    

































## Results

## Discussion

## Future Work






## References
https://en.wikipedia.org/wiki/Monte_Carlo_localization

https://en.wikibooks.org/wiki/Robotics/Navigation/Localization
