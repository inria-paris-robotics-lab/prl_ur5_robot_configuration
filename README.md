# prl_ur5_robot_configuration
This repo  holds the current configuration of the bimanual UR5 robot.

## Setup

For different experiments, the robot can be equipped with different grippers and sensors.
To simplify the modification of the description, a changeable parameters are placed in a separate setup files in the **config/** directory.
Each setup file contains a list of used devices and calibration parameters.
The calibration parameters contain the positions of the arms, grippers, sensors, and kinematic parameters of UR5 arms.