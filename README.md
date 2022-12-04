# Weekly Activities (after midterm) // Foxy Version

```
Group C (Team Members)
1. Dhungana Sudip (12194823)
2. Deuja Ritik (12194824)
3. Adhikari Keshav (12194874)
4. Yusupov Elbek Yormukhammad Udli (12194909)
5. Tamang Sanjib (12194939)
6. Thapa Pradip (12194940)
7. Pariyar Bijay (12194945)
```

# Week-9

# PC Setup using:
- Ubuntu 20.04
- ROS Foxy

## 1. Install ROS2 on Remote PC

Open the terminal and type the commands listed below one at a time.

```
wget https://raw.githubusercontent.com/ROBOTIS-GIT/robotis_tools/master/install_ros2_foxy.sh
```

![image](https://user-images.githubusercontent.com/92040822/200147279-93635a26-e453-4ef7-81ec-3d65961c7964.png)

```
sudo chmod 755 ./install_ros2_foxy.sh
bash ./install_ros2_foxy.sh
```
![image](https://user-images.githubusercontent.com/92040822/200147308-5d60e464-2113-4098-9ad6-5bf30fee222d.png)


## 2. Install Dependent ROS2 Packages

Open the terminal with Ctrl+Alt+T from Remote PC.

## 2.1 Install Gazebo11

```
sudo apt-get install ros-foxy-gazebo-*
```

![image](https://user-images.githubusercontent.com/92040822/200147456-6e07351b-d873-4a23-af36-dd7b33e2d682.png)


## 2.2 Install Cartographer

```
sudo apt install ros-foxy-cartographer
sudo apt install ros-foxy-cartographer-ros
```

![image](https://user-images.githubusercontent.com/92040822/200147523-2fe835e8-9cb7-4378-9995-b655a8497384.png)

## 2.3 Install Navigation2

```
sudo apt install ros-foxy-navigation2
sudo apt install ros-foxy-nav2-bringup
```

![image](https://user-images.githubusercontent.com/92040822/200147561-108de0ab-5763-4191-8ec9-4f9913afdc7d.png)


## 3 Install TurtleBot3 Packages

Build the TurtleBot3 packages with source code.

```
mkdir -p ~/turtlebot3_ws/src
cd ~/turtlebot3_ws/src/
git clone -b foxy-devel https://github.com/ROBOTIS-GIT/DynamixelSDK.git
git clone -b foxy-devel https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
git clone -b foxy-devel https://github.com/ROBOTIS-GIT/turtlebot3.git
cd ~/turtlebot3_ws
colcon build --symlink-install
echo 'source ~/turtlebot3_ws/install/setup.bash' >> ~/.bashrc
source ~/.bashrc
```

![image](https://user-images.githubusercontent.com/92040822/200147701-b75fadd2-5197-4183-b06b-9e17efbfb6d9.png)

![image](https://user-images.githubusercontent.com/92040822/200147808-b976d8f7-2505-4337-92a3-e7a4c286b708.png)

## 4. Environment Configuration

Set the ROS environment for PC

```
echo 'export ROS_DOMAIN_ID=30 #TURTLEBOT3' >> ~/.bashrc
source ~/.bashrc
```

![image](https://user-images.githubusercontent.com/92040822/200147852-c903d9f7-cc37-4418-9bc9-e3f6918dc929.png)

# Week-10

# SBC Setup using:
- Pen drive
- TurtleBot3 SBC Image (Raspberry Pi 4B (2GB or 4GB) ROS2 Foxy image)
- Raspberry Pi Imager

Following the preparation of a pen drive, downloading the correct image file for my hardware and the ROS version that is foxy, unzipping the downloaded image file, extracting the.img file, saving it to the local disk and burning the image file using a Raspberry Pi Imager was done. Then the following includes:

```
Click CHOOSE OS.
Click Use custom and select the extracted .img file from local disk.
Click CHOOSE STORAGE and select the microSD.
Click WRITE to start burning the image.
```

![image](https://github.com/sanjiblama28/San/blob/main/Veed%20Recording%20-%206%20November%202022%20(1).gif)

GParted GUI utility must first be installed using the provided code.

```
sudo apt install gparted
```

![image](https://user-images.githubusercontent.com/92040822/200148610-46d5d475-1fe8-4e39-8df6-da0478224778.png)

Next, gparted is launched. 

```
Select microSD card from the menu (mounted location may vary by system).
Right click on the yellow partition.
Select Resize/Move option.
Drag the right edge of the partition to all the way to the right end.
Click Resize/Move button.
Click the Apply All Operations green check button at the top.
```

![image](https://github.com/sanjiblama28/San/blob/main/Veed%20Recording%20-%206%20November%202022%20(3).gif)

## Configure the WiFi Network Setting

Launch a terminal window, and navigate to the netplan directory on the microSD card and open the 50-cloud-init.yaml file and begin editing it with a superuser permission sudo.

```
cd /media/$USER/writable/etc/netplan
sudo nano 50-cloud-init.yaml
```

Replace the WIFI SSID and WIFI PASSWORD with your wifi SSID and password once the editor has been opened.

Press Ctrl+S to save the document and Ctrl+X to close it.

![image](https://github.com/sanjiblama28/San/blob/main/Veed%20Recording%20-%206%20November%202022%20(4).gif)
![image](https://github.com/sanjiblama28/Github/blob/main/20221201_030911.jpg)
![image](https://user-images.githubusercontent.com/92040822/204874933-6a3d4aa7-dc98-49bc-9071-e6c457810958.png)

## ROS2 Network Configuration

ROS DOMAIN ID must be matched between Remote PC and TurtleBot3 for communication under the same network environment in ROS2 DDS communication.
In the .bashrc file, the default ROS Domain ID for TurtleBot3 is set to 30.
Please change the ID to minimize confusion when there are identical IDs in the same network.

```
ROS_DOMAIN_ID=30 #TURTLEBOT3
```

## NEW LDS-02 Configuration

Please follow the steps below on the TurtleBot3 SBC (Raspberry Pi).

1. Install the LDS-02 driver and update the TurtleBot3 package.

```
sudo apt update
sudo apt install libudev-dev
cd ~/turtlebot3_ws/src
git clone -b ros2-devel https://github.com/ROBOTIS-GIT/ld08_driver.git
cd ~/turtlebot3_ws/src/turtlebot3 && git pull
rm -r turtlebot3_cartographer turtlebot3_navigation2
cd ~/turtlebot3_ws && colcon build --symlink-install
```

![image](https://user-images.githubusercontent.com/92040822/204878291-2f56bb73-87af-4312-a460-f5bb49b0a4c3.png)

![image](https://user-images.githubusercontent.com/92040822/204879981-fdcf8708-b207-4d3e-a49f-adec275807a8.png)

![image](https://user-images.githubusercontent.com/92040822/204987063-623a27cb-3cb1-4345-b4c5-4e0a60985e3e.png)

Now, 
2. Export the LDS MODEL to the bashrc file. LDS-01 or LDS-02, depending on your LDS model.

```
echo 'export LDS_MODEL=LDS-02' >> ~/.bashrc
source ~/.bashrc
```
![image](https://user-images.githubusercontent.com/92040822/204987255-ab70dec1-7135-4448-a9c7-7a7fd64bd49a.png)

# OpenCR Setup

1. Connect the OpenCR to the Rasbperry Pi using the micro USB cable.
2. Install required packages on the Raspberry Pi to upload the OpenCR firmware.

```
sudo dpkg --add-architecture armhf
sudo apt update
sudo apt install libc6:armhf 
```

![image](https://user-images.githubusercontent.com/92040822/204988172-e33c0099-0d12-4723-a9ba-ecddeb6efbb6.png)

3. Depending on the platform, use either burger or waffle for the OPENCR_MODEL name.

```
export OPENCR_PORT=/dev/ttyACM0
export OPENCR_MODEL=burger
rm -rf ./opencr_update.tar.bz2
```

![image](https://user-images.githubusercontent.com/92040822/204989231-5613bc9d-d103-4406-a464-6ec586d6ce74.png)

4. Download the firmware and loader, then extract the file.

```
wget https://github.com/ROBOTIS-GIT/OpenCR-Binaries/raw/master/turtlebot3/ROS2/latest/opencr_update.tar.bz2
tar -xvf ./opencr_update.tar.bz2
```

![image](https://user-images.githubusercontent.com/92040822/204989421-5b95e2be-012a-4d2d-a585-8e5998686761.png)

5. Upload firmware to the OpenCR.

```
cd ~/opencr_update
./update.sh $OPENCR_PORT $OPENCR_MODEL.opencr
```

![image](https://user-images.githubusercontent.com/92040822/204989511-07ced6dd-7abe-431b-8127-651a0c2ef6c3.png)


5. OpenCR Test

We perform the OpenCR Test by following the below steps:
--> PUSH SW1 / PUSH SW2 were used to check if the robot has been properly assembeled or not and this process tests the left and right DYNAMIXEL's and the OpenCR board.
--> After assembling TurtleBot3, we connected the power to OpenCR and turn on the power switch of OpenCR. The red `power LED` will be turned on. 
--> And then the robot was placed in a flat ground in an open area.
--> After that, `PUSH SW 1` button was pushed for a few seconds to move the robot forward around 30 centimeters as recommended. 
--> And `PUSH SW 2` for a few seconds to command the robot to rotate 180 degrees in place. 

# Observation after Testing OpenCR

1. The robot has been succefully assembled.
2. The power connection to OpenCR is successful.
3. During the movement test, the wheels do not move, referring we were having problem with `DYNAMIXELs` so the next step would be setting up `DYNAMIXELs for TurtleBot3`.

# Setup DYNAMIXELs for TurtleBot3

To do this step, Firstly, we disconnected one of the DYNAMIXEL from our assembled TurtleBot3, as its important to connect `ONLY ONE DYNAMIXEL` with the OpenCR.

Following steps were performed to setup:

1. Turn on the power of the OpenCR and connection of the board with the PC via a USB cable.
2. Press and hold the `SW2` button and click on the `reset`button.  Once the `status` LED starts blinking, we release the `SW2` button.
3. Uploading Setup Firmware to OpenCR
- In the openCR board, we find `Examples` > `turtlebot3` > `turtlebot3_setup` > `turtlebot3_setup_motor` 

The image for that section is shown below. 

4. Clicking and Uploading button on the Arduino IDE
 Once the example upload to the OpenCR is completed, we connect ONE DYNAMIXEL to the OpenCR and clicked the `Serial Monitor` icon in the upper right corner.
 
 5. Reset OpenCR
 We press the `RESET` button if the example does not run properly.
 
 6. Selecting an Option
 When the `Serial Monitor` is running, menu for DYNAMIXEL setup will be displayed. TurtleBot3 consists of two DYNAMIXEL's actuators for the left and right wheels, so we select the proper option based on the assembled position. 
 
 For example, to set up the left side DYNAMIXEL, we enter `1` and to setup the right side DYNAMIXEL , we enter `2`.
 
 7. Confirmation
 
 To prevent any mistake, a confirmation will be required and to proceed the configuration, we enter `Y` to the input textbox. 
 
 
 8. Configure DYNAMIXEL
 
 The setup tool starts searching the connected DYNAMIXEL using different IDS and Baudrates. If DYNAMIXEL is detected, it will automatically set up for TurtleBot3.
When the setup is completed, `OK` message is printed on the screen.

9. Test DYNAMIXEL

Once the setup procedure and verify if the change has been made properly. We select one of the test options, and the selected DYNAMIXEL will iterate rotation and stop every 1 second to the counter-clockwise or clockwise depending on the selected wheel.
In order to end the test, we press `enter`.
To test the left DYNAMIXEL, we enter `3` and enter `4` for the right DYNAMIXEL test.

10. Upload TurtleBot3 Core

Once DYNAMIXEL setup is completed, TurtleBot3 Core example should be uploaded to OpenCR.
We can find the proper core example from `Examples` > `turtlebot3` > `turtlebot3_burger` and upload the example to openCR.

# Error while Configuring & Testing DYNAMIXELs

# Week-(11-12)

# SLAM

Its important to run the SLAM on `Remote PC` 

## 1. Run SLAM Node

1. If the Bringup is not running on the TurtleBot3 SBC, launch the Bringup first. Skip this step if you have launched bringup previously.
Open a new terminal from Remote PC with Ctrl + Alt + T and connect to Raspberry Pi with its IP address. The default password is turtlebot.

```
ssh ubuntu@{IP_ADDRESS_OF_RASPBERRY_PI}
```

![image](https://user-images.githubusercontent.com/92040822/204990674-f8be88a3-05ff-4e18-8970-826fece2ea90.png)

Please use the proper keyword among burger, waffle, waffle_pi for the TURTLEBOT3_MODEL parameter.

```
export TURTLEBOT3_MODEL=burger
ros2 launch turtlebot3_bringup robot.launch.py
```

![image](https://user-images.githubusercontent.com/92040822/204991887-22efbd01-4299-44f9-947c-8f9370a5f8ef.png)

2. Open a new terminal from Remote PC with Ctrl + Alt + T and launch the SLAM node. The Cartographer is used as a default SLAM method.

```
export TURTLEBOT3_MODEL=burger
ros2 launch turtlebot3_cartographer cartographer.launch.py
```

![image](https://user-images.githubusercontent.com/92040822/204992606-644e8085-a357-4920-8e31-44e9db2db3d2.png)

![image](https://user-images.githubusercontent.com/92040822/204992721-f0bdb26f-a890-41b7-8bdb-9f7ff00b361c.png)

## 2. Run Teleoperation Node

Once SLAM node is successfully up and running, TurtleBot3 will be exploring unknown area of the map using teleoperation. It is important to avoid vigorous movements such as changing the linear and angular speed too quickly. When building a map using the TurtleBot3, it is a good practice to scan every corner of the map.

1. Open a new terminal and run the teleoperation node from the Remote PC.
Please use the proper keyword among burger, waffle, waffle_pi for the TURTLEBOT3_MODEL parameter.

```
export TURTLEBOT3_MODEL=burger
ros2 run turtlebot3_teleop teleop_keyboard

Control Your TurtleBot3!
---------------------------
Moving around:
       w
  a    s    d
       x

w/x : increase/decrease linear velocity
a/d : increase/decrease angular velocity
space key, s : force stop

CTRL-C to quit
```

![image](https://user-images.githubusercontent.com/92040822/204993676-0954139b-86fa-45df-a9ff-67e960e6f4ad.png)

## 3. Tuning Guide

The SLAM in ROS2 uses Cartographer ROS which provides configuration options via Lua file.

Below options are defined in turtlebot3_cartographer/config/turtlebot3_lds_2d.lua file. For more details about each options, please refer to the Cartographer ROS official documentation.

1. MAP_BUILDER.use_trajectory_builder_2d
This option sets the type of SLAM.

2. TRAJECTORY_BUILDER_2D.min_range
This option sets the minimum usable range of the lidar sensor.

3. TRAJECTORY_BUILDER_2D.max_range
This option sets the maximum usable range of the lidar sensor.

4. TRAJECTORY_BUILDER_2D.missing_data_ray_length
In 2D, Cartographer replaces ranges further than max_range with TRAJECTORY_BUILDER_2D.missing_data_ray_length.

5. TRAJECTORY_BUILDER_2D.use_imu_data
If you use 2D SLAM, range data can be handled in real-time without an additional source of information so you can choose whether you’d like Cartographer to use an IMU or not.

6. TRAJECTORY_BUILDER_2D.use_online_correlative_scan_matching
Local SLAM : The RealTimeCorrelativeScanMatcher can be toggled depending on the reliability of the sensor.

7. TRAJECTORY_BUILDER_2D.motion_filter.max_angle_radians
Local SLAM : To avoid inserting too many scans per submaps, A scan is dropped if the motion does not exceed a certain angle.

8. POSE_GRAPH.optimize_every_n_nodes
Global SLAM : Setting POSE_GRAPH.optimize_every_n_nodes to 0 is a handy way to disable global SLAM and concentrate on the behavior of local SLAM.

9. POSE_GRAPH.constraint_builder.min_score
Global SLAM : Threshold for the scan match score below which a match is not considered. Low scores indicate that the scan and map do not look similar.

10. POSE_GRAPH.constraint_builder.global_localization_min_score
Global SLAM : Threshold below which global localizations are not trusted.

NOTE: Constraints can be visualized in RViz, it is very handy to tune global SLAM. One can also toggle POSE_GRAPH.constraint_builder.log_matches to get regular reports of the constraints builder formatted as histograms.

## 4. Save Map

The map is drawn based on the robot’s odometry, tf and scan information. These map data is drawn in the RViz window as the TurtleBot3 was traveling. After creating a complete map of desired area, save the map data to the local drive for the later use.

1. Launch the map_saver_cli node in the nav2_map_server package to create map files.
The map file is saved in the directory where the map_saver_cli node is launched at.
Unless a specific file name is provided, map will be used as a default file name and create map.pgm and map.yaml.

```
ros2 run nav2_map_server map_saver_cli -f ~/map
```

![image](https://user-images.githubusercontent.com/92040822/204996481-2c06bb19-d6eb-4e63-815c-7c5452a81c7a.png)


The -f option specifies a folder location and a file name where files to be saved.
With the above command, map.pgm and map.yaml will be saved in the home folder ~/(/home/${username}).

## 5. Map

The map uses two-dimensional Occupancy Grid Map (OGM), which is commonly used in ROS. The saved map will look like the figure below, where white area is collision free area while black area is occupied and inaccessible area, and gray area represents the unknown area. This map is used for the Navigation.

![image](https://user-images.githubusercontent.com/92040822/205026394-a447b491-0ef8-4e3d-b259-284cb8a09592.png)


## Presentation

The proposal preparation was also the focus of our effort this week.

# Week-13

# Navigation

## Run Navigation Nodes

1. If the Bringup is not running on the TurtleBot3 SBC, launch the Bringup. Skip this step if you have launched bringup previously.
Open a new terminal from Remote PC with Ctrl + Alt + T and connect to Raspberry Pi with its IP address. The default password is turtlebot.

```
ssh ubuntu@{IP_ADDRESS_OF_RASPBERRY_PI}
export TURTLEBOT3_MODEL=${TB3_MODEL}
ros2 launch turtlebot3_bringup robot.launch.py
```

![image](https://user-images.githubusercontent.com/92040822/204999396-2259acf3-aac3-487d-9a6a-6b4aafcbd01c.png)

2. Open a new terminal from Remote PC with Ctrl + Alt + T and launch the Navigation node. ROS 2 uses Navigation2. Please use the proper keyword among burger, waffle, waffle_pi for the TURTLEBOT3_MODEL parameter.

```
export TURTLEBOT3_MODEL=burger
ros2 launch turtlebot3_navigation2 navigation2.launch.py map:=$HOME/map.yaml
```

![image](https://user-images.githubusercontent.com/92040822/204998742-9a6abedf-4d5b-4b78-80d6-86984ec42927.png)

## Estimate Initial Pose

`Initial Pose Estimation` must be performed before running the Navigation as this process initializes the AMCL parameters that are critical in Navigation. Turtle3Bot has to be correctly located on the map with the LDS sensor data that neatly overlaps the displayed map. 

1. We clicked the `2D Pose Estimate` button in the RViz2 menu.
2. After that Click on the map where the actual robot is located and drag the large green arrow toward the direction where the robot is facing.
3. The steps 1 and 2 are repeated until the LDS sensor data is overlayed on the saved map. 
4. We launch keyboard teleoperation node to precisely locate the robot on the map. 
5. The robot is moved back and forth a bit to collect the surrounding environment information and narrow down the estimated location of the TurtleBot3 on the map which is displayed with tiny green arrows. 
6. The keyboard teleoperation node is terminated by entering `Ctrl` +`C` to the teleop node terminal in order to prevent different **cmd_vel** values are published from multiple nodes during navigation. 

## Set Navigation Goal

1. Click on the `Navigation2 Goal` button in the RViz2 menu.
2. Click on the map to set the destination of the robot and drag the green arrow toward the direction where the robot will be facing. 
 - This green arrow is a marker that can specify the destination of the robot.
 - The root of the arrow is `x`, `y` coordinate of the destination, and the angle θ is determined by the orientation of the arrow.
 - As soon as x, y, θ are set, TurtleBot3 will start moving to the destination immediately.
 
## Tuning Guide

1.1. Costmap Parameters

1.1.1. inflation_layer.inflation_radius
- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- This parameter makes inflation area from the obstacle.
- The route would be planned to avoid going through this location. Setting this to be greater than the robot radius is secure.

1.1.2. inflation_layer.cost_scaling_factor
- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The value of the costmap is multiplied by this inverse proportional factor. The value of the costmap decreases when this parameter is raised.

The optimal path for the robot to pass through obstacles is to take a median path between them. Setting a smaller value for this parameter will create a farther path from the obstacles.

1.2. dwb_controller

1.2.1. max_vel_x
--> Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
--> This factor is set the maximum value of translational velocity.

1.2.2. min_vel_x
--> Defined in turtlebot3_navigation2/param/${TB3_MODEL}.yaml
--> This factor is set the minimum value of translational velocity. If set this negative, the robot can move backwards.

1.2.3. max_vel_y
--> Defined in turtlebot3_navigation2/param/${TB3_MODEL}.yaml
--> The maximum y velocity for the robot in m/s.

1.2.4. min_vel_y
--> Defined in turtlebot3_navigation2/param/${TB3_MODEL}.yaml
--> The minimum y velocity for the robot in m/s.

1.2.5. max_vel_theta
--> Defined in turtlebot3_navigation2/param/${TB3_MODEL}.yaml
--> Actual value of the maximum rotational velocity. The robot can not be faster than this.

1.2.6. min_speed_theta
--> Defined in turtlebot3_navigation2/param/${TB3_MODEL}.yaml
--> Actual value of the minimum rotational speed. The robot can not be slower than this.

1.2.7. max_speed_xy
--> Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
--> The absolute value of the maximum translational velocity for the robot in m/s.

1.2.8. min_speed_xy
--> Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
--> The absolute value of the minimum translational velocity for the robot in m/s.

1.2.9. acc_lim_x
--> Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
--> The x acceleration limit of the robot in meters/sec^2.

1.2.10. acc_lim_y
--> Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
--> The y acceleration limit of the robot in meters/sec^2.

1.2.11. acc_lim_theta
--> Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
--> The rotational acceleration limit of the robot in radians/sec^2.

1.2.12. decel_lim_x
--> Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
--> The deceleration limit of the robot in the x direction in m/s^2.

1.2.13. decel_lim_y
--> Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
--> The deceleration limit of the robot in the y direction in m/s^2.

1.2.14. decel_lim_theta
--> Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
--> The deceleration limit of the robot in the theta direction in rad/s^2.

1.2.15. xy_goal_tolerance
--> Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
--> The x,y distance allowed when the robot reaches its goal pose.

1.2.16. yaw_goal_tolerance
--> Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
--> The yaw angle allowed when the robot reaches its goal pose.

1.2.17. transform_tolerance
--> Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
--> It allows the latency for tf messages.

1.2.18. sim_time
--> Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
--> This factor is set forward simulation in seconds. Setting this too small makes robot difficult to pass a narrow space while large value limits dynamic turns. You can observe the defferences of length of the yellow line in below image that represents the simulation path.


# Simulation

- We run the Simulation on Remote PC
- Launching the Simulation for the first time on the Remote PC may take a while to setup the environment

1. Gazebo Simulation

The Gazebo Simulation uses **ROS Gazebo package**, therefore proper Gazebo version for ROS2 Foxy has to be installed before running this instruction.

1.1. Install Simulation Package
The TurtleBot3 Simulation Package requires turtlebot3 and turtlebot3_msgs packages as prerequisite. Without these prerequisite packages, the Simulation cannot be launched.

```
cd ~/turtlebot3_ws/src/
git clone -b foxy-devel https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
cd ~/turtlebot3_ws && colcon build --symlink-install

```

1.2. Launch Simulation World

Three simulation environments are prepared for TurtleBot3. One of these environments are selected to launch Gazebo. For our TurtleBot 3 burger, its a Empty World.

`If we are to launch a new world, it is important to completely terminate other Simulation`.


a. Empty World

```
export TURTLEBOT3_MODEL=burger
ros2 launch turtlebot3_gazebo empty_world.launch.py
```
b. TurtleBot3 World
For TurtleBot3 Waffle!!

```
export TURTLEBOT3_MODEL=waffle
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```
c. TurtleBot3 House

For TurtleBot3 Waffle_pi

```
export TURTLEBOT3_MODEL=waffle_pi
ros2 launch turtlebot3_gazebo turtlebot3_house.launch.py
```

1.3. Operate TurtleBot3

In order to teleoperate the TurtleBot3 with the keyboard, launch the teleoperation node with below command in a new terminal window.

```
ros2 run turtlebot3_teleop teleop_keyboard
```







