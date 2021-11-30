# ros_rrt_exploration

This is a ROS package for Turtlebot3 waffle_pi robot to explore autonomously and manually with unknown maps. The maps are built using the Gmapping algorithm, with several environment maps to choose from, which can be switched in the launch file. 
And the autonomous exploration is done by the Rapid Exploration Random Tree (RRT) algorithm. Afterwards, the path planning algorithm in the Navigation stack is used to reach the target point in the newly generated map.
The implementation of the RRT algorithm is based on the rrt_exploration package (http://wiki.ros.org/rrt_exploration). I have further modified the source code to enable the implementation of the rrt algorithm on the turtlebot3 robot.

## There are five main steps to use this package

### 1. Install
The package is developed for ROS Melodic, and it should be compatible for the other ROS version. Just change the {ROS_DISTRO} to the corresponding ROS version
1. Install ROS using this link: http://wiki.ros.org/ROS/Installation

2. Install dependancy packages in the Linux terminal:
```bash
    source /opt/ros/melodic/setup.bash

    sudo apt-get install ros-${ROS_DISTRO}-gazebo-*
    sudo apt-get install ros-${ROS_DISTRO}-gazebo-ros-pkgs 
    sudo apt-get install ros-${ROS_DISTRO}-gazebo-ros-control
    sudo apt-get install ros-${ROS_DISTRO}-navigation
    sudo apt-get install ros-${ROS_DISTRO}-gmapping
    sudo apt-get install ros-${ROS_DISTRO}-turtlebot3
    sudo apt-get install ros-${ROS_DISTRO}-turtlebot3-msgs
    sudo apt-get install ros-${ROS_DISTRO}-teleop-twist-keyboard
    sudo apt-get install python-opencv python-numpy python-scikits-learn   
 ```
3. Clone this repository in the src folder in the catkin workspace.

4. Build the package

```bash
    source /opt/ros/melodic/setup.bash
    cd ~/catkin_ws
    catkin_make
    source ./devel/setup.bash
 ```
## 2. Autonomous Exploration
1. Set the Turtlebot3 Model in the terminal

```bash
	export TURTLEBOT3_MODEL=waffle_pi

	source /opt/ros/melodic/setup.bash
	source ./devel/setup.bash
```

2. Start the autonomous exploration and generate the map by executing the following command

```bash
	roslaunch ros_autonomous_slam turtlebot3_rrt.launch
```
The chosen map and the robot will show in Gazebo, and in RVIZ, the whole autonomous exploration and map construction process is visiable.

3. In RVIZ window, set exploration region for RRT using **Publish Point** tool.
	First, place four points counterclockwise as the vertices of the rectangular exploration region, and place the fifth point near the robot as the root node of the rrt tree. It is shown in the figure below, for more information please refer:  **[rrt exploration](http://wiki.ros.org/rrt_exploration)**.

4. When robot complete the mapping, run the command in a new terminal to save the map.

```bash
	rosrun map_server map_saver -f  '{map_name}'
```

## 3. Manual Exploration
It is also possible to do manual exploration, you can control the robot using keyboard and construct the map.

1. Run the following command to setup the environment and robot.

```bash
	roslaunch ros_autonomous_slam turtlebot3_teleop.launch
```

2. Open another terminal, run the command to control the robot using a/w/d/x/s.

```bash
	roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```

3. Save the map.

```bash
	rosrun map_server map_saver -f  '{map_name}'
```


## 4. Change the Map
There are three maps available in the package, the map files are in the world folder.
There is a big map file 'turtlebot3_house.world', a small map file 'turtlebot3_stage_4.world', and a big map with soft floor 'turtlebot3_house_modify.world'
The default map in launch file is the big map 'turtlebot_house.world', and you can change the executing map in each launch file, it is shows in the comments.

## 5. Navigation
Run the following commands to do path planning with the newly generated map.
A Gazebo and RVIZ window will open and show the robot location. 
After setting the goal position using **2D NavGoal** tool in RVIZ, you can see the whole navigation process.
For more information, please refer **[turtlebot3 navigation](https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation/#tuning-guide)**.
    
1. Run the command to setup enviornment with newly generated map.
```bash
    roslaunch ros_autonomous_slam turtlebot3_navigation.launch 
```

2. Set the estimated robot position using **2D Pose Estimate** tool in RVIZ. 
The arrow shows both the estimated initial location and the orientation of the robot.

3. Set the goal point using **2D NavGoal** tool in RVIZ with both position and orientation as well.
4. Then you can see the navigation process in RVIZ.






