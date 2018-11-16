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

---

## Human 3D Model Tool

http://www.makehumancommunity.org/

설명 : [[ROS Q&A] How to create and spawn humans in Gazebo7](https://www.youtube.com/watch?v=7kEmT-NE75c&feature=youtu.be)

---

## [EtainClub Tutorial](https://github.com/EtainClub/etainclub/wiki/ROS) [[Youtube]](https://www.youtube.com/watch?v=lR3tCNT-xVI&list=PLaPt_ZHO2DL5H7tEezzSraX1fUMPNqu_A)

### 강좌 1. Gazebo Mobile Robot Simulation (Indigo)
1. [프로젝트 설정하기](https://github.com/EtainClub/etainclub/wikiROS-Gazebo-강좌1) (완료)
2. [Gazebo로 3D World 만들기](https://github.com/EtainClub/etainclub/wikiROS-Gazebo-강좌2) (완료)
3. [Gazebo 로봇 모델 만들기](https://github.com/EtainClub/etainclub/wikiROS-Gazebo-강좌3) (완료)
4. [Gazebo에서 Ground Truth Pose 출력하기](https://github.com/EtainClub/etainclub/wikiROS-Gazebo-강좌4) (완료)
5. [평면도 Floorplan RVIZ에 표시하기](https://github.com/EtainClub/etainclub/wikiROS-Gazebo-강좌5) (완료)
6. [주행 클래스 생성하기](https://github.com/EtainClub/etainclub/wikiROS-Gazebo-강좌6) (완료)
7. [장애물 인식 코드 작성하기](https://github.com/EtainClub/etainclub/wikiROS-Gazebo-강좌7) (완료)
8. [Wall Following 코드 작성하기](https://github.com/EtainClub/etainclub/wikiROS-Gazebo-강좌8) (완료)
9. [Coverage Map 생성하기](https://github.com/EtainClub/etainclub/wikiROS-Gazebo-강좌9) (완료)
10. [Coverage Ratio 계산하기](https://github.com/EtainClub/etainclub/wikiROS-Gazebo-강좌10) (완료)

## [Tutorial] Building a Simulated Model for Gazebo and ROS from Scratch (part 1): [Blog](http://moorerobots.com/blog/post/1), [Youtube](https://www.youtube.com/watch?v=8ckSl4MbZLg)

## [Tutorial] Adding Sensors to the Gazebo Model (part 2): [Blog](http://moorerobots.com/blog/post/2), [Youtube](https://www.youtube.com/watch?v=EZ3MYf24c6Y)