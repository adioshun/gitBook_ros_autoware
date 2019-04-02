# catkin_make vs. catkin build

> The main difference is the isolated environment that you get with catkin build.

## 1. [catkin_tools](https://catkin-tools.readthedocs.io/en/latest/index.html)

### 1.1 설치 

```bash
sudo apt-get install python-catkin-tools
sudo pip install -U catkin_tools
```

### 1.2 명령어 

catkin init  : 초기화 in `~/catkin_workspace/`
catkin create pkg pkg_a    : 패키지 추가 in `~/catkin_workspace/src`
catkin build   : 빌드 `~/catkin_ws/devel/lib/`
catkin list :


## 2. [catkin make](http://wiki.ros.org/ko/ROS/Tutorials/BuildingPackages)

catkin_init_workspace : 작업공간 생성 in `~/catkin_ws/src`
catkin_create_pkg : 패키지 자동 생성 
catkin_make : 빌드 
catkin_find : 검색