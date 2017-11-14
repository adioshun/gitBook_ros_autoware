# ROS AUTONOMOUS VEHICLES 101

- [강의목차](https://www.robotigniteacademy.com/en/course/ros-autonomousvehicles-101/details/)

- [Youtube](https://www.youtube.com/watch?v=jbimBoI42AM)


## 1. Introduction

- `roslaunch catvehicle_tests_cmdvel_unsafetest.launch`

```
dbw_mkz_ros framework: https://bitbucket.org/DataspeedInc/dbw_mkz_ros
CatVehicle TestBed: https://cps-vo.org/group/CATVehicleTestbed
```
## 2. Sensors



#### A. Laser

- `rostopic info /catvehicle/front_laser_points`

- `rosrun rviz rviz` --> [설정](https://youtu.be/jbimBoI42AM?t=5m21s)

#### B. Camera

- `rostopic list | grep camera` --> `image_raw_front`
- rviz 이용 방법
- `rosrun rqt_image_view rqt_image_view` 이용 방법

#### C. GPS

- `rostopic info /fix` : latitud, longitud, altitude
- `rostopic info /fix_velocity` : Speed

- rviz이용 방법, [설정](https://youtu.be/jbimBoI42AM?t=11m19s)
![](https://i.imgur.com/0EqSHRW.png)



## 3. GPS Navigation




## 4. Obstacles and Security

- 안전한 차량을 위해서는 다음 두가지가 있어야함 
    - Obstacle Detection : 본 강의 에서는 Lager를 이용하여 탐지 
    - System Failure Measures : 여러 방법이 있지만, 본 강의에서는 DeadManSwitch만 다룸

- Control the car movement data flow
    - DeadMansSwitch : when controle communication is lost the autonomous vehicles have to move to a safe state. 
    - ObstacleDetection : When an obstacle is detected the vehicle must stop until the obstacle is removed. 
    
    
![](https://i.imgur.com/HbebYRz.png)
    
## 5. CAN-Bus

-rqt_graph

![](https://i.imgur.com/2i0T8OA.png)