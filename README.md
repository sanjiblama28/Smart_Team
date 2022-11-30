# Weekly Activities (after midterm)

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

# 1. Install ROS2 on Remote PC

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


# 2. Install Dependent ROS2 Packages

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


# 3 Install TurtleBot3 Packages

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

# 3. Environment Configuration

Set the ROS environment for PC

```
echo 'export ROS_DOMAIN_ID=30 #TURTLEBOT3' >> ~/.bashrc
source ~/.bashrc
```

![image](https://user-images.githubusercontent.com/92040822/200147852-c903d9f7-cc37-4418-9bc9-e3f6918dc929.png)

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

# Configure the WiFi Network Setting

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

Install the LDS-02 driver and update the TurtleBot3 package.

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



Now, 
Export the LDS MODEL to the bashrc file. LDS-01 or LDS-02, depending on your LDS model.

```
echo 'export LDS_MODEL=LDS-02' >> ~/.bashrc
source ~/.bashrc
```

# Week-10

## Presentation

The proposal preparation was the focus of our effort this week.

# Week-11

## Updated Presentation

https://github.com/sanjiblama28/Smart_Team/blob/main/SMART_MOBILITY_Final_Porposal_GroupC.pptx




























