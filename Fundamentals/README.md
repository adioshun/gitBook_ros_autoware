# Intro_ROS

> 참고 : [오로카](http://cafe.naver.com/openrt/2500)

- 2007년 미국의 스탠포드 대학 인공지능 연구소(AI LAB)가 진행하던 STAIR(STanford AI Robot) 프로젝트를 위해 Morgan Quigley 이 개발한 "Switchyard" 이라는 시스템에서 시작
- 2008년 미국의 로봇 전문 기업  윌로우게러지라는 회사가 이어받아 ROS라는 이름으로 개발하기 시작
- 2010년 1월 22일날 ROS 1.0 이라는 세상에 나왔다. 



## 1. ROS Features

* ROS provides library and tools for robotic software development.

  * Original build system \(Catkin\)
  * Image processing library \(OpenCV\)
  * Data logging tool \(ROSBAG\)
  * Visualization tools for data and software state \(RViz\)
  * Coordinate transformation library \(TF\)
  * Qt based GUI development tool \(RQT\)

* Inter-process communication

  * ROS uses message passing with **topics** of publish/subscribe form for inter-module connection/cooperation frameworks. 
  * In ROS, processes `(called node)` are **launched** and each node is run independently. 
  * In communication between nodes, by following the publish/subscribe scheme, 
    * a node writes messages \(publish\) into a topic and 
    * another node reads the messages \(subscribe\) of the topic.






  

## 2. 용어 정리

ROS는 메타운영체제(meta operating system) 이다. 메타운영체제는 애플리케이션과 분산 컴퓨팅 자원간의 가상화 레이어로 분선 컴퓨팅 자원을 활용하여 스케줄링, 로드, 감시, 에러 처리 등을 실행하는 시스템이다.

즉, ROS는 윈도우, 리눅스와 같은 기존의 운영체가 아니며, 기존운영체제에 추가적인 설치를 수행한다. 그러므로 기존운영체제에서 사용하던 스케줄링, 파일시스템, 프로세스 관리 등 등을 사용할 수있다. 이러한 컨셉을 미들웨어(Middleware) 또는 소프트웨어 프레임워크(Software framework)라고한다.

ROS는 메타 운영체제기반으로 다양한 응용패키지개발, 관리, 제공 기능을 제공하고 있다.

> 출처 : [daddynkidsmakers](http://daddynkidsmakers.blogspot.com/2015/08/ros-2.html)

### 2.1 마스터

* 노드와 노드사이의 연결과 메시지 통신을 위한 네임 서버와 같은 역활을 한다. 
* `roscore`가 실행 명령어이다. 
* 실행하면, 각 노드의 이름을 등록하고, 필요에 따라 정보를 받을 수 있다.

* 통신 방식 : 마스터는 접속하는 **슬래이브**들과의 접속 상태를 유지하지 않는 HTTP 기반의 프로토콜인 **XMLRPC**를 이용하여 슬레이브들과 통신한다.

  * 이로 인해, 슬레이브가 다운되더라도, 마스터는 다운되는 일이 없으며, 슬레이브의 센서 노드 등이 일부 종료하더라도, 전체 시스템이 다운되는 일은 없다. 
  * 심지어, 다운된 센서 노드가 다시 기동하면, 이 데이터를 받는 노드는 다시 데이터를 받아 처리하도록 되어 있다. 이는 토픽-메시지 기반 데이터 처리 방식이기 때문에 가능한 것이다. 

* 마스터는 사용자가 정해 놓은 ROS\_MASTER\_URL변수에 기재된 **URI주소**와 포트를 가진다.

  * 사용자가 설정해 놓지 않으면, URI주소로 현재 로컬 IP를 사용하고, 11311포트를 이용한다.

### 2.2 노드

* ROS 최소 단위의 실행 프로세서이다.

* 노드는 생성과 함께 마스터에 노드와 퍼블리셔, 서브스크라이버, 토픽, 서비스의 각 이름, 메시지 형태, URI 주소와 포트를 등록한다.

* 이 정보들을 기반으로 각 노드는 노드끼리 토픽과 서비스를 이용해 메시지를 주고 받는다.

* 통신 방식 : 노드는 마스터와 통신할 때는 XMLRPC를 이용하고, 노드간 통신에는 XMLRPC나 TCPROS를 사용한다.

  * 노드간 접속과 질의 응답은 XMLRPC를 사용하고, 메시지 통신은 노드간 직접 통신이므로 TCPPROS를 이용한다. 
  * URI주소는 노드가 실행중인 컴퓨터에 저장된 ROS\_HOSTNAME 환경 변수 값으로 URI주소를 사용한다.
  
- ROS nodes are basically programs made in ROS , 실행중인 프로세스? 
  - `rosnode list`
  - 'rosnode infor /Obiwan`
  - 'rosnode kill /topic_puvlisher`
  
### 2.3 패키지

* ROS 기본단위이다.

  * 공식 패키지 : www.ros.org/debbuild/indigo.html
  * 사용자 개발 공개 패키지 : [http://rosindex.github.io/stats/](http://rosindex.github.io/stats/)

* 메타 패키지 - 공통 목적을 지닌 여러 패키지를 모아둔 패키지 집합이다.

> ros 패키지를 설치 URL정보 : `/etc/apt/sources.list.d/ros-lastest.list`

### 2.4 메시지

* 노드 간에 데이터를 주고 받는 단위이다.

* 정수, 실수 변수의 구조체이며, 배열을 사용할 수 있다.

* 단방향 메시지 송수신 방식의 토픽과 양방향 메시지 요청/응답 방식의 서비스를 이용한다.

### 2.5 토픽

* 스토리이며, 퍼블리셔\(publisher\) 노드가 하나의 스토리에 대해, 토픽이란 이름으로 마스터에 등록한 후, 이 스토리에 대해 메시지 형태로 퍼블리쉬한다.

* 토픽을 수신받기 위해서, 서브스크라이버\(subscriber\) 노드가 마스터에 등록된 토픽의 이름에 해당하는 퍼블리셔 노드의 정보를 받는다.

* 이를 기반으로 서브스크라이버 노드와 퍼블리셔 노드가 직접 연결해 메시지를 토픽으로 송수신한다.

### 2.6 서비스

* 동기화된 메시지 처리 방식이다.** 일회성 메시지** 통신으로, 서비스 요청과 응답이 완료되면 연결된 두 노드의 접속은 끊긴다. 

### 2.7 캐킨\(catkin\)

* ROS빌드 시스템이다. 기본적으로 CMake를 이용한다.

* ROS는 CMake를 ROS에 맞도록 특화된 캐킨 빌드 시스템을 만들었다.


### 2.8 Parameter Server 

- ROS uses to store parameters
- This parameters can be used by Nodes at runtime and are normally used for static data, such as configure parameters
    
    - To get a list of parameters : `rosparam list`
    
    - To get a value of a parameter : `rospapam get <parameter_name?
    
    - To set a value to a parameter : `rosparam set <parameter_name> <value>`



## 3. ROS 파일 시스템

![](https://i.imgur.com/FtI3an4.png)

* 설치 폴더 : `/opt/ros`, 그 안에 roscore등 핵심 유틸리티 등이 설치된다.

  * /bin 실행 파일
  * /etc ROS 및 catkin 설정 파일
  * /include
  * /lib
  * /share ROS 패키지
  * env.\* 환경설정파일
  * setup.\* 환경설정파일
  
![](https://i.imgur.com/A3W5HvG.png)

* 사용자 작업 폴더: `catkin workspace`

  * /build catkin 빌드 관련 파일
  * /devel caktin 메시지, 서비스 헤더 파일과 사용자 패키지 라이브러리, 실행 파일
  * /src 사용자 패키지

* 사용자 `package` 폴더의 패키지는 다음과 같이 구성된다.

  * /include
  * /launch roslaunch 에 사용되는 launch파일
  * /node rospy용 스크립트
  * /msg 메시지 파일
  * /src 코드 소스
  * /srv 서비스 파일
  * CMakeLists.txt 빌드 설정
  * package.xml  패키지 설정
  
## 4. 센서 패키지 

- 1D range finders : 저가의 로봇을 만들떄 사용할만한 적외선 방식의 직선 거리 센서

- 2D range finders : LRF 센서로써 네비게이션에 많이 사용되는 센서들을 모아 두었다.

- 3D Sensors : 프라임센서 기반의 kinect, asus 뿐만 아니라 다양한 3차원 계측에 필요한 센서를 모아 두었다.

- Audio / Speech Recognition : 현재 음성인식 관련 부분은 매우 적다. 지속적으로 추가될 것으로 보인다.

- Cameras : 물체인식, 얼굴인식, 문자판독 등에 많이 사용되는 카메라의 드라이버, 각종 응용 노드들을 모아두었다.

> [Sensors supported by ROS](http://wiki.ros.org/Sensors)




---
# ROS in 5 Days. 강의 전에 Preview

- [강의 목차](https://www.robotigniteacademy.com/en/course/ros-in-5-days/details/)
- [Youtube](https://www.youtube.com/watch?v=DBFYZRMLr70&list=PLK0b4e05LnzZWg_7QrIQWyvSPX2WN2ncc)

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












