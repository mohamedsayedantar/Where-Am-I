# Where Am I    [![Udacity - Robotics NanoDegree Program](https://s3-us-west-1.amazonaws.com/udacity-robotics/Extra+Images/RoboND_flag.png)](https://www.udacity.com/robotics)



Building a mobile robot for localization task and Creating a ROS package that launches this robot model in a Gazebo world and utilizes packages like AMCL and the Navigation Stack, trying to achieve the best possible localization results by Exploring, adding, and tuning specific parameters corresponding to each package.

![robot1](https://github.com/mohamedsayedantar/udacity_bot/blob/master/images/R1.png)



## project outlines :
- Abstract
- Introduction
- Background
- Model Configuration
- Results
- Discussion
- Future Work
- References

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

Because the robot may not always behave in a perfectly predictable way, it generates many random guesses of where it is going to be next. These guesses are known as particles. Each particle contains a full description of a possible future state. When the robot observes the environment, it discards particles inconsistent with this observation, and generates more particles close to those that appear consistent. In the end, hopefully most particles converge to where the robot actually is.



![map](https://github.com/mohamedsayedantar/udacity_bot/blob/master/images/map.png)


## Background 
localization is a core element for the mobile robots, several approaches to localize robots pose like Kalman Filter, Markov Localization, Grid Localization, Monte Carlo Localization. 


Kalman filtering, also known as linear quadratic estimation (LQE), is an algorithm that uses a series of measurements observed over time, containing statistical noise and other inaccuracies, and produces estimates of unknown variables that tend to be more accurate than those based on a single measurement alone, by estimating a joint probability distribution over the variables for each timeframe. The filter is named after Rudolf E. Kálmán, one of the primary developers of its theory.


Monte Carlo localization (MCL), also known as particle filter localization, is an algorithm for robots to localize using a particle filter. Given a map of the environment, the algorithm estimates the position and orientation of a robot as it moves and senses the environment. The algorithm uses a particle filter to represent the distribution of likely states, with each particle representing a possible state, a hypothesis of where the robot is. The algorithm typically starts with a uniform random distribution of particles over the configuration space, meaning the robot has no information about where it is and assumes it is equally likely to be at any point in space. Whenever the robot moves, it shifts the particles to predict its new state after the movement. Whenever the robot senses something, the particles are resampled based on recursive Bayesian estimation, how well the actual sensed data correlate with the predicted state. Ultimately, the particles should converge towards the actual position of the robot.

during this project the AMCL package will be used to localize the robot in the provided map.

![MCl](https://github.com/mohamedsayedantar/udacity_bot/blob/master/images/MCL.png)


## Model Configuration









![robot2](https://github.com/mohamedsayedantar/udacity_bot/blob/master/images/R2.png)

https://en.wikipedia.org/wiki/Monte_Carlo_localization

https://en.wikibooks.org/wiki/Robotics/Navigation/Localization
