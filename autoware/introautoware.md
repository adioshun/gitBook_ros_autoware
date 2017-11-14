# Autoware

## 1. Autoware 

### 1.1 개요
- Autoware is an open source software based on ROS. 

- It is developed by Nagoya University and is intended for autonomous driving research and development. 

- It is open sourced on GitHub


- Autoware provides the following functions.
    - Localization
    - Object detection
    - Driving control
    - 3D map generation and sharing


![](https://i.imgur.com/ptfCJXZ.png)

### 1.2 bag file (rosbag)
    - In ROS, all the messages of topics are logged and time-stamped to a .bag file called “rosbag”. 
    - This can be used for replay of the messages with RViz (3D visualization tool for ROS). 
    - In robotics, it is often difficult to analyze the interactions with multiple sensors at once, and this tool enables efficient analysis and debugging of system. 
    - Also, the logged messages can be replayed repeatedly, allowing developers to debug their systems without actual sensors.
    
### 1.3 User Interface
    
![](https://i.imgur.com/smNBB8h.png)


    
    
## 2. The operations of main functionalities 

### 2.1 replay of demo data
- Replay the rosbag demo data downloaded from Nagoya University.
    1. Setup of demo data
    2. Launch Autoware
    3. Prepare to play rosbag
    4. Localization
    5. Mission planning
    6. Motion following
    

#### A. Setup of demo data


- 데이터셋 준비 
```
cd ~
mkdir .autoware
cd ~/.autoware

# A script for generating a demo launch file:
wget http://db3.ertl.jp/autoware/sample_data/my_launch.sh

#Map/calibration/path data (Moriyama area, 175MB):
wget http://db3.ertl.jp/autoware/sample_data/sample_moriyama_data.tar.gz

#rosbag data (3GB):
wget http://db3.ertl.jp/autoware/sample_data/sample_moriyama_150324.tar.gz
# Note that this ROSBAG data do not contain the video data. Therefore the object detection is not supported

# Unpack the demo data as follows.
tar xfz sample_moriyama_data.tar.gz
tar xfz sample_moriyama_150324.tar.gz
```

- Run my_launch.sh
```
# Run the following script to generate launch files.
sh my_launch.sh

# my_launch/폴더에 아래 파일들 생성 확인
my_map.launch		　　　　　# Load PointClouds and vector maps
my_sensing.launch	　        # Load device drivers
my_localization.launch	　        # Localozation
my_detection.launch	　        # Object detection
my_mission_planning.launch	　# Path planning
my_motion_planning.launch	　# Path following


```

#### B. Launch Autoware

### 2.2 Autoware on a real vehicle
Drive a real vehicle with Autoware autonomously.





### 2.3 rosbag replay
Replay the data corrected on the procedure on left.

