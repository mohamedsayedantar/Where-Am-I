# Where Am I    [![Udacity - Robotics NanoDegree Program](https://s3-us-west-1.amazonaws.com/udacity-robotics/Extra+Images/RoboND_flag.png)](https://www.udacity.com/robotics)



Building a mobile robot for localization task and Creating a ROS package that launches this robot model in a Gazebo world and utilizes packages like AMCL and the Navigation Stack, trying to achieve the best possible localization results by Exploring, adding, and tuning specific parameters corresponding to each package.

![robot1](https://github.com/mohamedsayedantar/udacity_bot/blob/master/images/R1.png)
![robot2](https://github.com/mohamedsayedantar/udacity_bot/blob/master/images/R2.png)



## project outlines :
1. Abstract
2. Introduction
3. Background
4. Model Configuration
5. Results
6. Discussion
7. Future Work


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















