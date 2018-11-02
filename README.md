# Wheelchair-Arm-Control

A robot arm control library with functions of instruction interpretation, object recognition, arm motion planning and execution.
## Environment Setup
[ROS Melodic](http://wiki.ros.org/melodic/Installation/Ubuntu)  
[ros_control](http://wiki.ros.org/ros_control)  
[moveit!](https://moveit.ros.org/install/)  
[gazebo_ros_pkgs](http://wiki.ros.org/gazebo_ros_pkgs)  
[PointCloudLibrary](http://www.pointclouds.org/downloads/linux.html)
[PCL_ROS](http://library.isr.ist.utl.pt/docs/roswiki/Documentation.html)


## How to run  
0. clone to your ros catkin workspace and build the package  
```
cd ../catkin_ws/src
git clone https://github.com/scottlx/Wheelchair-Arm-Control.git  
cd ..  
catkin_make
```

1. source setup.bash
```
source ../catkin_ws/devel/setup.bash
```

## overview
### my_arm  
urdf files, mesh files, rviz model visualize launch file, gazebo launch file.  

Visualize the arm model in rviz:  

`roslaunch my_arm display.launch`  

Spawn the model into gazebo:  

`roslaunch my_arm gazebo.launch`  
### my_arm_control

low_level control  

`roslaunch my_arm_control my_arm_control.launch`  
It loads seven position controllers with controller_manager. We can publish messages to the controller to control the joints.  
After launching `gazebo.launch` and `my_arm_control.launch`  

`rosrun rqt_gui rqt_gui`  

Then apply a sinusoid wave to each joint  

![alt](/demo_img/publisher.png)  

Here is what you see in gazebo. Each joints is revoluting with a force of sinusoid wave:  
![alt](/demo_img/gazebo.gif)  

### arm_moveit_config  

Config file generated by moveit setup assistant  

play with the motion_planning plugin in rviz  

`roslaunch arm_moveit_config demo`  

![alt](/demo_img/moveit.gif)  

### object recognition using PCL

we are using PCL(point cloud library) to process depth image data from ROS
Here is a simple gazebo virtual environment
containing our robotic arm, some basic background, a coke and a beer
![alt](/demo_img/gazebo.JPG)
You can see there is a kinectic depth camera besides the arm_moveit_config



Here is what the depth camera sees:
![alt](/demo_img/point_cloud_view.JPG)


we are using [Correspondence Grouping](http://www.pointclouds.org/documentation/tutorials/correspondence_grouping.php) methods to find the coke can in the virtual environment
![alt](/demo_img/pcd_location.JPG)
You can see that rotational matrix and transitional matrix being calculated.
The green dots in the picture are the matched corresponding points
#### how to run pcl_recognition

1. make sure you have install [PCL_ROS](http://wiki.ros.org/pcl_ros) which is a ROS package used as an interface with PCL
2. make sure you have publish your points information to rostopic
   run `rostopic list` to see whether you have `/camera/depth/points`
3. run `cd pcl & rosrun pcl_ros pointcloud_to_pcd input:=/camera/depth/points`
   this will save pcd file to local directory, you can change the name to scene.pcd
4. run
  ```./pcl coke_model.pcd scene.pcd -k -c --model_ss 0.02 --scene_ss 0.02 --cg_thresh 5 --cg_size 0.13```


## Goals to achieve  

1. Apply motion planning to low level control (ros_control)  
2. Use C++ or Python to do the planning rather than rviz plugin.  
