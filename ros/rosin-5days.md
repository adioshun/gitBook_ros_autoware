# ROS in 5 Days.


## 0. Chapter 0: Course Preview 

### 0.1 Topics Unit (Chapters 3 and 4)

- ROS handles almost all its communications through topics. 

- Even more complex communication systems, such as services or actions, rely, at the end, on topics. 

```
roslaunch publisher_example move.launch
```



### 0.2 Services Units (Chapters 5 and 6)
- Services allow you to code a specific functionaility for your robot
    - Service Server, which provides the functionality to anyone who wants to use it (call it). `roslaunch service_demo service_launch.launch`
    - Service Client, who is the one who calls/requests the service functionality.`rosservice call /service_demo "{}"`
    
    

### 0.3 Actions Unit (Chapters 7 and 8)


- 액션과 서비스는 함수를 코딩할수 있게 한다는 점에서 같다. `Actions are similar to services, in the sense that they also allow you to code a functionality for your robot, and then provide it so that anyone can call it. `

- 가장큰 차이점은 `The main difference between actions and services is that`
    - 서비스는 다른무엇간를 하기 전에 해당 서비스가 종료 되어야 한다.` when you call a service, the robot has to wait until the service has ended before doing something else.`

 -  반면에 액션은 해당 동작을 하면서 다른것도 할수 있따. `On the other hand, when you call an action, your robot can still keep doing something else while performing the action.`
 
- 또 다른 점은 액션은 피드백 기능이 있다. `There are other differences, such as an action allowing you to provide feedback while the action is being performed.`


```
# Execute in WebShell #1
roslaunch action_demo action_launch.launch

# Execute in WebShell #2
roslaunch action_demo_client client_launch.launch
```

### 0.4 Debugging Tools Unit (Chapter 9)

---

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


###### [cat *.launch]

- tag형태의 명령어 집합
- node 태그가 중요 
    - pkg attribute = name of the package
    - type attribute
    - name attribute
    - output attribute
    

### 2.3. Create a ROS Package

- 사용자용 패키지 생성을 위해서는 `catkin_ws`에서 작업해야 함

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
    - 'rostopic echo /Obiwan` # 실시간으로 정보 출력 
    

### 2.5. Compiling a ROS Package

- ./catkin_ws로 이동하여 진행 하여야 함 
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

---

## Chapter 3. ROS Topics - part 1

> [Youtube](https://www.youtube.com/watch?v=wOlfT8GUcCk&t=156s)


### 3.1 Topic Publisher

- A topic is like a pipe. Nodes use topics to publish information for other nodes so they can communicate. 

- `rostopic list` 
- `rostopic list | grep '/{topic이름}` 


![](https://i.imgur.com/Dln3vPe.png)

```
실습 코드
```

- node초기화 
- create a Publisher that keeps publishing into the '/counter' topic a sequence of integer.

- 즉, A Publisher is a node that keeps publishing a message into a topic

- A topic is a channel that acts as a pipe, where oter ROS nodes can either publish or read information. 

- To get a list of available topics `rostopic list`








### 3.2 ROS Messages