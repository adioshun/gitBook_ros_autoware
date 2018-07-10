# Gazebo

https://dnddnjs.gitbooks.io/drone-autonomous-flight/content/gazebo6_c124_ce58.html


## Velodyne simulation

> [ROS_wiki](http://wiki.ros.org/velodyne_simulator), [code](https://bitbucket.org/DataspeedInc/velodyne_simulator.git)

```python
apt-get install ros-$ROS_DISTRO-velodyne-simulator
```

Source installation (melodic)
```
cd ~/catkin_ws/src
git clone https://bitbucket.org/DataspeedInc/velodyne_simulator.git
cd ~/catkin_ws/ && catkin_make
```

실행

```
roslaunch velodyne_description example.launch 

```