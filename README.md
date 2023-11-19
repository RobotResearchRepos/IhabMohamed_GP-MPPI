# GP-MPPI

This repository contains the ROS implementation of the GP-MPPI control strategy, an online learning-based control strategy that integrates the state-of-the-art sampling-based Model Predictive Path Integral [(MPPI)](https://arc.aiaa.org/doi/pdf/10.2514/1.G001921) controller with a local perception model based on Sparse Gaussian Process (SGP).

The key idea is to leverage the learning capability of SGP to construct a variance (uncertainty) surface, enabling the robot to learn about the navigable space surrounding it, identify a set of suggested subgoals, and ultimately recommend the optimal subgoal that minimizes a predefined cost function for the local MPPI planner. MPPI then computes the optimal control sequence that satisfies the robot and collision avoidance constraints. This approach eliminates the need for a global map of the environment or an offline training process.

<img src="media/GP-MPPI-Architecture.png" alt="GP-MPPI-Architecture" width="400"> <img src="media/MU1_SM_T2_2x.gif" alt="Maze 1, Sample Mode (SM)" width="400">

## Paper:

If you find this code useful in your research, please cite:

Ihab S. Mohamed, Mahmoud Ali, and Lantao Liu. "**GP-guided MPPI for Efficient Navigation in Complex Unknown Cluttered Environments**." IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), 2023.

Paper: [https://arxiv.org/abs/2306.12369](https://arxiv.org/abs/2307.04019)

Bibtex:
```
@article{mohamed2023gp,
  title={GP-guided MPPI for Efficient Navigation in Complex Unknown Cluttered Environments},
  author={Mohamed, Ihab S and Ali, Mahmoud and Liu, Lantao},
  booktitle={IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)},
  year={2023}
}
```
### Media:
**Video**: https://youtu.be/et9t8X1wHKI

## Software Requirements and Installation Instructions:
 Please refer to the provided guidelines in [our log-MPPI](https://github.com/IhabMohamed/log-MPPI_ros#software-requirements) repository for detailed instructions. 

## ROS Packages:

The entire project will be uploaded soon; however, we are currently referring to our related repositories for the baselines [(namely, MPPI and log-MPPI)](https://github.com/IhabMohamed/log-MPPI_ros) and [GP perception model](https://github.com/mahmoud-a-ali/vsgp_pcl).

### jackal_ros
The `jackal_ros` package contains the common packages for Jackal, including Gazebo simulator with various world definitions and their ROS launch files, robot description, messages, and controllers. 

### mission_control
This package is forked from [ethz-asl/deep_motion_planning](https://github.com/ethz-asl/deep_motion_planning) with very few modifications. It contains a mission control node that executes a user-defined mission. Therefore, a txt file is parsed and a sequence of commands is generated. This sequence is then processed step-by-step. For more details on the definition of a mission, please refer to the [README](mission_control/README.md) file in the package directory.

### mppi_control
This node contains the implementation of both vanilla MPPI and log-MPPI control strategies. It subscribes to: (i) the 2D local costmap built by the robot on-board sensor for achieving a collision-free navigation and (ii) the set of desired poses published by the `mission_control` node; then, consequently, it publishes the control commands, namely, the linear and angular velocities of the robot.

### velodyne_simulator
URDF description and Gazebo plugins to simulate Velodyne laser scanners. If you encountered a very slow-motion of the robot in Gazebo, there are two ways to tackle this issue:
(i) you should be sure that the `gpu` parameter in [VLP-16.urdf.xacro](velodyne_simulator/velodyne_description/urdf/VLP-16.urdf.xacro) is set to `true` (we actually prefer this option), OR (ii) you can decrease the number of `samples` to let's say 500 instead of 1875 in [VLP-16.urdf.xacro](velodyne_simulator/velodyne_description/urdf/VLP-16.urdf.xacro). 

# To run GP-MPPI
1. roslaunch jackal_gazebo world_stage.launch env_name:=forest1 (0.2 tres/m2) OR maze1
2. roslaunch  gp_subgoal gp_subgoal_sim.launch
4. roslaunch mppi_control control_stage.launch gp_mppi:=true; gp_mppi:=false means that MPPI or log-mppi will be run 


### Primary code maintainer:
Ihab S. Mohamed and Mahmoud Ali (e-mail: {mohamedi, alimaa}@iu.edu)\
Vehicle Autonomy and Intelligence Lab ([VAIL](https://vail.sice.indiana.edu/))\
Indiana University - Bloomington, USA


