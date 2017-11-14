# ROS in 5 Days.

- [강의 목차](https://www.robotigniteacademy.com/en/course/ros-in-5-days/details/)
- [Youtube](https://www.youtube.com/watch?v=DBFYZRMLr70&list=PLK0b4e05LnzZWg_7QrIQWyvSPX2WN2ncc)

## 1. Chapter 1: Course Preview 

### 1.1 Topics Unit (Chapters 3 and 4)

- ROS handles almost all its communications through topics. 

- Even more complex communication systems, such as services or actions, rely, at the end, on topics. 

```
roslaunch publisher_example move.launch
```



### 1.2 Services Units (Chapters 5 and 6)
- Services allow you to code a specific functionaility for your robot
    - Service Server, which provides the functionality to anyone who wants to use it (call it). `roslaunch service_demo service_launch.launch`
    - Service Client, who is the one who calls/requests the service functionality.`rosservice call /service_demo "{}"`
    
    

### 1.3 Actions Unit (Chapters 7 and 8)


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

### 1.4 Debugging Tools Unit (Chapter 9)

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

---

## Chapter 2a. ROS Topics - part 1


### 2a.1 Topic Publisher

- A topic is like a pipe. Nodes use topics to publish information for other nodes so they can communicate. 

- `rostopic list` 
- `rostopic list | grep '/{topic이름}` 
- `rostopic echo /Obiwan` # 실시간으로 정보 출력 


![](https://i.imgur.com/Dln3vPe.png)

- create a Publisher that keeps publishing into the '/counter' topic a sequence of integer.

- A **Publisher** is a node that keeps publishing a message into a topic

- A **topic** is a channel that acts as a pipe, where oter ROS nodes can either publish or read information. 소켓??
    
    - To get a list of available topics `rostopic list`
    
    - To read the information that is being published in a topic `rostopic echo <topic_name>`
    - 마지막 메시지만 출력 `rostopic echo <topic_name> -n1`
    - To get information about a certain topic `rostopic info <topic_name>`

### 2a.2 ROS Messages

- Topics handle information through messages

- Messages can be of many types. 

- Messages are defined in `.msg`files, which are located inside a `msg`directory of a package. 

    - To get information about a message `rosmsg show <message>` , `rosmsg show std_msgs/Int32`
    
 
  
     
    
## Chapter 2b. ROS Topics - part 2

### 2b.1 Topic Subscriber

- Publisher와 반대 개념, To read information from topic

- 테스트 전에 publish를 위해 임시 실행 `rostopic pub <topic_name> <message_type> <value>` eg. `rostopic pub /count std_msgs/int32 5`
    - This command will publish the message you specify with the value you specify, in the topic you specify. 
    
![](https://i.imgur.com/k7VWK0B.png)

- create a subscriber node that listens to the /counter topic and each time it reads something it calls a function that does a print of the msg. 

- 당장은 아무 반응 없음, But when you executed the rostopic pub command, you published a message into the /counter topic, so the function has printed number

### 2b.2 Custom Topic Message Compilation

- How to prepare `CMakeLists.txt` and `package.xml` for custom topic message compilation

1. Create a directory named `msg` inside your package
2. Inside this directory, create a file named `Name_of_your_message.msg` eg. `Age.msg`
3. Modify `CMakeLists.txt file`
4. Modify `package.xml` file
5. Compile 
6. Use in code 


```
# ./src/my_publisher/msg/Age.msg

float32 years
float32 months
float32 days
```

> CMakeList.txt 수정하는법 : [Youtube](https://youtu.be/GxpS18INc9s?t=16m30s)


---

## Chapter 3a: ROS Services #Part 1

### 3a.1 Topics - Services - Actions

- 이전장의 Topic만 가지고도 많은 일을 할수 있다. 
    - Topics are insufficient or just too cumbersome to use
    - 좀더 쉽게 일처리 하기 위해 `service`사용 
    
- 서비스를 이해하기 전에 topic과 action을 비교 할수 있어야 한다. 

- **Services** are** Synchronous**. 
    - When your ROS program calls a service, 
    - your program **can not continue** until it receives a result form the service 

- **Actions** are** Asynchronous**. 
    - It's like launching a new **thread**
    - When your ROS program calls an action, your program can perform other tasks while the action is being performed in another thread. 
    
###### [실습] start_demo.launch

- `roslaunch iri_wam_aff_demo start_demo.launch` 실행

- The launch file has launched two nodes 
    - `/iri_wam_reproduce_trajectory` : provides the `/execute_trajectoru` **service**
    - `/iri_wam_aff_demo` : Performs call to that service. 
    
  
- To list the available service `rosservice list` 
- To get detailed inforamtion `rosservice info {name_service}`

   
    

### 3a.2 Services Introduction

### 3a.3 How to call a ROS Service

- `rosservice call /the_service_name "TAB + TAB" ` 


###### [실습] How to know the structure of the service message used by the service 

1. you can do a `rosservie info` to know the type of service message that it uses `rosservice infor /name_of_the_service`
2. This will return the `name_of_the_package/Name_of_Service_message`
3. Then you can explore the structure of that service message with the `rossrv show Name_of_the_package/Name_of_Service_message`
	- `rossrv show gazebo_msgs/DeleteModel`

###### [참고] Service message have TWO pars

```
REQUEST
--- 
RESPONSE
```
- REQUEST contains a string called `model_name`
- `---` : Three dashes means service message 
- RESPONSE is composed of a boolen named `success`, and a string named `status_message`


![](https://i.imgur.com/iyPW8Em.png)


## Chapter 3b: ROS Services #Part 2


### 3b.1 How to give a Service

![](https://i.imgur.com/vcqgbx7.png)

- `rospy.Service('/my_service', Empty, my_callback)` 
	- `my_service` 라는 이름의 서비스 생성 
    - `Empty` 라는 이름의 메시지 `Name of the service that we will use`
    - `my_callback` 서비스 호출시 실행할 함수


### 3b.3 How to create your own service message


> [Youtube](https://youtu.be/KnoIJq7n3m4?t=10m27s)


### 3b.3 Custom Service Compilation


---


## 4. Chapter 4a: ROS Actions #Part 1


### 4a.1 Playing with the Quadrotor simulation

- `rostopic pub /drone/takeoff std_msgs/Empty "{}"`

- `rosrun teleop_twist_keyboard teleop_twist_keyboard.py`

- `rostopic pub /drone/land std_msgs/Empty "{}"`


### 4a.2 What are ROS Actions

- Actions are like asynchronous call to service

- Actions are the same as services. 

- When you call an action, you are calling a functionality that another node is providing. 

- The difference is that when your node calls a service, it must wail the service finished. 
	- WHen your node calls an action, it doesn't necessarily have to wait for the action to complete. 
    
- Hence, an action is an asynchronous call to another node's functionality
	- The node that **provides** the functionality has to contain an **action server**. The action server allows other nodes to call that action functionality. 
    - The node that **calls** to the functionality has to contain an **action client**. The action clien allow a node to connect to the action server to another node 
    
    ![](https://i.imgur.com/Es8OdZE.png)
    
    
![](https://i.imgur.com/i4XM7p6.png)
		

- 서비스의 리스트를 볼때는 `rosservice list`
- 액션의 리스트를 볼때는 ~~`rosaction list`~~ 가 아니라 `rostopic list`

- `top`과 `action`을 어떻게 비교 하나? 일반적으로 아래의 5가지 액션 topic만 쓰는 구조이므로 이걸로 기억 하기 
	- `/{}_action_server/cancel`
	- `/{}_action_server/feedback`
	- `/{}_action_server/goal`
	- `/{}_action_server/result`
	- `/{}_action_server/status`


### 4a.3 Calling an Action Server


- calling an action server mean sending a message to it

- In the same way as with **topics** and **services**, it all works by passing messages around
	- The message of **topic** is composed of a single part : the inforatmion the topic provides
    - The message of a **service** has tow part : the goal and the response
    - The message of an **action** serviver is devided in tree part : the goal, the result, and the feedback. 

- All of the action messages used are defined in the **action directory** of their package
	- 해당 `package`에 가면 `action이란 폴더`가 있음. 해당 폴더 안에 `{}.action`이란 파일 존재 


![](https://i.imgur.com/2VOs6hT.png)

- goal : COnsist of variable called nseconds of type int32. 
	- This int32 type is a standard ROS message, therefore, it can be foun in the `std_msgs_package`
    - Because it's a standard package of ROS, it's net needed to indicate the package where the int32 can be found
    
- Result : Consis of a variable called **allPictures**, an array of type **CompressedImages[]** found in the `sensor_msgs_package`

- feedback : consist of a variable called **lastImage** of type **CompressedImages[]** found in the `sensor_msgs_package`
	- The feedback is a message that the action server generates every once in a while to indicate how the action is going 
    
    
###### [정리] How to call an action server

- The way you call an action server is by implementing an **action client**

![](https://i.imgur.com/XeHqf4S.png)
![](https://i.imgur.com/0bV3wBM.png)
    

> [코드설명](https://youtu.be/LoRXdNMuslQ?t=22m22s)


### 4a.4 Performing other tasks while the Action is in progress

- The simpleActionClient objects have two functions that can be used for knowing if the action that is being performed has finished, and how:
	- **wait_for_result()** 
    - **get_state()** : pending(0), active(1), done(2), warn(3), error(4), 2보다 낮으면 아직 작업중이므로...

###### [코드] wait_for_result()
![](https://i.imgur.com/dZ1cwAA.png)


###### [코드] get_state()
![](https://i.imgur.com/Lm6RSMg.png)





### 4a.5 The axclient

- 지금까지의 아래의 두 방법을 이용하여서 action server에 goal을 전송 하였다. 
	- Publishing directly into the **/goal** topic of the Action Server
    - Publishing the goal unsing Python **code **

- 좀더 편하고 쉬운 방법이 있다. axclient
	- The axclient is a GUI tool provided by the actionlib packages
    - that allows you to interact with an Action Server in a easy/Visual way
    - 실행법 `rosrun actionlib axclient.py /ardrone_action_server` 


![](https://i.imgur.com/U7Zg7ix.png)



## 4. Chapter 4b: ROS Actions #Part 2



### 4b.1 Writing an Action Server

- Directly : 
```
#goal의 type 정보 읽어 오기 
rostopic info /fibonacci_as/goal

# pub로 실행
rostopic pub /fibonacci_as/goal actionlib_tutorials/FibonacciActionGoal "header" TAB + TAB`하여 전체 내용 출력 되면 값 수정 

# 값의 type은 src/..../action폴더의 *.action파일로 확인 가능 
```


- python코드로 실행 

![](https://i.imgur.com/wH6p0iN.png)
![](https://i.imgur.com/QJZMz5G.png)
![](https://i.imgur.com/khw4pqQ.png)

> [코드설명]()

### 4b.2 Creating your own Action Server Message


### 4b.3 Custom Action Messages compilation



