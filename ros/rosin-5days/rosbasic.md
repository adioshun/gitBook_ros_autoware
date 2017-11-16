## Chapter 2. ROS Basic Concepts.

- roslaunch <package_name> <launch file>

> [Youtube](https://www.youtube.com/watch?v=-GZP81bTuO8), [강의자료](http://www.theconstructsim.com/ros-for-beginners/)




### 2.1 What's a ROS Package

`roslaunch <package_name> <launch file>`

- All the files that a specific ROS programs contains 
    - launch folder : *.launch파일들 보관 
    - src folder
    - CMakeLists.txt
    - packages.xml : inforamtion & dependency 
    
- go to the ROS package `roscd <package_name>`

![](https://i.imgur.com/LF62ccd.png)

###### [cat *.launch]

- tag형태의 명령어 집합
- node 태그가 중요 
    - pkg attribute = name of the package
    - type attribute
    - name attribute
    - output attribute
    

### 2.3. Create a ROS Package

- 사용자용 패키지 생성을 위해서는 `catkin_ws`에서 작업해야 함

```
작업 폴더 catkin_ws만들기 
1. mkdir -p ~/catkin_ws/src
2. cd ~/catkin_ws
3. catkin_make #build, devel, src 폴더 자동 생성 
4. devel/setup.bash 실행 # ROS 팩키지 변수가 저장 `echo #ROS_PACKAGE_PATH`

```


- 하부 `src`폴더에서 패키지 생성 : `catkin_create_pkt <새 패키지 이름> <패키지 Dependencies>`
    - `rospack list`로 확인 가능 

- `catkin_ws/my_package/src/`에 `my_ros_program.py`파일 생성 

```python
#! /usr/bin/env python
import rospy

rospy.init_node('ObiWan')
print "hello world"

```

- 실행 : `roslaunch 

### 2.4. ROS Noede 

- ROS nodes are basically programs made in ROS , 실행중인 프로세스? 
    - `rosnode list`
    - 'rosnode infor /Obiwan`
    - 'rosnode kill /topic_puvlisher`
    

### 2.5. Compiling a ROS Package

- `./catkin_ws`로 이동하여 진행 하여야 함 
- `catkin_make`로 컴파일 수행 
    - src폴더 밑의 모든것 컴파일, 일부 패키지만 컴파일 하고 싶으면 `catkin_make --only-pkg-with-deps <pagage_name>`으로 컴파일 

### 2.6 Parameter Server 

- ROS uses to store parameters
- This parameters can be used by Nodes at runtime and are normally used for static data, such as configure parameters
    
    - To get a list of parameters : `rosparam list`
    
    - To get a value of a parameter : `rospapam get <parameter_name?
    
    - To set a value to a parameter : `rosparam set <parameter_name> <value>`
    
### 2.7 ROScore

- The roscore is the main process that manages all the ROS system

- You always need to have a roscore running in order to work with ROS : `roscore`

![](http://www.theconstructsim.com/wp-content/uploads/2017/09/ros-for-beginner-fig-8-768x470.png)

### 2.8 Environment Variables 

- ROS uses a set of Linux system environmet variables in oder to work properly. `export | grep ROS`

