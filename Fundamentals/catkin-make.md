## 3. 패키지 설치 방법

### 3.1 소스 설치

```python
# INDIGO VERSION
cd ~/catkin_ws/src/ && git clone https://github.com/ros-drivers/velodyne.git
cd velodyne/
rosdep install --from-paths ./ --ignore-src --rosdistro $ROS_DISTRO -y
cd ~/catkin_ws/ && catkin_make
```

### 3.2 apt설치

```
apt-get install ros-$ROS_DISTRO-velodyne
```


---

## 4. 에러 처리 

###### \[패키지 설치시 에러처리\] Could not find a package configuration file provided by

* apt-get install apt-file && apt-file update

  ```python
  Could not find a package configuration file provided by "Qt5Core"
    (requested version 5.0) with any of the following names:

      Qt5CoreConfig.cmake
      qt5core-config.cmake

    Add the installation prefix of "Qt5Core" to CMAKE_PREFIX_PATH or set
    "Qt5Core_DIR" to a directory containing one of the above files.  If
    "Qt5Core" provides a separate development package or SDK, be sure it has
    been installed.
  ```

* run `apt-file search Qt5CoreConfig.cmake`

  * review the result

  ```python
  qtbase5-dev: /usr/lib/x86_64-linux-gnu/cmake/Qt5Core/Qt5CoreConfig.cmake
  qtbase5-gles-dev: /usr/lib/x86_64-linux-gnu/cmake/Qt5Core/Qt5CoreConfig.cmake
  ```

* Install the missing package `sudo apt install qtbase5-dev`

