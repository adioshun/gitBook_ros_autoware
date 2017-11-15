# Autoware

## 1. Autoware 

### 1.1 개요

![](https://i.imgur.com/ptfCJXZ.png)



- Autoware is an open source software based on ROS. 

- It is developed by Nagoya University and is intended for autonomous driving research and development. 

- It is open sourced on GitHub


- Autoware provides the following functions.
    - Localization
    - Object detection
    - Driving control
    - 3D map generation and sharing





    
### 1.2 Autoware overview

![](https://github.com/CPFL/Autoware-Manuals/raw/master/en/imgs/fig1.png)

- Autoware uses LIDAR and on-vehicle cameras to **localize** the ego-car position. 

- Autoware can **detect** surrounding objects, 
    - such as pedestrians, vehicles, traffic lights etc., 
    - by using LIDAR and GNSS 

- The making judgments of driving/stopping at lanes or intersections are performed with an embedded multi-core CPU. 

- Operations of controlling vehicle behaviors utilize conventional on-vehicle control mechanism, 
    - while support systems such as driving assistance and safety diagnosis support, use multi-core CPU.

#### A. 3-D Map Generation and Sharing
```
The most notable feature of Autoware is the fact that the autonomous driving of Autoware
uses a 3-D map. The 3-D map, unlike 2-D maps used in car navigation system, is a map
including various information, such as roads and all the still objects around the roads. The
3-D map will play a critical role in deploying autonomous driving into the real world.
Autoware localizes the position of the ego-vehicle by matching the 3-D map loaded into the
PC with the surrounding information captured by the LIDAR on the roof. The precision of
localization is approximately 10cm, which number indicates far higher precision than GPS.
As of Sep. 2015, the 3-D map covers the only limited area. If the ego-vehicle with Autoware
enters into the area without the 3-D map, Autoware can generate a 3-D map in real-time
from the information captured by LIDAR.The generated 3-D map in this way not only can be
utilized for the developers using Autoware, but also it can be uploaded and shared on the
Nagoya University servers. The feature enables online updating of the 3-D map. This
mechanism allows for creating the 3-D map including every nook and corner of the city, such
as tiny alleys that the specialized vehicles for 3-D mapping can not enter. In addition, the
geographic data from the 3-D map can generate a 3-D map of vector format.
```
#### B. Localization (NDT:Normal Distributions Transform)
``
The position of the ego-vehicle can be located by scan matching based on the NDT
algorithm with the 3-D map of PCD (Point Cloud Data) format and LIDAR data. The position
error of localization is around 10cm.
```  
#### C. Object Detection

- DPM (Deformable Part Models) algorithms can **detect** cars, pedestrians and traffic lights from camera images. 

- **Tracking** using Kalman filter can be implemented and it improves detection accuracy. 

-** Fusing** 3-D LIDAR data can obtain the distances of the detected objects.

- **Traffic lights detection** is conducted as following steps. 
    1. First, the 3-D positions of traffic lights are calculated by using the localization result and the high-definition 3-D map. 
    2. Next, the 3-D positions of traffic lights are projected on camera images by sensor fusion. 
    3. Then, traffic light colors are recognized by image processing.    


#### D. Path Generation

```
AutowareRoute (a route data generation application using MapFan) generates a path to the
selected destination. Then, autonomous driving system determines the lanes on the path.
The path data generation application is provided as a car navigation of smartphone. The
trajectories in the lanes are calculated kinematically.
```


### 1.3 User Interface
    
![](https://i.imgur.com/smNBB8h.png)


### 1.4 Utilities and Others

- ros/src/util/ : Runtime Manager, sample data, pseudo-drivers
- ui/tablet/ : Smart phone applications
- vehicle/ : Vehicle control, vehicle data acquisition





