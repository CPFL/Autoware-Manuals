# Nagoya University

# Autoware User’s Manual - Document Version 1.1

## For Autoware *ver.2016.Sep.12*

## Table of contents

1. [*About This Document*](#chapter-1---about-this-document)
2. [*ROS and Autoware*](#chapter-2---ros-and-autoware)
  - [*Robot middleware - ROS*](#robot-middleware---ros)
  - [*ROS Features*](#ros-features)
  - [*Autoware*](https://github.com/khanhgithead/Autoware-Manuals/blob/markdown/en/Autoware_UsersManual_v1.1.md#autoware)
  - [*3-D Map Generation and Sharing*](#3-d-map-generation-and-sharing)
  - [*Localization(NDT：Normal Distributions Transform)*](#localization-ndtnormal-distributions-transform)
  - [*Object Detection*](#object-detection)
  - [*Route Generation*](#path-generation)
  - [*Autonomous Driving*](#autonomous-driving)
  - [*User Interface*](#user-interface)
  - [*Platform structure for Autoware*](#platform-structure-for-autoware)
  - [*Perception/Recognition*](#perceptionrecognition)
  - [*Judgement/Operation/Localization*](#judgementoperationlocalization)
  - [*Path Planning*](#path-planning)
  - [*Data Loading (3-D Map, Database, Files)*](#data-loading-3-d-map-database-files)
  - [*Device Drivers and Sensor Fusion*](#device-drivers-and-sensor-fusion)
  - [*Interface for Smart Phone Applications*](#interface-for-smart-phone-applications)
  - [*Utilities and Others*](#utilities-and-others)
3. [*Operating Autoware*](#chapter-3---operating-autoware)
  - [*Preparations*](#preparations)
  - [*Demo Data*](#demo-data)
  - [*Runtime Manager Launching*](#runtime-manager-launching)
  - [*RViz Configuration*](#rviz-configuration)
  - [*Operating Quick Start*](#operating-quick-start)
  - [*Load map(Quick Start)*](#load-map-quick-start)
  - [*Load driver(Quick Start)*](#load-driver-quick-start)
  - [*Autoware Main Functions*](#autoware-main-functions)
  - [*Localization(NDT：Normal Distributions Transform)*](#localization-ndtnormal-distributions-transform-1)
  - [*Object Detection*](#object-detection-1)
  - [*Path Planning*](#path-planning-1)
  - [*Path Following*](#path-following)
  - [*Dynamic Map*](#dynamic-map)
  - [*AutowareRider*](#autowarerider)
  - [*AutowareRoute*](#autowareroute)
  - [*Features*](#features)
  - [*AutowareRider Launching*](#autowarerider-launching)
  - [*Path Planning Application Usage*](#path-planning-application-usage)
  - [*Send Path data to ROS PC*](#send-path-data-to-ros-pc)
  - [*CAN Collection Application Usage Data*](#can-collection-application-usage-data)
  - [*Send CAN Data to a ROS PC*](#send-can-data-to-a-ros-pc)
  - [*Start Launch File*](#start-launch-file)
4. [*Autoware User Interface Details*](#chapter-4---autoware-user-interface-details)
  - [*Runtime Manager*](#runtime-manager)
  - [*Runtime Manager – Quick Start Tab*](#runtime-manager-quick-start-tab)
  - [*Runtime Manager – Setup Tab*](#runtime-manager-setup-tab)
  - [*Runtime Manager – Map Tab*](#runtime-manager-map-tab)
  - [*Runtime Manager – Sensing Tab*](#runtime-manager-sensing-tab)
  - [*Runtime Manager – Computing Tab*](#runtime-manager-computing-tab)
  - [*Runtime Manager – Interface Tab*](#runtime-manager-interface-tab)
  - [*Runtime Manager – Database Tab*](#runtime-manager-database-tab)
  - [*Runtime Manager – Simulation Tab*](#runtime-manager-simulation-tab)
  - [*Runtime Manager – Status Tab*](#runtime-manager-status-tab)
  - [*Runtime Manager - Topics Tab*](#runtime-manager---topics-tab)
  - [*ROSBAG Record Dialog*](#rosbag-record-dialog)
  - [*RViz* **エラー! ブックマークが定義されていません。**](#_Toc464046987)
  - [*AutowareRider*](#autowarerider-1)
  - [*AutowareRoute*](#autowareroute-1)
5. [*System Setup*](#chapter-5---system-setup)
  - [*Installation*](#installation)
  - [*OS*](#os)
  - [*ROS*](#ros)
  - [*OpenCV*](#opencv)
  - [*CUDA(if necessary)*](#cudaif-necessary)
  - [*FlyCapture(if necessary)*](#flycaptureif-necessary)
  - [*Autoware*](#autoware-1)
  - [*AutowareRider(if necessary)*](#autowarerider-if-necessary)
  - [*canlib(if necessary)*](#canlibif-necessary)
  - [*SSH Public Key Generation(if necessary)*](#ssh-public-key-generationif-necessary)
6. [*Terminology*](#chapter-6---terminology)
7. [*Related Documents*](#chapter-7---related-documents)


# Chapter 1 - About This Document

*This chapter describes the purpose of this document.*

There are two documents provided by Nagoya University:

- Autoware User’s Manual
- Autoware Developer’s Manual

# Chapter 2 - ROS and Autoware

*Before operating Autoware, ROS and Autoware are described in this chapter.*

## Robot middleware - ROS

Recently, the wide range potential of robotics has been focused by not only robotics experts but also non-robotics experts to join robot development. It is believed that this trend leads robotics to advancement and developments to other domains. However, robot development is getting harder because the advancement and the complexity of robot functions have been increased. Unlike PCs and smartphones, robotic development has considered various hardware, OS, programming languages. Hence, the differences have mainly been obstructed for robotic developers as well as robotics experts to join robot development.

To solve the problem, the demand of making common platforms has been increased, and some platforms have been published. Within a common platform, developers can combine various software published by other developers, and speed up development by reusing them. Therefore, it is expected that developers can more focus on fields of interest.

ROS (Robot Operating System) is a framework for robotic software development. It was developed by [Willow Garage](https://en.wikipedia.org/wiki/Willow_Garage) in U.S. OSFR (Open Source Robotics Foundation) has currently maintained it. The open sourced ROS has mainly been used by communities in U.S. and Europe as well as communities in Japan.

Note that ROS has “OS” in its name, however, it is not “OS” like Windows or Linux. It is a middleware that runs on UNIX based OS.

### ROS Features

1.  *Library and tools*.  ROS provides library and tools for robotic software development. The primary libraries and tools are listed as follows:
  - Original build system (Catkin)
  - Image processing library (OpenCV)
  - Data logging tool (ROSBAG)
  - Visualization tools for data and software state (RViz)
  - Coordinate transformation library (TF)
  - Qt based GUI development tool (RQT)

2.  *Inter-process communication*.  ROS uses *message passing* with topics of publish/subscribe form for inter-module connection/cooperation frameworks. Here, message passing is an inter-process communication mechanism in which a sender can send data to one or more receivers. This feature enables us to design distribution systems. In ROS, processes called *node* are launched and each node is run independently. In communication between nodes, by following the publish/subscribe scheme, a node writes messages (publish) into a topic and another node reads the messages (subscribe) of the topic.

3.  File components
  -  *bag file (ROSBAG)*.  In ROS, all the messages of topics are recorded and time-stamped into a .bag file called *ROSBAG*. This file can be used for replaying the messages on RViz as same timing as recording. In robotic development, it is difficult to synchronize and analyze the interactions with multiple sensors at the same time, but ROS enables efficient analysis and debugging robotic systems. In addition, since the recorded messages can be replayed repeatedly, this feature allows developers to debug their systems without actual sensors.
  -  *Launch file*.  A “Launch” file is used to start multiple nodes at the same time. The launch file contains the nodes to be started and their parameters written in XML format.

## Autoware

Autoware is open source software based on ROS. Autoware is pushed on Github for autonomous driving research and development. Most of autonomous driving system consist of *recognition*, *judgment*, and *operation*. Autoware provides necessary functions, such as 3-D map generation, localization, object recognition, and vehicle control, for autonomous driving.

![Figure 1 - Autoware Overview](/en/imgs/fig1.png)

Autoware uses LIDAR (Light Detection and Ranging) and on-vehicle cameras to localize the ego-car position. In addition, Autoware can detect surrounding objects, such as pedestrians, vehicles, traffic lights etc., by using LIDAR and GNSS (Global Navigation Satellite System).  The making judgments of driving/stopping at lanes or intersections are performed with an embedded multi-core CPU. Operations of controlling vehicle behaviors utilize conventional on-vehicle control mechanism, while support systems such as driving assistance and safety diagnosis support, use multi-core CPU.

### 3-D Map Generation and Sharing

The most notable feature of Autoware is the fact that the autonomous driving of Autoware uses a 3-D map. The 3-D map, unlike 2-D maps used in car navigation system, is a map including various information, such as roads and all the still objects around the roads. The 3-D map will play a critical role in deploying autonomous driving into the real world.
Autoware localizes the position of the ego-vehicle by matching the 3-D map loaded into the PC with the surrounding information captured by the LIDAR on the roof. The precision of localization is approximately 10cm, which number indicates far higher precision than GPS.

As of Sep. 2015, the 3-D map covers the only limited area. If the ego-vehicle with Autoware enters into the area without the 3-D map, Autoware can generate a 3-D map in real-time from the information captured by LIDAR.The generated 3-D map in this way not only can be utilized for the developers using Autoware, but also it can be uploaded and shared on the Nagoya University servers. The feature enables online updating of the 3-D map. This mechanism allows for creating the 3-D map including every nook and corner of the city, such as tiny alleys that the specialized vehicles for 3-D mapping can not enter. In addition, the geographic data from the 3-D map can generate a 3-D map of vector format.

### Localization (NDT：Normal Distributions Transform)

The position of the ego-vehicle can be located by scan matching based on the NDT algorithm with the 3-D map of PCD (Point Cloud Data) format and LIDAR data. The position error of localization is around 10cm.

### Object Detection

DPM (Deformable Part Models) algorithms can detect cars, pedestrians and traffic lights from camera images. Tracking using Kalman filter can be implemented and it improves detection accuracy. Fusing 3-D LIDAR data can obtain the distances of the detected objects. Traffic lights detection is conducted as following steps. First, the 3-D positions of
traffic lights are calculated by using the localization result and the high-definition 3-D map. Next, the 3-D positions of traffic lights are projected on camera images by sensor fusion. Then, traffic light colors are recognized by image processing.

### Path Generation

AutowareRoute (a route data generation application using MapFan) generates a path to the selected destination. Then, autonomous driving system determines the lanes on the path. The path data generation application is provided as a car navigation of smartphone. The trajectories in the lanes are calculated kinematically.

### Autonomous Driving

The generated path includes appropriate speed information. Autonomous driving system uses it as the target speed. In addition, the route includes landmarks, “way point”, set at 1m intervals. The autonomous driving system operates “path following” by following the way points. Referring near way points on curves and distant way points on straights stabilizes autonomous driving. If ego-vehicle deviates from the route, the system aims to the vicinity point and back to the target route. Note that the safest path is selected by the driving system.

### User Interface

A user interface called “Runtime Manager” of Autoware enables developers to operate functions, such as localization, object detection, and path following, easily. In addition, RViz can integrate and visualize localization on 3-D map, object detection, path planning, and path following. Furthermore, a tablet user interface, “AutowareRider”, of Autoware enables navigation, path planning, transition to autonomous driving mode and etc., on tables, easily. Moreover, Autoware can visualize 3-D map used in autonomous driving system and project it on on-vehicle displays and Oculus devices.

![Figure 2 - User Interface](/en/imgs/fig2.png)

## Platform structure for Autoware 

Autoware is an application using ROS and ROS only works on Unix-based platforms. Figure 3 illustrates the overall system structure for Autoware.

![Figure 3 - Platform Stucture for Autoware](/en/imgs/fig3.png)

### Perception/Recognition

```shell
ros/src/computing/perception/detection
```
The figure to be updated (tmp)

![Figure 4 - Perception and Recognition](/en/imgs/fig4.png)

### Judgement/Operation/Localization
```shell
ros/src/computing/perception/localization
```
![Figure 5 - Judgement, Operation, and Localization](/en/imgs/fig5.png)

### Path Planning
```shell
ros/src/computing/planning
```
The figure to be updated (tmp)

![Figure 6 - Path Planning](/en/imgs/fig6.png)

### Data Loading (3-D Map, Database, Files)
```shell
ros/src/data
```
![Figure 7 - Data Loading](/en/imgs/fig7.png)

### Device Drivers and Sensor Fusion
```shell
ros/src/sensing/drivers & ros/src/sensing/fusion
```
The figure to be updated (tmp)

![Figure 8 - Device Drivers and Sensor Fusion](/en/imgs/fig8.png)

### Interface for Smart Phone Applications
```shell
ros/src/socket
```
The figure to be updated (tmp)

![Figure 9 - Interface for Smart Phone Applications](/en/imgs/fig9.png)

### Utilities and Others
```shell
ros/src/util/ # Runtime Manager, sample data, pseudo-drivers
ui/tablet/ # Smart phone applications
vehicle/ # Vehicle control, vehicle data acquisition
```

# Chapter 3 - Operating Autoware

*The main function operations of Autoware are described in this chapter.  This description assumes operating by Runtime Manager. Please refer Chapter 4 for user inter face details.*

## Preparations

Main functions of Autoware can easily be operated by Quick Start tab in Runtime Manager. Nagoya University has provided sample demo data for Quick Start. Preparation for the demo is described in the following sections.

### Demo Data

Operations by Quick Start tab in Runtime Manager are described in this section.  Here, it is assumed that the required data is put in \~/.autoware/data.

1. *Download*.  Download the demo data from the following sites, and put them in \~/.autoware/data

  -  Script for generating launch files
	[*http://db3.ertl.jp/autoware/sample\_data/my\_launch.sh*](http://db3.ertl.jp/autoware/sample_data/my_launch.sh)
	Data of map (Moriyama area), calibration and route
	[*http://db3.ertl.jp/autoware/sample\_data/sample\_moriyama\_data.tar.gz*](http://db3.ertl.jp/autoware/sample_data/sample_moriyama_data.tar.gz)
	ROSBAG data
	[*http://db3.ertl.jp/autoware/sample\_data/sample\_moriyama\_150324.tar.gz*](http://db3.ertl.jp/autoware/sample_data/sample_moriyama_150324.tar.gz)
	ROSBAG data does not contain video data for object detection.

2. *Extraction*.  Extract the downloaded data to \~/.autoware/ by the following command:
```shell
$ tar xfz sample\_moriyama\_data.tar.gz -C ~/.autoware/
```
3. *Launch script*.  Run the following script to generate lacunh files for playing demo by Qutick Start tab.
```shell
$ sh my_launch.sh
```
4. *Launch files to be generated*.  
	```shell
	my_map.launch # Load maps
	my_sensing.launch # Load drivers
	my_localization.launch # Localization
	my_detection.launch # Object detection
	my_mission_planning.launch # Path planning
	my_motion_planning.launch # Path following
	```

5. *If you want to generate launch files to other directories*.  If you want to generate launch files in other directories, specify the path as an argument for launching the script.  Example:　if you put the data in 
``` shell
~/.autoware/data/quick_start/ROSBAG_sample/
```

### Runtime Manager Launching

1. Runtime Manager can be launched by double-clicking `Autoware/ros/run` in ROS PC. Alternatively, it can be launched by `./run` on a terminal.  The run file contains shell scripts.  Starting run, two terminals are launched.  The one is for roscore, the other is for the output of runtime manager.

2. Enter login password and press \[OK\] on the displayed password dialog.

![Figure 10 - Password Dialog for Administrative Privileges](/en/imgs/fig10.png)

### RViz Configuration

1.  Launch RViz by pressing \[RViz\] on the right-bottom in Runtime Manager.

Select \[File\] - \[Open Config\] in the \[RViz\] menu.

Select the following Config file and press \[Open\] on the dialog displayed by pressing \[Choose a file to open\].
Autoware/ros/src/.config/RViz/default.RViz

## Operating Quick Start

 Steps of main function operations of Autoware by Quick Start in Runtime Manager are described in the following sections.

### Load map (Quick Start)

1.  Load 3-D map (Point Cloud) and Vector map (Vector Map)
Specify the `my_map.launch` generated in the *Demo Data* section to the file selection dialog of \[Map\] in the \[Quick Start\] tab and press Map.

### Load driver (Quick Start)

1.  Specify the `my_sensing.launch` generated in the *Demo Data* section to the file selection dialog of \[Sensing\] in the \[Quick Start\] tab and press Sensing.

## Autoware Main Functions

### Localization (NDT：Normal Distributions Transform)

How to use a ROSBAG is described in this section.

1. Specify the following ROSBAG by the file selection dialog in \[Simulation\] tab, and press \[Play\]. Press \[Pause\] to suspend ROSBAG immediately.

    ```shell
    ~/.autoware/sample_moriyama_150324.bag
    ```

2. Specify the my\_map.launch generated in the Demo Data section to the file selection dialog of \[Map\] in the \[Quick Start\] tab, and press Map.

3. Specify the my\_localization.launch generated in the Demo Data section to the file selection dialog of \[Localization\] in the \[Quick Start\] tab, and press \[Localization\].

4. Pressing \[Pause\] in the \[Simulation\] tab to resume the ROSBAG, a map is displayed. If NDT is run, the results are also displayed. If nothing is displayed, press \[Reset\] in the RViz, or remove and set checks of \[Points Map\] and \[Vector Map\] in the Display. Figure 12 shows the loaded map and the localized vehicle on RViz.  Note: Localization is not stable until 23% (110/479 second) of the progress bar displayed in the Simulation tab, because the demo ROSBAG does not include maps.

![Figure 11 - Localization](/en/imgs/fig11.png)

5. If the results of localization do not follow the GNSS arrow, press \[2D Pose Estimate\] in the top of RViz, move the mouse cursor and click the GNSS arrow.

6. Selecting \[ThirdPersonFollower(RViz)\] in the \[TopDownOrtho(RViz)\] on the right pane in RViz, Figure 12 view can be obtained.

![Figure 12 - Thirdperson Follower](/en/imgs/fig12.png)

### Object Detection

1.  Specify the `my_detection.launch` generated in the Demo Data section to the file selection dialog of \[Detection\] in the \[Qutick Start\] tab, and press \[Detection\].

2.  If object detection succeeds in running NDT, vehicles will be displayed as blue spheres, and pedestrians will be green spheres.

NOTE: the demo ROSBAG data does not contain video data for object detection.

![Figure 13 - Object Detection](/en/imgs/fig13.png)

### Path Planning

![Figure 14 - Path Planning](/en/imgs/fig14.png)

1.  Specify the `my_mission_planning.lauch` generated in the Demo Data section to the file selection dialog of \[Mission Planning\] in the \[Quick Start\] tab and press \[Mission Planning\].

2.  The path and the target speed are displayed in blue line.

### Path Following

![Figure 15 - Path Following](/en/imgs/fig15.png)

1.  Specify the `my_motion_planning.launch` generated in the Demo data section to the file selection dialog of Motion Planning in the Quick Start tab, and press \[Motion Planning\].

2.  If the specified path by path planning is coming in the displaying window, blue spheres on the path, and red circles calculated by Pure Pursuit are displayed.

### Dynamic Map

Dynamic map is a method of sharing car and pedestrian information, which is recognized by the ego vehicle or other vehicles, through Nagoya University database server.

1. Ensure the following topics are in Publish

	All are not necessary. If the following topics are published, register their information to the database.

	`/current_pose` (ego vehicle, /vel\_pose\_mux in Publish)
	`/obj_car_pose` (other vehicles, /obj\_fusion in Publish)
	`/obj_person_pose` (pedestrains, /obj\_fusion in Publish)

![Figure 16 - pos_uploader App Dialog](/en/imgs/fig16.png)

2. Click \[Position\] -&gt; \[pos\_uploader\] -&gt; \[app\] in the \[Database\] tab on the providing information side, enter information to access database server by SSH, and press \[OK\]. 
	(Generating SSH key is described in the generating SSH public key section)

3. Check \[Position\] -&gt; \[pos\_uploader\] (launch nodes) in the \[Database\] tab on the sending information side.

4.  Click \[Position\] -&gt; \[pos\_uploader\] -&gt; \[app\] in the \[Databse\] tab on the reviewing information side, enter information to access database server by SSH, and press \[OK\]. Checking \[show my pose\], the ego vehicle position is published.

![Figure 17 - pos_downloader App Dialog](/en/imgs/fig17.png)

5.  Check \[Position\] -&gt; \[pos\_downloader\] in the \[Database\] tab on “RViz” in the reviewing information side.

6.  Adding a topic of “Marker” of “/mo marker” on the reviewing information side, the recognized ego vehicle and other vehicles are displayed.

7.  The table, “*can”,* of the Nagoya university database (“VoltDB”) is defined as follows:

  **Name** | **Type** | **Description**
  -------- | -------- | ---------------
  id | varchar(32) | MAC address and terminal-specific information
  lon | Float | Latitude (Android terminal)
  lat | Float | Longitude (Android terminal)
  H | Float | Not used
  X | Float | Horizontal perpendicular x-axis (pos\_uploader)
  Y | Float  | Horizontal perpendicular y-axis (pos\_uploader)
  Z | Float | Horizontal perpendicular z-axis (pos\_uploader)
  area | Float | Horizontal perpendicular axis area number (pos\_uploader, fixed at 7)
  dir | Float | direction (Android terminal)
  acct\_x | Float | Acceleration x (pos\_uploader)
  acct\_y | Float | Acceleration y (pos\_uploader)
  acct\_z | Float | Acceleration z (pos\_uploader)
  vec | Float | Not used
  type | Smallint | 1=ego vehicle, 2=Detected vehicle, 3=Detected pedestrian, 0=Position of Android terminal
  self | Smallint | Not used
  tm | timestamp | Time stamp (GMT)

## AutowareRider

AutowareRider is an Android application to operate Autoware, which runs on ROS PCs, from tablet terminals. The UI is similar to Night Rider, which is a TV series.

### AutowareRoute

AutowareRoute is another Android application implemented with MapFan SDK for planning route data from a departure point to a destination point.

### Features

AutowareRider provides the following functions.

  - Send the route data generated by AutowareRoute to ROS PCs
  - Launch CAN data collection application
  - Start a launch file of ROS PC by pressing buttons
  - Update UI according to CAN data received from ROS PCs.

### AutowareRider Launching

1.  Launch Runtime Manager on an ROS PC.
2.  Press \[Active\] in \[Network Connection\] -&gt; \[Tablet UI\] on the Main Tab to launch the following nodes: 
```shell
	tablet_receiver
	tablet_sender
```
3.  Specify the following parameters in each \[app\] of \[Planning\] -&gt; \[Path\] in the \[Computing\] tab.

\[lane\_navi\]
vector\_map\_directory
A directory path of a high definition map

\[lane\_rule\]
vector\_map\_directory
A directory path of a high definition map

ruled\_waypoint\_csv
A path of a file saved waypoints
Velocity
Speed (unit: km/h, default: 40, range: 0\~200)

Difference around Signal
Acceleration around traffic lights (unit: km/h, default: 2, range: 0\~20)

\[lane\_stop\]
Red Light
Change to the velocity of red light

Blue Light
Change to the velocity of blue light

4. Launch the followings by turning on the checkbox of \[Planning\] -&gt; \[Path\] on the \[Computing\] tab.

\[lane\_navi\]
\[lane\_rule\]
\[lane\_stop\]

5.  Launch AutowareRider from the list of Android tablet applications.
6.  Specify the following parameters in the \[右上メニュー\] (left-top menu) -&gt; \[設定\] (setting).

ROS PC
IP address
ROS PC IPv4 address

Receiver port number
tablet\_receiver port number (default: 5666)

Sender port number
tablet\_sender port number (default: 5777)

7. Press \[OK\] to access to the ROS PC.
Here, the configuration is saved into a file automatically, and the file is used from next connection as default setting.

8. If the central bar on the window is light red, the connection is success.

Table 2 Bar color and connection status

**Bar color** | **Connection status**
------------- | ---------------------
Dark red | Not connected to ROS PC
Light red | Connected to ROS PC
Light blue | Autonomous driving (mode\_info: 1)
Light yellow | Error (error\_info: 1)

### Path Planning Application Usage

1.  Press \[NAVI\] on “AutowareRider” to launch *path planning*.
2.  Press and hold a point on the map in order of the followings:
  a.  出発地に設定 (set departure point)
  b.  目的地に設定 (set destination point)
  c.  ルート探索実行 (run route planning)

3.  *Path planning* after *route planning* is done, path data is sent to an ROS PC. The path data is saved automatically, and route planning is omitted from the next time.
4.  AutowareRider window is displayed after sending the path data.

### Send Path data to ROS PC

Refer the step ③ above for path planning application usage.

### CAN Collection Application Usage Data

1.  Specify the following parameters in \[右上メニュー\] (right top menu) -&gt; \[設定\] (setting) on the AutowareRider (tmp).

```
	データ収集 (data collection)
		Table name
			Destination table name

	SSH
		Hostname
			SSH destination hostname
		Port number
			SSH destination port number (default: 22)
		Username
			SSH login username
		Password
			SSH login password

	ポートフォワーディング (port forwarding)
		Local port number
			Port number of local machine source (default: 5558)
		Remote host name
			Remote machine host name (default: 127.0.0.1)
		Remote port number
			Port number of a remote machine (default: 5555)
```

2.  The configuration is saved by pressing \[OK\].

	Note: SSH password is not saved in the configuration file. The password is saved on the memory during running AutowareRider.

3.  Launch any one of the followings in \[右上メニュー\] (right top menu) -&gt; \[データ収集\] (data collection)

CanGather
CarLink (Bluetooth)
CarLink (USB)

4.  The usages after launching applications are same as launching applications individually. Refer the below URL.

	https://github.com/CPFL/Autoware/blob/master/vehicle/general/android/README.md

### Send CAN Data to a ROS PC

Refer the step ④ in CAN Data Collection Usage (tmp).

### Start Launch File

1.  \[S1\] and \[S2\] of AutowareRider starts the following launch files, respectively (tmp).

	```shell
	check.launch
	set.launch
	```

	Pressing each button, the corresponding launch file is started on an ROS PC.

	Table 2 Buttons and launch file status

  **Button** | **Launch file status**
  ---------- | ----------------------
  Pressed (black) | Start ({ndt,if}\_stat: false)
  Pressed (red) | Start ({ndt,if}\_stat: true)

# Chapter 4 - Autoware User Interface Details

## Runtime Manager

Runtime Manager is a Python script (scripts/runtime\_manager\_dalog.py) included in the runtime\_manager package. It can be launched by the following command:

```shell
	$ rosrun runtime_manager runtime_manager_dialog.py
```

Runtime Manager dialog composes of multiple tabs. Operating Runtime Manager dialog enables processes of starting/ending ROS nodes used in Autoware, and publishing topics for parameters of the launched ROS. The buttons to start/end ROS nodes are classified into functionalities and placed in different tabs. Each Tab window can be switched by pressing each Tab on the top of the Runtime Manager dialog.

## Runtime Manager – Quick Start Tab

![Figure 18 - Runtime Manager - Quickstart Tab](/en/imgs/fig18.png)

-   **\[Map\]** … Start/end a launch script specified by full path in the \[Map\] text box. Alternatively, a script can be selected from the file selection dialog displayed by pressing the \[Ref\] button.
-   **\[Sensing\]** … Start/end a launch script specified by full path in the \[Sensing\] text box. Alternatively, a script can be selected from the file selection dialog displayed by pressing the \[Ref\] button.
-   **\[Localization\]** … Start/end a launch script specified by full path in the \[Localization\] text box. Alternatively, a script can be selected from the file selection dialog displayed by pressing the \[Ref\] button.
-   **\[Detection\]** … Start/end a launch script specified by full path in the \[Detection\] text box. Alternatively, a script can be selected from the file selection dialog displayed by pressing the \[Ref\] button.
-   **\[Mission Planning\]** … Start/end a launch script specified by full path in the \[Mission Planning\] text box. Alternatively, a script can be selected from the file selection dialog displayed by pressing the \[Ref\] button.
-   **\[Motion Planning\]** … Start/end a launch script specified by full path in the \[Motion Planning\] text box. Alternatively, a script can be selected from the file selection dialog displayed by pressing the \[Ref\] button.
-   **\[Android Tablet\]** … Start/end the script runtime\_manager/tablet\_socket.launch for communicating with Tablets.
-   **\[Oculus Rift\]** … &lt;Unimplemented&gt;
-   **\[Vehicle Gateway\]** … Start/end the script runtime\_manager/vehicle\_socket.launch for communicating with ZMP Robocar.
-   **\[Cloud Data\]** … Start/end obj\_db/obj\_downloader nodes.
-   **\[Auto Pilot\]** … Publish /mode\_cmd topics according to the status of buttons.
-   **\[ROSBAG\]** … Display ROSBAG Record dialogs for ROSBAG.
-   **\[RViz\]** … Start/end RViz.
-   **\[RQT\]** … Start/end RQT. 

## Runtime Manager – Setup Tab

![Figure 19 - Runtime Manager - Setup Tab](/en/imgs/fig19.png)

### \[Baselink to Localizer\]

-   **\[TF\]** … Pressing \[TF\], a topic of Vehicle control position (base\_link → Velodyne position(velodyne)is published.
-   **\[x\], \[y\], \[z\], \[yaw\], \[pitch\], \[roll\]** … Enter the relative position of velodyne to base\_link.

###  \[Vehicle Model\]

-  **\[Vehicle Model\]** … Start/end a script
*model\_publisher/vehicle\_model.launch* with the argument, which is the full path of URDF(Unified Robot Description Format)file(a vehicle model displayed on RViz)specified in the text box. Alternatively, a URDF file can be selected from a file selection dialog displayed by pressing \[Ref\]. If a URDF file is not specified in the \[Vehicle Model\] text box, *Autoware/ros/src/.config/model/default.urdf* is used.
-   **\[ROSBAG\]** … Display a ROSBAG Record dialog.
-   **\[RViz\]** … Start/end RViz.
-   **\[RQT\]** … Start/end RQT.

## Runtime Manager – Map Tab

![Figure 20 Runtime Manager - Map Tab](/en/imgs/fig20.png)

Figure Runtime Manager - Map Tab

-   **\[Point Cloud\]** … Start/end the *map\_file/points\_map\_loader* nodes with the argument, which is the full path of 3-D map (PCD files) specified in the text box. Alternatively, a 3-D map can be selected by the file selection dialog displayed by pressing \[Ref\].
-   **\[Auto Update\]** … Specify automatic update status (enable or not) at launching *map\_file/points\_map\_loader*. The number of scenes on automatic update can be specified by selecting an item in the drop-down box. This configuration is effective only if the check box of *Auto Update* is on. Specify the full path of an area list file in the Area List text box, which is used as an argument when launching *map\_file/point\_map\_loader*. Alternatively, an area list file can be selected by file selection dialog displayed by pressing \[Ref\].
-   **\[Vector Map\]** … Start/end the map\_file/vector\_map\_loader nodes with an argument, which is the full path of Vector map (csv files) specified in the text box. Alternatively, a Vector map can be selected by file selection dialog displayed by pressing *Ref*.
-   **\[TF\]** … Start/end the launch file specified in full path in the text box. If launch file is not specified, the launch file *\~/.autoware/data/tf/tf.launch* is started/ended. Alternatively, a launch file can be selected by file selection dialog displayed by pressing *Ref*.

### \[Map Tools\]

-   **\[PCD Filter\]** …Start/end map\_tools/pcd\_filter nodes with an argument, which is the full path of 3-D map (PCD files) specified in the text box. Alternatively, a 3-D map can be selected by file selection dialog displayed by pressing *Ref*.
-   **\[PCD Binarizer\]** …Start/end map\_tools/pcd\_binalizer nodes with an argument, which is the full path of ASCII format PCD file specified in the text box. Alternatively, a PCD file can be selected by file selection dialog displayed by pressing *Ref*.
-   **\[ROSBAG\]** … Display a ROSBAG Record dialog.
-   **\[RViz\]** … Start/end RViz.
-   **\[RQT\]** … Start/end RQT.

## Runtime Manager – Sensing Tab

![Figure 21 - Runtime Manager - Sensing Tab](/en/imgs/fig21.png)

### \[Drivers\]

#### \[CAN\]
-  **\[can\_converter\] check box** … Start/end *kvaser/can\_converter* nodes.
-  **\[can\_draw\] check box** … Start/end *kvaser/can\_draw* nodes.
-  **\[can\_listener\] check box** … Start/end *kvaser/can\_listener* nodes.
-  **\[can\_listener\]-\[config\]** … Display a *can\_listener* dialog. Specify channels to be selected at launching nodes.
#### \[Cameras\]
-  **\[PointGrey Grasshoper 3 (USB1)\] check box** … Start/end a *pointgrey/grasshopper3.launch* script.
-  **\[PointGrey Grasshoper 3 (USB1)\] - \[config\]** … Display a *calibration\_path\_grasshopper3* dialog. Specify a CalibrationFile path to be selected at launching scripts.
-  **\[PointGrey Generic\] check box** … Start/end a *pointgrey\_camera\_driver/camera.launch* script.  **\[PointGrey PointGray LadyBug 5\]** **check box**… Start/end a *pointgrey/ladybug.launch* script.
-  **\[PointGrey PointGray LadyBug 5\] -\[config\]** … Display a *calibration\_path\_ladybug* dialog. Specify a CalibrationFile path to be selected at launching scripts.
-  **\[USB Generic\] check box** … Start/end *runtime\_manager/uvc\_camera\_node* nodes.
-  **\[IEEE1394\] check box** … &lt;Unimplemented&gt;
-  **\[Baumer VLG-22\] check box** … Start/end a *vlg22c\_cam/baumer.launch* script.
#### \[GNSS\]
-  **\[Javad Delta 3(TTY1)\] check box** … Start/end a *javad\_navsat\_driver/gnss.sh* script.
-  **\[Javad Delta 3(TTY1)\]-\[config\]** … Display a serial dialog. Specify parameters related to RS232C.
-  **\[Serial GNSS\] check box** … Start/end a *nmea\_navsat/nmea\_navsat.launch* script.  **\[Serial GNSS\]-\[config\]** … Display a serial dialog. Specify parameters related to RS232C.
#### \[IMU\]
-  **\[Crossbow vg440\] check box** … &lt;Unimplemented&gt;
#### \[LIDARs\]
-  **\[Velodyne HDL-64e-S2\] check box** … Start/end a *velodyne\_pointcloud/velodyne\_hdl64e\_s2.launch* script.
-  **\[Velodyne HDL-64e-S2\]-\[config\]** … Display a calibration\_path dialog. Specifya CalibrationFile path to be selected at launching scripts.
-  **\[Velodyne HDL-64e-S3\] check box** … Start/end a *velodyne\_pointcloud/velodyne\_hdl64e\_s3.launch* script.
-  **\[Velodyne HDL-64e-S3\]-\[config\]** … Display a calibration\_path dialog. Specify a CalibrationFile path to be selected at launching scripts.
-  **\[Velodyne HDL-32e\] check box** … Start/end a *velodyne\_pointcloud/velodyne\_hdl32e.launch* script.
-  **\[Velodyne HDL-32e\]-\[config\]** … Display a calibration\_path dialog. Specify a CalibrationFile path to be selected at launching scripts.
-  **\[Velodyne VLP-16\] check box** … Start/end a *velodyne\_pointcloud/velodyne\_vlp16.launch* scripts.
-  **\[Velodyne VLP-16\]-\[config\]** … Display a calibration\_path dislog. Specify a CalibrationFile path to be selected at launching script.
-  **\[Hokuyo TOP-URG\] check box** … Start/end a *hokuyo/top\_urg.launch* script.
-  **\[Hokuyo 3D-URG\] check box** … Start/end a *hokuyo/hokuyo\_3d\_urg.launch* script.
-  **\[SICK LMS511\] check box** … &lt;Unimplemented&gt;
-  **\[IBEO 8L Single\] check box** … &lt;Unimplemented&gt;

#### \[Points Filter\]

*A chapter image and the description to be updated (tmp)*

-  **\[Calibration Tool Kit\]** … Start/end *Calibration\_camera\_lidar/calibration\_toolkit* nodes
-  **\[Calibration Publisher\]** … Start/end a *runtime\_manager calibration\_publisher.launch* script. Specify the full path of a YAML file to be selected at launching nodes by a launched calibration\_publiher dialog.
-  **\[Points Image\]** … Start/end a *runtime\_manager/points2image.launch* script.
-  **\[Virtual Scan Image\]** … Start/end a *runtime\_manager/vscan.launch* script.
-  **\[Scan Image\]** … Start/end *scan2image/scan2image* nodes
-  **\[ROSBAG\]** … Display a ROSBAG Record dialog.
-  **\[RViz\]** … Start/end RViz.
-  **\[RQT\]** … Start/end RQT.

## Runtime Manager – Computing Tab

![Figure 22 - Runtime Manager - Computing Tab](/en/imgs/fig22.png)

### \[Localication\]

#### \[gnss\_lozalizer\]

-   **\[fix2tfpose\] check box** … Start/end *gnss\_localizer/fix2tfpose* nodes
-   **\[nmea2tfpose\] check box** … Start/end a *gnss\_localizer/nmea2tfpose.launch* script.

#### \[ndt\_lozalizer\]

-   **\[ndt\_mapping\] check box** … Start/end a *ndt\_localizer/ndt\_mapping.launch* script. Clicking \[app\], *ndt\_mapping* dialog is displayed.
-   **\[ndt\_matching\] check box** … Start/end a *ndt\_localizer/ndt\_matching.launch* script. Clicking \[app\], *ndt* dialog is displayed.

#### \[vel\_pose\_mux\]

-   **\[vel\_pose\_mux\] check box** … Start/end a *vel\_pose\_mux/vel\_pose\_mux.launch* script. Clicking \[app\], a *vel\_pose\_mux* dialog is displayed.

### \[Detection\]

#### \[cv\_detector\]

-   **\[dpm\_ocv\] check box** … Start/end a *cv\_tracker/dpm\_ocv.launch* script. A *dpm\_ocv* dialog is displayed. After specifying parameters, launch the script by pressing \[Detection Start\]. Clicking \[app\], a dialog is displayed. Selecting a turning parameter class (Car or Pedestrian), either a *car\_dpm* or a *pedestrian\_dpm* dialog is displayed. Setting parameters, a topic of either */config/car\_dpm* or */config/pedestrian\_dpm* is published.
-   **\[dpm\_ttic\] check box** … Start/end a *cv\_tracker/dpm\_ttic.launch* script. A *dpm\_ttic* is displayed. After specifying parameters, launch the script by pressing \[Detection Start\]. Clicking \[app\], a dialog is displayed.
-   **\[rcnn\_node\] check box** … Start/end a *cv\_tracker/rcnn.launch* script. Clicking \[app\], a dialog is displayed.
-   **\[range\_fusion\] check box** … Start/end a cv\_tracker/ranging.launch script. A car\_fusion dialog is displayed at launching the script. Clicking \[app\], a dialog is displayed.
-   **\[klt\_track\] check box** … Start/end a *cv\_tracker/klt\_tracking.launch* script. Clicking \[app\], a dialog is displayed.
-   **\[kf\_track\] check box** … Start/end a *cv\_tracker/kf\_tracking.launch* script. A dialog is displayed at launching the script. Clicking \[app\], a dialog is displayed.
-   **\[obj\_reproj\] check box** … Start/end a *cv\_tracker/reprojection.launch* script. An *obj\_reproj* dialog is displayed. Clicking \[app\], a dialog is displayed.

#### \[lidar\_detector\]

-  **\[euclidean\_cluster\] check box** … Start/end a *lidar\_tracker/euclidean\_clustering.launch* scritp. Clicking \[app\], a dialog is displayed.
-  **\[obj\_fusion\] check box** … Start/end a *lidar\_tracker/obj\_fusion.launch* script. An obj\_fusion dialog is displayed at launching the script. Clicking \[app\], a dialog is displayed.

#### \[road\_wizard\]

-  **\[feat\_proj\] check box** … Start/end a *road\_wizard/feat\_proj.launch* script. Clicking \[app\], a dialog is displayed.
-  **\[region\_tlr\] check box** … Start/end a *road\_wizard/traffic\_light\_recognition.launch* script. Clicking \[app\], a dialog is displayed.

#### \[viewers\]

-   **\[image\_viewer\] check box** … Start/end a *viewers/viewers.launch* script. *image\_viewer* is specified as a script parameter *viewer\_type*. Clicking \[app\], a dialog is displayed.
-   **\[image\_d\_viewer\] check box** …Start/end a *viewers/viewers.launch* script. *image\_viewer* is specified as a script parameter *viewer\_type*. Clicking \[app\], a dialog is displayed.
-   **\[points\_image\_viewer\] check box** …Start/end a *viewers/viewers.launch* script. *points\_image\_viewer* is specified as a script parameter *viewer\_type*. Clicking \[app\], a dialog is displayed.
-  **\[points\_image\_d\_viewer\] check box** …Start/end a *viewers/viewers.launch* script. *points\_image\_d\_viewer* is specified as a script parameter *viewer\_type*. Clicking \[app\], a dialog is displayed.
-  **\[vscan\_image\_viewer\] check box** …Start/end a *viewers/viewers.launch* script. *vscan\_image\_viewer* is specified as a script parameter *viewer\_type*. Clicking \[app\], a dialog is displayed.
-  **\[vscan\_image\_d\_viewer\] check box** …Start/end a *viewers/viewers.launch* script. *vscan\_image\_d\_viewer* is specified as a script parameter *viewer\_type*. Clicking \[app\], a dialog is displayed.
-  **\[traffic\_light\_viewer\] check box** …Start/end a *viewers/viewers.launch* script. *traffic\_light\_viewer* is specified as a script parameter *viewer\_type*. Clicking \[app\], a dialog is displayed.

### \[Semantics\]

-  **\[laserscan2costmap\] check box** … Start/end an *object\_map/laserscan2costmap.launch* script. Clicking \[app\], a dialog is displayed.
-  **\[points2costmap\] check box** … Start/end an *object\_map/points2costmap.launch* script. Clicking \[app\], a dialog is displayed.

### \[Mission Planning\]

#### \[lane\_planner\]

-  **\[lane\_navi\] check box** … Start/end a *lane\_planner/lane\_navi.launch* script. Clicking \[app\], a dialog is displayed.
-  **\[lane\_rule\] check box** … Start/end a *lane\_planner/lane\_rule* script. Clicking \[app\], a dialog is displayed.
-  **\[lane\_stop\] check box** … Start/end a *lane\_planner/lane\_stop* script. Clicking \[app\], a dialog is displayed.
-  **\[lane\_select\] check box** … Start/end a *lane\_planner/lane\_script*. Clicking \[app\], a dialog is displayed.

#### \[freespace\_planner\]

-  **\[astar\_navi\] check box** … Start/end a *freespace\_planner/astar\_navi* script. Clicking \[app\], a dialog is displayed.

### \[Motion Planning\]

#### \[driving\_planner\]

-  **\[velocity\_set\] check box** … Start/end a *driving\_planner/velocity\_set.launch* script. Clicking \[app\], a dialog is displayed.
-  **\[path\_select\] check box** … Start/end a *driving\_planner/path\_select* script. Clicking \[app\], a dialog is displayed.
-  **\[lattice\_trajectory\_gen\] check box** … Start/end a *driving\_planner/lattice\_trajectory\_gen.launch* script. Clicking \[app\], a dialog is displayed.
-  **\[lattice\_twist\_convert\] check box** … Start/end a *driving\_planner/lattice\_twist\_convert* nodes. Clicking \[app\], a dialog is displayed.

#### \[waypoint\_marker\]

-  **\[waypoint\_loader\] check box** … Start/end a *waypoint\_maker/waypoint\_loader.launch* script. Clicking \[app\], a dialog is displayed.
-  **\[waypoint\_saver\] check box** … Start/end a *waypoint\_maker/waypoint\_saver.launch* script. Clicking \[app\], a dialog is displayed.
-  **\[waypoint\_clicker\] check box** … Start/end a *waypoint\_maker/waypoint\_clicker* nodes. Clicking \[app\], a dialog is displayed.

#### \[waypoint\_follower\]

-  **\[pure\_pursuit\] check box** … Start/end a *waypoint\_follower/pure\_pursuit.launch* script. Clicking \[app\], a dialog is displayed.
-  **\[twist\_filter\] check box** … Start/end a *waypoint\_follower/twist\_filter.launch* script. Clicking \[app\], a dialog is displayed.
-  **\[wf\_simulator\] check box** … Start/end a *waypoint\_follower/wf\_simulator.launch* script. Clicking \[app\], a dialog is displayed.
-  **\[ROSBAG\]** … Dislpay ROSBAG Record Dialog.
-  **\[RViz\]** … Start/end RViz.
-  **\[RQT\]** … Start/end RQT.
  

## Runtime Manager – Interface Tab

![Figure 23 - Runtime Manager - Interface Tab](/en/imgs/fig23.png)

-  **\[Android Tablet\]** … Start/end *runtime\_manager/tablet\_socket.launch* for communicating with Tablets.
-  **\[Oculus Rift\]** … &lt;Unimplemented&gt;
-  **\[Vehicle Gateway\]** … Start/end *runtime\_manager/vehicle\_socket.launch* for communicating with ZMP Robocar.
-  **\[Sound\] check box** … Start/end a *sound\_player/sound\_player.py*.
-  **\[Auto Pilot\]** … Publish a topic (*/mode\_cmd*) according the button status.
-  **\[Lamp\]** … Publish a topic (*/lamp\_cmd*) according to the button status.
-  **\[Indicator\]** … Publish a topic (*/indicator\_cmd*) according to the button status.
-  **\[D\] toggle switch** … Publish a topic (*/gear\_cmd*) of buttons turning ON.
-  **\[R\] toggle switch** … Publish a topic (*/gear\_cmd)* of buttons turning ON.
-  **\[B\] toggle switch** … Publish a topic (*/gear\_cmd*) of buttons turning ON.
-  **\[N\] toggle switch** … Publish a topic (*/gear\_cmd*) of buttons turning ON.
-  **\[Accel\] slider** … Publish a topic (*/accel\_cmd*) of the slider status.
-  **\[Brake\] slider** … Publish a topic (*/brake\_cmd*) of the slider status.
-  **\[Steer\] slider** … Publish a topic (*/steer\_cmd*) of the slider status.
-  **\[Torque\] slider** …&lt;Unimplemented&gt;
-  **\[Veloc\] slider** … Publish a topic (*/twist\_cmd*) of the slider status.
-  **\[Angle\] slider** … Publish a topic (*/twist\_cmd*) of the slider status.
-  **\[ROSBAG\]** … Display a ROSBAG Record dialog.
-  **\[RViz\]** … Start/end RViz.
-  **\[RQT\]** … Start/end RQT.

## Runtime Manager – Database Tab

![Figure 24 - Runtime Manager - Database Tab](/en/imgs/fig24.png)

### \[CAN\]

-  **\[can\_uploder\] check box** … Start/end *obj\_db/can\_uploader* nodes. Clicking \[app\], an *other* dialog is displayed.

### \[Map\]

-  **\[map\_downloader\] check box** … Start/end the *map\_file/map\_downloader.launch* scripts. Clicking \[app\], a map\_downloader dialog is displayed.

### \[Position\]

-  **\[pos\_downloader\] check box** … Start/end *pos\_db/pos\_downloader* nodes. Clicking \[app\], a *pos\_db* dialog is displayed.
-  **\[pos\_uploder\] check box** … Start/end *pos\_db/pos\_uploader* nodes. Clicking \[app\]*,* a *pos\_db* dialog is displayed.

### \[Sensors\]

-  **\[image\_upload\] check box** … ＜Unimplemented＞
-  **\[pointcloud\_uplod\] check box** …＜Unimplemented＞
-  **\[Query\]** … ＜Unimplemented＞
-  **\[ROSBAG\]**… Display a ROSBAG Record dialog to recode a ROSBAG.
-  **\[RViz\]** … Start/end RViz.
-  **\[RQT\]** … Start/end RQT.

## Runtime Manager – Simulation Tab

![Figure 25 - Runtime Manager - Simulation Tab](/en/imgs/fig25.png)

\* A capture and the description to be updated (tmp), /use\_sim\_time

-  **\[ROSBAG\] text box** … Start a ROSBAG with an argument specified full path of a bag file in the text box by pressing \[Play\]. Alternatively, a bag file can be specified by the file selection dialog displayed by pressing \[Ref\].
-  **\[Rate\]**… Specify a numerical value for the ROSBAG play command with –r option. If it is not specified, -r option is not used.
-  **\[State Time(s)\]** … Specify a numerical value for the ROSBAG play command with –start option. If it is not specified, -start option is not used.
-  **\[Repeat\] check box** … If this is ON, --loop option is specified at launching the ROSBAG play command.
-  **\[Play\]** … Start the ROSBAG play command with a bag specified in the ROSBAG text box.
-  **\[Stop\]** … End the running ROSBAG play command.
-  **\[Pause\]** … Suspend the running ROSBAG play command.
-  **\[ROSBAG\]** … Display a ROSBAG Record dialog to record a ROSBAG.
-  **\[RViz\]** … Start/end RViz.
-  **\[RQT\]** … Start/end RQT.
 
## Runtime Manager – Status Tab

![Figure 26 - Runtime Manager - Status Tab](/en/imgs/fig26.png)

-  Top window … Display the execution results of the running *top* command.
-  Left-bottom window … Display the periodic execution time published by related node.
-  Right-bottom window … Display launched nodes, standard outputs and standard error outputs of scripts. Some nodes displaying progress bars can not show information.
-  Bottom window … Display CPU (core) load and memory usage.
-  **\[System Monitor\]** … Start/end the gnome-system-monitor command.
-  **\[ROSBAG\]** … Display a ROSBAG Record dialog to record a ROSBAG.
-  **\[RViz\]** … Start/end RViz.
-  **\[RQT\]** … Start/end RQT.
  

## Runtime Manager - Topics Tab

![Figure 27 - Runtime Manager - Topics Tab](/en/imgs/fig27.png)

-  Left window … Display the list of topics. Launch a rostopic info &lt;target topic&gt; command and display the result of the command on the right bottom window by clicking links. If the Echo check box is ON, rostopic echo &lt;target topic&gt; is launched and the results are displayed on the right-top window.
-  **\[Refresh\]** … Update the list of topics displayed on the left window using the obtained topic list by running *rostopic list* command.
-  **\[ROSBAG\]** … Display a ROSBAG Record dialog to record a ROSBAG.
-  **\[RViz\]** … Start/end RViz.
-  **\[RQT\]** … Start/end RQT. 

## ROSBAG Record Dialog

![Figure 28 - ROSBAG Record Dialog](/en/imgs/fig28.png)

-  **Text box** … Specify the full path of a bag file for the ROSBAG record command. Alternatively, a bag file can be selected by the file selection dialog displayed by pressing \[Ref\].
-  **\[split\] check box** … If the check box is ON and a numerical values is set in the size text box, --split and –size=&lt;target size&gt; options for launching ROSBAG record command by pressing \[Start\] are specified.
-  **\[size\] text box** … If *split* check box is ON, specify split file size in megabytes.
-  **\[Start\]** … Start the ROSBAG record command with a bag file specified in the top text box.
-  **\[Stop\]** … End the running ROSBAG record command.
-  **\[All\]** check box… If check box is ON, -a option is specified for launching the ROSBAG record command.
-  **(Other check boxes)** … Specify topics whom check boxes are ON for launching the ROSBAG record command. Only if all check box is OFF, they are enable.
-  **\[Refresh\]** … Starting the rostopic list command and searching running topics, update the other check boxes.

## RViz

![Figure 29 - RVIZ](/en/imgs/fig29.png)

\* the description to be updated (tmp)

## AutowareRider

This is an android application which has a UI similar to Knight Rider.

The running window is as follows:

![Figure 30 - AutowareRider](/en/imgs/fig30.png)

-  **\[Navi\]** … Start AutowareRoute.apk.
-  **\[MAP\]** … &lt;Unimplemented&gt;
-  **\[S1\]** … Start check.launch by ROS PC.
-  **\[S2\]** … Start set.launch by ROS PC.
-  **\[B\]** … Send gear B information to ROS PC.
-  **\[N\]** …Send gear N information to ROS PC.
-  **\[D\]** …Send gear D information to ROS PC.
-  **\[R\]** …Send gear R information to ROS PC.
-  **\[AUTO CRUISE\]** … &lt;Unimplemented&gt;
-  **\[NORMAL CRUISE\]** … &lt;Unimplemented&gt;
-  **\[PURSUIT\]** … &lt;Unimplemented&gt;(End the application)

The following items can be selected on the right-top menu.

**\[設定\] (configuration)**

**\[データ収集\] (data collection)**

![Figure 31 AutowareRider Configuration Window](/en/imgs/fig31.png)

### \[ROS PC\]

-  **IPアドレス** … ROS PC IPv4 address
-  **命令受信ポート番号** … tablet\_receiver port number (default: 5666)
-  **情報送信ポート番号** … tablet\_sender port number (default: 5777)

### \[データ収集\] (data collection)

-  **テーブル名** … Data transfer destination table name

### \[SSH\]

-  **ホスト名** …SSH destination hostname
-  **ポート番号** …SSH destination port number (default: 22)
-  **ユーザ名** … SSH login username
-  **パスワード** … SSH login password

### \[ポートフォワーディング\] (port forwarding)

-  **ローカルポート番号** … port number of local machine source (default: 5558)
-  **リモートホスト名** … remote machine host name (default: 127.0.0.1)
-  **リモートポート番号** … remote machine destination port number (default: 5555)

\[データ収集\] (data collection) window is as follows:

![Figure 32 - AutowareRider Data Collection Window](/en/imgs/fig32.png)

-  **\[CanGather\]** … Start CanGather.apk.
-  **\[CarLink (Bluetooth)\]** … Start CarLink\_CAN-BT\_LS.apk.
-  **\[CarLink (USB)\]** … Start CarLink\_CANusbAccessory\_LS.apk.

## AutowareRoute

AutowareRoute is an Android application implemented by MapFan SDK for path planning.

The start window is as follows:

![Figure 33 - Autoware Route](/en/imgs/fig33.png)

Pressing and holding the map, the following dialog is displayed.

![Figure 34 - AutowareRoute Window](/en/imgs/fig34.png)

Figure AutowareRoute window

-  **\[出発地に設定\]** … Specify the departure point by pressing and holding a point.
-  **\[立寄地に設定\]** … Specify the way-stop point by pressing and holding a point.
-  **\[目的地に設定\]** … Specify the destination point by pressing and holding a point.
-  **\[ルート消去\]** … Delete path data generated by path planning.
-  **\[ルート探索実行\]** … Generate path data considering points of departure, way-stop and destination.
-  **\[終了\]** … (no description (tmp))

# Chapter 5 - System Setup

The supported OS distributions and the required software are described in this chapter.

## Installation

Install OS (Linux), ROS, Autoware, etc., into PC as the following procedural steps.

### OS

The Linux distributions supported by Autoware at Sep. 2016 are as follows:

Ubuntu 14.04 LTS x86\_64 (Recommended),  
Ubuntu 15.04 x86\_64

Refer to the below URLs about install media and installation steps.

Ubuntu Japanese Team [*https://www.ubuntulinux.jp/*](https://www.ubuntulinux.jp/)

Ubuntu [*http://www.ubuntu.com/*](http://www.ubuntu.com/)

### ROS

If you use Ubuntu 14.04, install ROS and the required packages by the following steps:

```shell
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu trusty main" > \
/etc/apt/sources.list.d/ros-latest.list'
$ wget http://packages.ros.org/ros.key -O - | sudo apt-key add -
$ sudo apt-get update
$ sudo apt-get install ros-indigo-desktop-full ros-indigo-nmea-msgs \
ros-indigo-nmea-navsat-driver ros-indigo-sound-play ros-indigo-jdk-visualization
$ sudo apt-get install libnlopt-dev freeglut3-dev qtbase5-dev libqt5opengl5-dev \
libssh2-1-dev libarmadillo-dev libpcap-dev gksu
```

Add below path to \~/.bashrc etc.

```shell
[ -f /opt/ros/indigo/setup.bash ] && . /opt/ros/indigo/setup.bash
```

If you use Ubuntu 15.04, install ROS and the required packages as the following steps:

```shell
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > \
/etc/apt/sources.list.d/ros-latest.list'
$ sudo apt-key adv --keyserver hkp://pool.sks-keyservers.net:80 \
--recv-key 0xB01FA116
$ sudo apt-get install ros-jade-desktop-full ros-jade-nmea-msgs \
ros-jade-nmea-navsat-driver ros-jade-sound-play
$ sudo apt-get install libnlopt-dev freeglut3-dev qt5-default libqt5opengl5-dev \
libssh2-1-dev libarmadillo-dev libpcap-dev gksu
```
Add below path to \~/.bashrc etc.

```shell
[ -f /opt/ros/jade/setup.bash ] && . /opt/ros/jade/setup.bash
```

### OpenCV

1.  Install the following packages:
```shell
$ sudo apt-get -y install libopencv-dev build-essential cmake git \
libgtk2.0-dev pkg-config python-dev python-numpy libdc1394-22 \
libdc1394-22-dev libjpeg-dev libpng12-dev libtiff4-dev libjasper-dev \
libavcodec-dev libavformat-dev libswscale-dev libxine-dev \
libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev libv4l-dev \
libtbb-dev libqt4-dev libfaac-dev libmp3lame-dev libopencore-amrnb-dev \
libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev \
x264 v4l-utils unzip
```
2.  Get OpenCV source code of the version 2.4.11 or later, install it as follows:
```shell
$ wget https://github.com/Itseez/opencv/archive/2.4.11.zip
$ unzip 2.4.11.zip
$ cd opencv-2.4.11/
$ mkdir build && cd build/
$ cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/local \
-D WITH_TBB=ON -D BUILD_NEW_PYTHON_SUPPORT=ON \
-D WITH_V4L=ON -D INSTALL_C_EXAMPLES=ON \
-D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=ON \
-D WITH_OPENGL=ON -D WITH_VTK=ON -D CUDA_GENERATION=Auto ..
$ make -j4
unzip opencv-2.4.11.zip
$ cd opencv-2.4.11
$ cmake .
$ make
$ sudo make install
$ sudo ldconfig
```
### CUDA(if necessary)

If you use GPU on NVIDIA graphic board for conducting calculations, CUDA is required. Refer the website [*http://docs.nvidia.com/cuda/cuda-getting-started-guide-for-linux/*](http://docs.nvidia.com/cuda/cuda-getting-started-guide-for-linux/) and install as follows:

1.  Check environment

```shell
$ lspci | grep -i nvidia
```
(ensure the NVIDIA board information is output)

```shell
$ uname -m
```
(ensure x86\_64 is used)

```shell
$ gcc --version
```
(ensure gcc installation is completed)

Install CUDA

Download CUDA from *<http://developer.nvidia.com/cuda-downloads>.* (hereinafter, installing cuda-repo-ubuntu1404\_7.0-28\_amd64.deb is assumed)
```shell
$ sudo dpkg -i cuda-repo-ubuntu1404_7.0-28_amd64.deb
$ sudo apt-get update
$ sudo apt-get install cuda
```
Restart OS

```shell
$ lsmod | grep nouveau
```
(ensure nouveau driver is not loaded)

Check CUDA

```shell
$ cat /proc/driver/nvidia/version
```
(kernel module and gcc version are displayed)
```shell
$ cuda-install-samples-7.0.sh \~
$ cd ~/NVIDIA_CUDA-7.0_Samples/1_Utilities/deviceQuery/
$ make
$ ./deviceQuery
```
If necessary, add below path to .bashrc etc.
```shell
export PATH=”/usr/local/cuda:\$PATH”
export LD_LIBRARY_PATH=”/usr/local/cuda/lib:\$LD_LIBRARY_PATH”
```
### FlyCapture (if necessary)

If you use PointGray cameras, install FlyCapture SDK as follows:

1.  Download FlyCapture SDK from PointGrey website ([*http://www.ptgrey.com/*](http://www.ptgrey.com/)). User registration is required before download.

2.  Install packages

```shell
$ sudo apt-get install libglademm-2.4-1c2a libgtkglextmm-x11-1.2-dev libserial-dev
```

3.  Extract the archives of the downloaded file

```shell
$ tar xvfz flycapture2-2.6.3.4-amd64-pkg.tgz
```

4.  Launch installer
```shell
$ cd flycapture2-2.6.3.4-amd64/
$ sudo sh install_flycapture.sh
```
Enter y after the below texts are displayed.
```shell
This is a script to assist with installation of the FlyCapture2 SDK.
Would you like to continue and install all the FlyCapture2 SDK packages?
(y/n)$ y
```
Enter y after the below texts are displayed.
```shell
...
Preparing to unpack updatorgui-2.6.3.4_amd64.deb ...
Unpacking updatorgui (2.6.3.4) ...
updatorgui (2.6.3.4) を設定しています ...
Processing triggers for man-db (2.6.7.1-1ubuntu1) ...
Would you like to add a udev entry to allow access to IEEE-1394 and USB hardware?
If this is not ran then your cameras may be only accessible by running flycap as sudo.
(y/n)$ y
```

### Autoware

Get Autoware by following steps. Then build and install it.

If you get the latest autoware form github
```shell
$ git clone https://github.com/CPFL/Autoware.git
$ cd Autoware/ros/src
$ catkin_init_workspace
$ cd ../
$ ./catkin_make_release
$ source devel/setup.bash
```

If you use archives:
```shell
$ wget http://www.pdsl.jp/app/download/10394444574/Autoware-beta.zip
$ unzip Autoware-beta.zip
$ cd Autoware-beta/ros/src
$ catkin_init_workspace
$ cd ../
$ ./catkin_make_release
$ source devel/setup.bash
```
### AutowareRider (if necessary)

AutowareRider is an Android application, which has similar UI as Knight Rider, for operating Autoware running on ROS PC from tablet devices.  Get APK from below URLs and install them.

Main

-   AutowareRider.apk [*https://github.com/CPFL/Autoware/blob/master/ui/tablet/AutowareRider/AutowareRider.apk*](https://github.com/CPFL/Autoware/blob/master/ui/tablet/AutowareRider/AutowareRider.apk).  

Path data planning apllication
-   AutowareRoute.apk [*https://github.com/CPFL/Autoware/blob/master/ui/tablet/AutowareRoute/AutowareRoute.apk*](https://github.com/CPFL/Autoware/blob/master/ui/tablet/AutowareRoute/AutowareRoute.apk).  

CAN data collection application
-   CanDataSender.apk [*https://github.com/CPFL/Autoware/blob/master/vehicle/android/CanDataSender/bin/CanDataSender.apk*](https://github.com/CPFL/Autoware/blob/master/vehicle/android/CanDataSender/bin/CanDataSender.apk)
   

### canlib (if necessary)

Get linuxcan.tar.gz from *Kvaser LINUX Driver and SDK* in the kvaser website ([*http://www.kvaser.com/downloads/*](http://www.kvaser.com/downloads/)) and install it as the following steps:
```shell
$ tar xzf linuxcan.tar.gz
$ cd linuxcan
$ make
$ sudo make install
```
### SSH Public Key Generation (if necessary)

When pos\_db access database via SSH, it uses SSH key without pass phrase.  Therefore, generating the SSH key of the database server and registering it to the server are required.

1.  Generating SSH Key

Run the following commands.
```shell
$ ssh-keygen -t rsa
```
(Press Enter key without strings.)
If you use DSA, specify -t dsa.

Registering the SSH key to the database server.  Copy the generated SSH key to the server as follows:

```shell
$ ssh-copy-id -i ~/.ssh/id_rsa.pub posup@db3.ertl.jp
```
(”posup” indicates user name and ”db3.ertl.jp” denotes database server name.)
Enter passwords if necessary.

# Chapter 6 - Terminology

**Term** | **Description**
-------- | ---------------
3-D map | *3-Dimension Map*:  This type maps is different from 2-D maps that are used in car navigation systems, and include various information, such as three-dimensional objects built around the roads, while 2-D map is used for the car navigation systems. PCD (point cloud data) type of 3-D map is used in Autoware.
Autoware | \[Autoware\] This is an open source software running on ROS for autonomous driving.
AutowareRider | \[Autoware\] This is an Android application for operating Autoware running on ROS PC by tablet terminal. This application has similar UI (user interface) as Knight Rider, which is a TV series.
AutowareRoute | \[Autoware\] This is an Android application implemented with MapFan SDK for planning path data.
CAN | *Controller Area Network*:  This is a standard for data transfer between interconnected devices. This standard has been proposed by BOSCH Ltd. in Germany as an inter-vehicle network, and standardized as ISO11898 and ISO 11519. Currently, CAN is the inter-vehicle network standard.
Catkin | \[ROS\] The original building system
CUDA | *Compute Unified Device Architecture*:  A parallel computing platform and programming model with GPU provided by NVIDIA corporation.
DMI | *Distance Measuring Instrument*
DPM | *Deformable Part Model*: an object detection method
FlyCapture SDK | A SDK to Control Point Grey Camera
FOT | *Field Operational Tests*:  A statistical verification by observing traffic environment, driver’s operations, and vehicle behavior under real driving environment
GNSS | *Global Navigation Satellite System*
IMU | *Inertial Measurement Unit*: Devices measuring the angular velocity and acceleration
KF | *Kalman Filter*: This is an estimation method for future states of the target objects using observed values.
LIDAR | *Laser Imaging Detection and Ranging/Laser Scanner/Laser Rader*:  This device emits laser pulse and measures the scattered light of them. The distance between the LIDAR and the objects can be calculated by the measured results.
Message | \[ROS\] Data structure used in communication among nodes
NDT | *Normal Distributions Transform*: A localization method
Node | \[ROS\] A process to operate single function
Odometry | A position estimation method using calculated the rotation angle velocity of the wheel.
OpenCV | *Open source Computer Vision library*
Point Cloud | A data set of points in 3-D space.  These points are represented in Cartesian coordinate space (x, y, z).
Publish | \[ROS\] Sending a Message (called broadcasting or publication)
Qt | Application user interface framework
ROS | ROS is a software framework for robot software development. This framework provides hardware abstraction, low-level device control, well-used functions, inter-process communication, package management tool and etc.
ROS PC | Computers installed ROS and Autoware
ROSBAG | \[ROS\] A data logging tool.  The extension is *bag*.
RQT | \[ROS\] A software development tool base on Qt
RViz | \[ROS\] Visualization tool for data and software status
SLAM | Simultaneous Localization and Mapping
Subscribe | \[ROS\] Receiving Message (called subscription)
TF | \[ROS\] A coordinate transformation library
Topic | \[ROS\] A path with a name for sending/receiving Message.  Sending Message is called *Publish* and receiving Message is called *Subscribe*.
Way point | A set of coordinate pairs
Calibration | A processing to calculate camera parameters for adjusting the points projected on camera images to the positions in 3-D space
Sensor fusion | A method to achieve high recognition function by integrating multiple sensor information for calculating more accurate position, posture, etc.
Vector map | GIS (Global Information System) data representing geometric information of the roads in vector
Message passing | Inter-process communication that a sender transmits data to one or more receivers

# Chapter 7 - Related Documents

### Autoware

<http://www.pdsl.jp/fot/autoware/>

### AutowareRider

-   Main:  AutowareRider.apk [*https://github.com/CPFL/Autoware/blob/master/ui/tablet/AutowareRider/AutowareRider.apk*](https://github.com/CPFL/Autoware/blob/master/ui/tablet/AutowareRider/AutowareRider.apk)
-   Path data planning application:  AutowareRoute.apk [*https://github.com/CPFL/Autoware/blob/master/ui/tablet/AutowareRoute/AutowareRoute.apk*](https://github.com/CPFL/Autoware/blob/master/ui/tablet/AutowareRoute/AutowareRoute.apk)
-   CAN data collection appllication:  CanDataSender.apk  [*https://github.com/CPFL/Autoware/blob/master/vehicle/android/CanDataSender/bin/CanDataSender.apk*](https://github.com/CPFL/Autoware/blob/master/vehicle/android/CanDataSender/bin/CanDataSender.apk)

### CUDA

[*http://www.nvidia.com/object/cuda\_home\_new.html*](http://www.nvidia.com/object/cuda_home_new.html)

[*http://www.nvidia.co.jp/object/cuda-jp.html*](http://www.nvidia.co.jp/object/cuda-jp.html)

[*http://docs.nvidia.com/cuda/cuda-getting-started-guide-for-linux/*](http://docs.nvidia.com/cuda/cuda-getting-started-guide-for-linux/)

[*http://developer.nvidia.com/cuda-downloads*](http://developer.nvidia.com/cuda-downloads)

### FlyCapture SDK

[*http://www.ptgrey.com/flycapture-sdk*](http://www.ptgrey.com/flycapture-sdk)

### OpenCV

[*http://opencv.org/*](http://opencv.org/)

[*http://opencv.jp/*](http://opencv.jp/)

*http://sourceforge.net/projects/opencvlibrary/*

### Qt

[*http://www.qt.io/*](http://www.qt.io/)

[*http://qt-users.jp/*](http://qt-users.jp/)

### ROS

[*http://www.ros.org/*](http://www.ros.org/)

### Ubuntu Japanese Team

[*https://www.ubuntulinux.jp/*](https://www.ubuntulinux.jp/)

### Ubuntu

[*http://www.ubuntu.com/*](http://www.ubuntu.com/)

### Velodyne Drivers

[*https://github.com/ros-drivers/velodyne*](https://github.com/ros-drivers/velodyne)

### Demo Files

-   Script for generating demo launch file:  [*http://db3.ertl.jp/autoware/sample\_data/my\_launch.sh*](http://db3.ertl.jp/autoware/sample_data/my_launch.sh)
-   Data for demo (Moriyama map, Calibration, Path data):  [*http://db3.ertl.jp/autoware/sample\_data/sample\_moriyama\_data.tar.gz*](http://db3.ertl.jp/autoware/sample_data/sample_moriyama_data.tar.gz)
-   ROSBAG data:  [*http://db3.ertl.jp/autoware/sample\_data/sample\_moriyama\_150324.tar.gz*](http://db3.ertl.jp/autoware/sample_data/sample_moriyama_150324.tar.gz)  Note: this ROSBAG data doesn’t include image information for object
detection.