> ref [What package do I need to build...](https://askubuntu.com/questions/374755/what-package-do-i-need-to-build-a-qt-5-cmake-application/374775)

---




# catkin_make vs. catkin build

> The main difference is the isolated environment that you get with catkin build.

## 1. [catkin build](https://catkin-tools.readthedocs.io/en/latest/index.html)

### 1.1 설치 

```bash
sudo apt-get install python-catkin-tools
sudo pip install -U catkin_tools
```

### 3.1 [catkin build](http://korearosnews.blogspot.com/2015/03/catkin-catkintools.html)

```python
#apt-get install python-catkin-tools

mkdir ~/catkin_workspace
cd ~/catkin_workspace
mkdir src
catkin init
```

### 1.2 명령어 

- catkin init  : 초기화 in `~/catkin_workspace/`
- catkin create pkg pkg_a    : 패키지 추가 in `~/catkin_workspace/src`
    - catkin create pkg my_pcl_tutorial --catkin-deps pcl_ros roscpp sensor_msgs
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


```python
# SOURCING  ENVIRONMENT - for kinetic version
source /opt/ros/$ROS_DISTRO/setup.bash

# CREATING A ROS CATKIN WORKSPACE
mkdir -p ~/catkin_ws/src && cd ~/catkin_ws/
catkin_make
cd ./build
make -j4 -l4


### 3.3 후처리 

# SOURCING  CATKIN ENVIRONMENT - and automatically get it to source from now on
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
source ~/catkin_ws/devel/setup.bash; source ~/.bashrc
echo $ROS_PACKAGE_PATH
```




- catkin_init_workspace : 작업공간 생성 in `~/catkin_ws/src`
- catkin_create_pkg : 패키지 자동 생성 
  - catkin_create_pkg `<새 패키지 이름> <패키지 Dependencies>`
  - catkin_create_pkg `my_pcl_tutorial` pcl_ros roscpp sensor_msgs
  - `<패키지 Dependencies>`는 생략 후 나중에 package.xml로 지정 가능
- catkin_make : 빌드 
- catkin_find : 검색



---

## 3. [Sample `CMakeLists.txt`](http://www.pointclouds.org/documentation/tutorials/using_pcl_pcl_config.php)

```python 
CMAKE_MINIMUM_REQUIRED (VERSION 2.6 FATAL_ERROR) 

SET ( SRC_FILES ground.cpp )
SET ( EXEC_FILE ground )

# 프로젝트 이름 및 버전
PROJECT(ground)
MESSAGE ( ${CMAKE_PROJECT_NAME} )
#MESSAGE ( [<Type>] <메시지> ) #Type = STATUS, FATAL_ERROR 


find_package(PCL 1.8 REQUIRED)

find_package(catkin REQUIRED COMPONENTS
roscpp
rospy
std_msgs
geometry_msgs
message_generation)

catkin_package()

# 공통 헤더 파일 Include 디렉토리 (-I)
include_directories(
${PCL_INCLUDE_DIRS}   
${catkin_INCLUDE_DIRS}
)


#ADD_LIBRARY ( <라이브러리_이름> [STATIC|SHARED|MODULE] <소스_파일> <소스_파일> ... )

# 공통 링크 라이브러리 (-l)
# LINK_LIBRARIES( <라이브러리> <라이브러리> ... )

# 공통 링크 라이브러리 디렉토리 (-L)

LINK_DIRECTORIES (
${PCL_LIBRARY_DIRS}
${ROS_LIBRARY_DIRS}
)


ADD_DEFINITIONS(${PCL_DEFINITIONS})


ADD_EXECUTABLE ( ${EXEC_FILE} ${SRC_FILES} )


TARGET_LINK_LIBRARIES(${EXEC_FILE}
    ${PCL_LIBRARIES}
    ${catkin_LIBRARIES}
    )

#https://www.tuwlab.com/ece/27260

#.bashrc
#export catkin_INCLUDE_DIRS="/opt/ros/melodic/include"
#export catkin_LIBRARIES="/opt/ros/melodic/lib"

#export PCL_LIBRARY="/usr/local/lib"
#export PCL_INCLUDE_DIRS="/usr/local/include/pcl-1.9"
```





---

## 에러처리 

### fatal error: ros/ros.h: No such file or directory

```python 
vi ~/.bashrc
export catkin_INCLUDE_DIRS="/opt/ros/melodic/include"

vi CMakelists.txt
include_directories(
    ${PCL_INCLUDE_DIRS}
    ${catkin_INCLUDE_DIRS}  ##추가  
    )
```

### undefined reference to `ros::spin()'

```python 
vi CMakeLists.txt
target_link_libraries(ground 
   ${catkin_LIBRARIES} #추가 
   )
```

### 빌드후 패키지를 못 찾는 문제 

```python 
vi ~/.bashrc
source /opt/ros/melodic/setup.bash
source ~/catkin_ws/devel/setup.bash
source ~/catkin_build/devel/setup.bash  #추가 
```





###  Could not find a package configuration file provided by


> rosdep install --from-paths src --ignore-src -r -y


* apt-get install apt-file && apt-file update

  ```python
  CMake Error at /opt/ros/melodic/share/catkin/cmake/catkinConfig.cmake:83 (find_package):
  Could not find a package configuration file provided by "pcl" with any of
  the following names:

    pclConfig.cmake
    pcl-config.cmake

  Add the installation prefix of "pcl" to CMAKE_PREFIX_PATH or set "pcl_DIR"
  to a directory containing one of the above files.  If "pcl" provides a
  separate development package or SDK, be sure it has been installed.

  ```

해결법 #1

```
locate PCLConfig.cmake
vi  XXX.cmake 
set(pcl_DIR "/usr/local/share/pcl/PCLConfig.cmake")
```

해결법 #2

```
cmake .. -DPCL_DIR=/usr/local/share/pcl-1.7
```

해결법 #3 

* run `apt-file search Qt5CoreConfig.cmake`

  ```python
  qtbase5-dev: /usr/lib/x86_64-linux-gnu/cmake/Qt5Core/Qt5CoreConfig.cmake
  qtbase5-gles-dev: /usr/lib/x86_64-linux-gnu/cmake/Qt5Core/Qt5CoreConfig.cmake
  ```

* Install the missing package `sudo apt install qtbase5-dev`

> ref [What package do I need to build...](https://askubuntu.com/questions/374755/what-package-do-i-need-to-build-a-qt-5-cmake-application/374775)

