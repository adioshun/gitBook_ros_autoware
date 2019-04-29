# catkin_make vs. catkin build

> The main difference is the isolated environment that you get with catkin build.

## 1. [catkin build](https://catkin-tools.readthedocs.io/en/latest/index.html)

### 1.1 설치 

```bash
sudo apt-get install python-catkin-tools
sudo pip install -U catkin_tools
```

### 1.2 명령어 

- catkin init  : 초기화 in `~/catkin_workspace/`
- catkin create pkg pkg_a    : 패키지 추가 in `~/catkin_workspace/src`
    - catkin create pkg my_pcl_tutorial --catkin-deps pcl pcl_ros roscpp sensor_msgs
- catkin build   : 빌드 `~/catkin_ws/devel/lib/`
- catkin list :

#### [example](https://www.systutorials.com/docs/linux/man/1-catkin_tools/)

```python 
$ mkdir -p /tmp/path/to/my_catkin_ws/src      # Make a new workspace and source space
$ cd /tmp/path/to/my_catkin_ws                # Navigate to the workspace root
$ catkin init                                 # Initialize it with a hidden marker file
$ cd /tmp/path/to/my_catkin_ws/src            # Navigate to the source space
$ catkin create pkg pkg_a                     # Populate the source space with packages...
$ catkin create pkg pkg_b
$ catkin create pkg pkg_c --catkin-deps pkg_a
$ catkin create pkg pkg_d --catkin-deps pkg_a pkg_b
$ catkin build                                # Build all packages in the workspace
$ source ../devel/setup.bash                  # Load the workspace's environment
$ catkin clean --all                          # Clean the build products
$ catkin build pkg_d                          # Build `pkg_d` and its deps
$ cd /tmp/path/to/my_catkin_ws/src/pkg_c      # Navigate to `pkg_c`'s source directory
$ catkin build --this                         # Build `pkg_c` and its deps
$ catkin build --this --no-deps               # Rebuild only `pkg_c`
```

---

## 2. [catkin make](http://wiki.ros.org/ko/ROS/Tutorials/BuildingPackages)

catkin_init_workspace : 작업공간 생성 in `~/catkin_ws/src`
catkin_create_pkg : 패키지 자동 생성 
catkin_make : 빌드 
catkin_find : 검색