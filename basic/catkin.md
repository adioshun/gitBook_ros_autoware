# catkin_make vs. catkin build

> The main difference is the isolated environment that you get with catkin build.

## 1. catkin_tools

### 1.1 설치 

```bash
sudo apt-get install python-catkin-tools
sudo pip install -U catkin_tools
```



## 2. [catkin make](http://wiki.ros.org/ko/ROS/Tutorials/BuildingPackages)

catkin_init_workspace : 작업공간 생성 in `~/catkin_ws/src`
catkin_create_pkg : 패키지 자동 생성 
catkin_make : 빌드 
catkin_find : 검색