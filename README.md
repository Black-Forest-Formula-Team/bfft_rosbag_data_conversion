# bfft_rosbag_data_conversion

<p align="center">
  <a href="https://blackforestformula.hs-offenburg.de/">
    <img alt="BFFT_Logo" title="BFFT" src="https://scontent-frt3-1.xx.fbcdn.net/v/t1.0-9/69419451_117866062911797_4569414645357477888_o.jpg?_nc_cat=107&ccb=1-3&_nc_sid=973b4a&_nc_ohc=b5rqMomf8_AAX8x_CMD&_nc_ht=scontent-frt3-1.xx&oh=7ab30784f93fdf5ad846196156f856e6&oe=606D20C4" width="1000">
  </a>
</p>

# Black Forest Formula Team - Convert ROSBAGs to CSV files (one per ROS topic)
This package converts ROSBAGs into CSV files (relatively stable and performant) or an SQL database (quite slow, not recommended). There is one CSV file per ROS topic inside the ROSBAG with the option to include/exclude topics. For example: You might want to exclude all data that is coming from cameras like pixels and want to include topics like "orientation", "location", ... (everything you would want in a relational database/file).

This code is created and maintained by the [Black Forest Formula Team](https://blackforestformula.hs-offenburg.de/) at [University of Applied Sciences Offenburg](https://www.hs-offenburg.de/). As this is a subrepository of the overall system you will find more information about the system in the main repository and its [Wiki](https://github.com/Black-Forest-Formula-Team/bfft_formula-student_driverless/wiki).

____________________


## Repository organisation
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Introduction](#introduction)
- [Hardware & Software Preconditions](#hardware--software-preconditions)
  - [Hardware Stack](#hardware-stack)
  - [Software Stack](#software-stack)
- [Setup](#setup)
- [Getting started](#getting-started)
  - [Clone packages](#clone-packages)
  - [Build Process](#build-process)
  - [Connecting IMU and sensors via CAN bus](#connecting-imu-and-sensors-via-can-bus)
- [Code Repository Conventions](#code-repository-conventions)
- [Feedback](#feedback)
- [Our Developers](#our-developers)
- [Release History](#release-history)
- [Meta](#meta)
- [Contributing to one of our Repos](#contributing-to-one-of-our-repos)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
____________________
## Introduction
We are developing this package for the autonomous infrastructure of our first electric racecar since we would like to be able to visualize all data coming in through our sensors and actors in (close to) realtime after each run on the race track (or testing). One example might be to display the acceleration after the drive to be able to make data-driven desicions to adjust the chassis or control algorithm. Another one might be feedback for the driver, powertrain adjustments, etc. The goal is to be able to visualize everything on a Windows PC since these OS are commonly used by other students & engineers and mandatory for some programs (like Matlab).

____________________
## Hardware & Software Preconditions
We are using a NVIDIA Jetson AGX Xavier for our system. More input on or hardware setup you can find in the [Wiki](https://github.com/Black-Forest-Formula-Team/bfft_formula-student_driverless/wiki/01-Setup-of-NVIDIA-Jetson-AGX-Xavier) of our main repository. This package depends less on the hardware setup as more on the following software.

### Software Stack
* Ubuntu 18.04 LTS
* Python 3.X
* ROS 1 Melodic
* JetPack 4.4
____________________
## Setup
For our Hardware and Software setup please visit the page [Setup of NVIDIA Jetson AGX Xavier](https://github.com/Black-Forest-Formula-Team/bfft_formula-student_driverless/wiki/01-Setup-of-NVIDIA-Jetson-AGX-Xavier) in our Wiki. There you will find a guide how to install ROS Melodic, relevant drivers and libraries. You can also find a guide there how to enable CAN on the NVIDIA Jetson Board and how to wire it to a CAN sensor using a CAN-transceiver.
____________________
## Getting started
Assuming you are used to ROS1, have your hardware wired and/or followed the Wiki pages mentioned above you should now be able to clone this package into your catkin workspace (we assume it lays under ```cd ~/catkin_ws/```, you might have to adjust this to your needs). 

### Clone packages
To do so clone this package into your workspace and make sure you installed the necessary libaries ([Link to Wiki](https://github.com/Black-Forest-Formula-Team/bfft_formula-student_driverless/wiki/01-Installation-Libraries-for-Workspace-(Python-and-ROS))).


* [bfft_rosbag_data_conversion](https://github.com/Black-Forest-Formula-Team/bfft_rosbag_data_conversion): Take data recorded in [ROSBAGS](https://wiki.ros.org/rosbag) (internal data format) and export it into CSV files (one per topic). Use CSV files for data visualization purpose
```
cd ~/catkin_ws/src/
git clone https://github.com/Black-Forest-Formula-Team/bfft_rosbag_data_conversion.git
```

For more input please refer to the [Catkin Docs](https://wiki.ros.org/catkin/workspaces)

### Build Process
Now we are able to build the workspace (if we have all libraries installed) with the packages downloaded above. 
```
catkin_make
```
If a library is missing make sure to install it via ```sudo apt-get install ros-melodic-libraryname``` if its a ROS library or via ```pip3 install libraryname``` if its a python3 lib.

Source setup file to be able to execute ros commands from every terminal
```
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

## Convert ROSBAG to CSV
First, you have to adjust the path were your recorded ROSBAGs are located. To do so open the ```ROSBAG_to_CSV.launch``` and change the variables ```rb_path``` and ```rb_name```. If you like you can also add topics to include and exclude in this file.

To start the conversion process now type in the following:
```
roslaunch bfft_rosbag_data_conversion ROSBAG_to_CSV.launch
```

Now, a new folder should be available in the folder the ROSBAG was located with the same name as the ROSBAG. This folder now holds one CSV file per topic you decided to include in the launch file. 
![Rosbag to csv](https://github.com/Black-Forest-Formula-Team/bfft_formula-student_driverless/blob/main/demo/convert-ROSBAG-to-CSV.gif)

## Conversion using Bash Script
You can even use the convenience script to do so as described [here]() by executing
```sh bfft_scripts/rosbagToCSV.sh```
after downloading our script repository.

```
cd 
git clone https://github.com/Black-Forest-Formula-Team/bfft_scripts.git
```

This process is displayed in the gif below.
![ROSBAG to CSV](https://github.com/Black-Forest-Formula-Team/bfft_formula-student_driverless/blob/main/demo/convert-ROSBAG-to-CSV-script.gif)

________________________________
## Code Repository Conventions
For our coding conventions please visit the wiki page [ROS & Python Conventions](https://github.com/Black-Forest-Formula-Team/bfft_formula-student_driverless/wiki/00-Coding-Conventions)!

____________________
## Feedback

Feel free to send us feedback! 

If there's anything you'd like to chat about, please feel free to text us on one of our social media plattforms: 
* [Instagram](https://www.instagram.com/black_forest_formula/) 
* [Facebook](https://www.facebook.com/blackforestformula/) 
* [LinkedIN](https://linkedin.com/company/20527126)!

Support this project by becoming a sponsor. Your logo will show up on our website with a link to your website. [[Become a sponsor](https://blackforestformula.hs-offenburg.de)]

____________________
## Our Developers
Dev-Team Vehicle Control Unit & Autonomous Driving in alphabetical order
* [Alex](https://github.com/AlexSperka) - Initial work
* [Benedikt](https://github.com/newtop95) - Initial work
* [Steffi](https://github.com/steffistae) - Initial work
* [Tizian](https://github.com/tdagner) - Initial work

____________________
## Release History
* 0.0.1
    * Initial setup, work in progress

____________________
## Meta
Distributed under the MIT license. See ``LICENSE.md`` for more information.

____________________
## Contributing to one of our Repos
1. Fork it (<https://github.com/Black-Forest-Formula-Team/bfft_can_bus_msgs_to_ros_topic/fork>)
2. Create your feature branch (`git checkout -b feature/fooBar`)
3. Commit your changes (`git commit -am 'Add some fooBar'`)
4. Push to the branch (`git push origin feature/fooBar`)
5. Create a new Pull Request

____________________


<p align="left">
  <a href="https://www.hs-offenburg.de/">
      <img alt="HSO_Logo" title="HSO_Logo" src="https://static.onthehub.com/production/attachments/15/66edb074-5e09-e211-bd05-f04da23e67f6/7978f7db-e206-4cd7-b7b2-6d9696e98885.png" width="1000">
  </a>
</p>
