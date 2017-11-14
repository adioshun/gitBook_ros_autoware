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


    
    
