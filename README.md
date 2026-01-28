# ROS-Based-Navigation-Robot
This repository aims to use an Autonomous Wheeled Robot equipped with 3D Lidar and a Camera to map the environment and navigate it while avoiding obstacles.

- We use the Husky wheeled robot to achieve the same.
- For mapping the environment we use the RtabMap package by using a Velodyne Lidar on the Husky Robot and an IntelRealsense RGB-D camera. 
- For Path Planning and Navigation of the robot, we use the move_base with the Global Planner for path planning and the Teb Local Planner for local path and dynamic obstacle avoidance

## Pre-requisites:
1) Ubuntu 20.04
2) ROS-Noetic

## Installations:
Execute the following to install certain packages:
 ```
$ sudo apt install ros-noetic-realsense2-description
$ sudo apt-get install ros-noetic-velodyne-description
$ sudo apt-get install ros-noetic-realsense2-camera
$ sudo apt-get install ros-noetic-velodyne-description
$ sudo apt install ros-noetic-move-base
$ sudo apt install ros-noetic-teb-local-planner
 ```
## Step 1: Setting up the environment
1) Create a catkin workspace:
    ```
    $ mkdir ~/catkin_ws/
    $ cd ~/catkin_ws/
    $ mkdir src/
    $ catkin_init_workspace
    
   ```
2) Clone the repository :
     ```
    $ cd ~/catkin_ws/src
    $ git clone https://github.com/kjyothiswaroop/ROS-Based-Navigation-Robot/
    $ mv husky/ rtabmap_ros/ ~/catkin_ws/src/
    $ rm -rf ROS-Based-Navigation-Robot
     
   ```
   Now inside ~/catkin_ws/ execute:
   ```
    $ cd ~/catkin_ws/
    $ catkin_make
   
   ```
## Step 2: Launching the Husky Robot
1) Setting the required variables:
  Open a terminal and execute the following:-
   ```
    $ export HUSKY_URDF_EXTRAS=$(rospack find rtabmap_demos)/launch/config/husky_velodyne_gpu_extra.urdf.xacro
    $ export HUSKY_REALSENSE_ENABLED=1
    $ export HUSKY_REALSENSE_XYZ="0.2206 0 0.1"
     
   ```
2) In the same terminal, execute the following :(This step is to launch Husky robot in Gazebo environment)
   ```
    $ cd ~/catkin_ws/
    $ source devel/setup.bash
    $ roslaunch husky_gazebo husky_playpen.launch
     
   ```
3) In a different terminal, execute the below:( This step is to visualise the robot in Rviz)
   ```
    $ cd ~/catkin_ws/
    $ source devel/setup.bash
    $ roslaunch husky_viz view_robot.launch
     
   ```
## Step3: Mapping the environment
1) Starting the RtabMap :
   Open a new terminal and execute the below commands
   ```
    $ cd ~/catkin_ws/
    $ source devel/setup.bash
    $ roslaunch rtabmap_demos demo_husky.launch lidar3d:=true slam2d:=false camera:=true rtabmap_viz:=true
     
   ```
2) Running Teleop for Husky to move around and map the environment:
   Open a new terminal and execute the below commands
   ```
    $ cd ~/catkin_ws/
    $ source devel/setup.bash
    $ roslaunch husky_control teleop_keyboard.launch
     
   ```
At this point, you should be able to see the Robot with a Velodyne Lidar and RGB-D camera in Gazebo as well as Rviz. Additionally, the RtabMap Viz window will also open to show the 3D map being formed. Move the robot slowly using the teleop terminal such that the robot covers the entire area and in the end comes back to the start point so as to perform loop closure.

Once the mapping is done, close all the terminals opened till now.

The map gets saved to the Rtabmap database located at ~/.ros/rtabmap.db.
This can be visualised using :
```
    $ cd ~/catkin_ws/
    $ source devel/setup.bash
    $ rtabmap-databaseViewer ~/.ros/rtabmap.db
     
```

## Step4 : Navigation 
1) Execute the following in a new terminal :
   ```
    $ export HUSKY_URDF_EXTRAS=$(rospack find rtabmap_demos)/launch/config/husky_velodyne_gpu_extra.urdf.xacro
    $ export HUSKY_REALSENSE_ENABLED=1
    $ export HUSKY_REALSENSE_XYZ="0.2206 0 0.1"
    $ cd ~/catkin_ws/
    $ source devel/setup.bash
    $ roslaunch husky_gazebo husky_playpen.launch
     
   ```
2) In a second terminal execute the following :
   ```
    $ cd ~/catkin_ws/
    $ source devel/setup.bash
    $ roslaunch rtabmap_demos demo_husky.launch lidar3d:=true slam2d:=false camera:=true rtabmap_viz:=true localization:=true
     
   ```
This will launch the Husky Robot again in the Gazebo environment and open an Rviz window and Rtabmap Viz window. This Rtabmap is being used for localisation.
Using the "2D Nav Goal" option in Rviz give a goal in the environment and the robot will plan its path and achieve the goal.

Additionally, add a dynamic obstacle in the Gazebo environment and the robot will avoid the obstacle.



