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

### jackal_ros
The [`jackal_ros`](https://github.com/IhabMohamed/GP-MPPI/tree/main/jackal_ros) package contains the common packages for Jackal, including Gazebo simulator with various world definitions and their ROS launch files, robot description, messages, and controllers. 

### gp_subgoal
The [`gp_subgoal`](https://github.com/IhabMohamed/GP-MPPI/tree/main/gp_subgoal) package utilizes the SGP occupancy model to learn the navigable space around the robot, identifies a set of suggested subgoals, and recommends the optimal subgoal to the MPPI local planner by minimizing a predefined cost function.

### mppi_control
The [`mppi_control`](https://github.com/IhabMohamed/GP-MPPI/tree/main/mppi_control) node encompasses the implementation of vanilla MPPI, log-MPPI, and GP-MPPI control strategies. It subscribes to: (i) the 2D local costmap generated by the robot's onboard sensor for achieving collision-free navigation and (ii) the recommended GP-subgoal published by the gp_subgoal node. Subsequently, it publishes the control commands, specifically the linear and angular velocities of the robot.

### velodyne_simulator
URDF description and Gazebo plugins to simulate Velodyne laser scanners. To enhance the **_real-time_** performance of the GP-subgoal recommender node (`gp_subgoal`), we've reduced the sample count to 500 (namely, `samples:=500`) from the original 1875 in [VLP-16.urdf.xacro](https://github.com/IhabMohamed/GP-MPPI/blob/b7faf0006f1730d14fbecce75b2f87f48c5e7671/velodyne_simulator/velodyne_description/urdf/VLP-16.urdf.xacro#L16).

## Usage: 
1. To initiate the Gazebo simulation with Jackal in the provided cluttered environment, execute:
	```
	roslaunch jackal_gazebo world_stage.launch env_name:=maze1
	```
 	* `maze1` is a maze-like environment spanning 20 meters by 20 meters, featuring three U-shaped rooms (U1, U2, and U3), along with several additional obstacles.
 	* `forest1` is a forest-like environment measuring 50 meters by 50 meters, with a tree density of 0.2 trees per square meter. To create your own forest-like environment, please utilize the [forest_gen](https://github.com/ethz-asl/forest_gen) package.

2. To start the MPPI or log-MPPI controller, 2D costmap node, and visualization of the system, run:
 	* For MPPI, run:
	```
	roslaunch mppi_control control_stage.launch normal_dist:=true
	```
	* For log-MPPI, run:
	```
	roslaunch mppi_control control_stage.launch normal_dist:=false
	```

1. roslaunch jackal_gazebo world_stage.launch env_name:=forest1 (0.2 tres/m2) OR maze1
2. roslaunch  gp_subgoal gp_subgoal_sim.launch
4. roslaunch mppi_control control_stage.launch gp_mppi:=true; gp_mppi:=false means that MPPI or log-mppi will be run 


### Primary code maintainer:
Ihab S. Mohamed and Mahmoud Ali (e-mail: {mohamedi, alimaa}@iu.edu)\
Vehicle Autonomy and Intelligence Lab ([VAIL](https://vail.sice.indiana.edu/))\
Indiana University - Bloomington, USA


