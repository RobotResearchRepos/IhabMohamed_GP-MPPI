# GP-MPPI

This repository contains the ROS implementation of the GP-MPPI control strategy, an \textit{online} learning-based control strategy that integrates the state-of-the-art sampling-based Model Predictive Path Integral [(MPPI)](https://arc.aiaa.org/doi/pdf/10.2514/1.G001921) controller with a local perception model based on Sparse Gaussian Process (SGP).

The key idea is to leverage the learning capability of SGP to construct a variance (uncertainty) surface, enabling the robot to learn about the navigable space surrounding it, identify a set of suggested subgoals, and ultimately recommend the optimal subgoal that minimizes a predefined cost function for the local MPPI planner. MPPI then computes the optimal control sequence that satisfies the robot and collision avoidance constraints. This approach eliminates the need for a global map of the environment or an offline training process.

![GP-MPPI-Architecture](media/GP-MPPI-Architecture.png)

## ROS Packages:
The whole project will be uploaded soon. However, We are referring to our related repository of the baselines, namely, MPPI and log-MPPI.

# To run GP-MPPI
1. roslaunch jackal_gazebo world_stage.launch env_name:=forest1 (0.2 tres/m2) OR maze1
2. roslaunch vsgp_nav_glb mppi_subgoal_sim.launch
3. roslaunch mppi_control control_stage.launch gp_mppi:=false , gp_mppi:=false means that MPPI or log-mppi will be run 


### Primary code maintainer:
Ihab S. Mohamed (e-mail: mohamedi@iu.edu)\
Vehicle Autonomy and Intelligence Lab ([VAIL](https://vail.sice.indiana.edu/))\
Indiana University - Bloomington, USA


