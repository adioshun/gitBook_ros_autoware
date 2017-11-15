# ROS

## 1. ROS Features

- ROS provides library and tools for robotic software development. 
    - Original build system (Catkin)
    - Image processing library (OpenCV)
    - Data logging tool (ROSBAG)
    - Visualization tools for data and software state (RViz)
    - Coordinate transformation library (TF)
    - Qt based GUI development tool (RQT)
    
- Inter-process communication
    - ROS uses message passing with **topics** of publish/subscribe form for inter-module connection/cooperation frameworks. 
    - In ROS, processes `(called node)` are **launched** and each node is run independently. 
    - In communication between nodes, by following the publish/subscribe scheme, 
        - a node writes messages (publish) into a topic and 
        - another node reads the messages (subscribe) of the topic.

- File components
    - bag file (ROSBAG)
        - In ROS, all the messages of topics are recorded and time-stamped into a **.bag** file `called ROSBAG`. 
        - This file can be used for replaying the messages on **RViz** as same timing as recording. 
    - Launch file
        - A “Launch” file is used to **start multiple nodes at the same time**. 
        - The launch file contains the **nodes **to be started and their parameters written in **XML format**.
        
        
        