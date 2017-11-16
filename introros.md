# ROS

## 1. ROS Features

- ROS provides library and tools for robotic software development. 
    - Original build system (Catkin)
    - Image processing library (OpenCV)
    - Data logging tool (ROSBAG)
    - Visualization tools for data and software state (RViz)
    - Coordinate transformation library (TF)
    - Qt based GUI development tool (RQT)
    
- Inter-process communication
    - ROS uses message passing with **topics** of publish/subscribe form for inter-module connection/cooperation frameworks. 
    - In ROS, processes `(called node)` are **launched** and each node is run independently. 
    - In communication between nodes, by following the publish/subscribe scheme, 
        - a node writes messages (publish) into a topic and 
        - another node reads the messages (subscribe) of the topic.

- File components
    - bag file (ROSBAG)
        - In ROS, all the messages of topics are recorded and time-stamped into a **.bag** file `called ROSBAG`. 
        - This file can be used for replaying the messages on **RViz** as same timing as recording. 
    - Launch file
        - A “Launch” file is used to **start multiple nodes at the same time**. 
        - The launch file contains the **nodes **to be started and their parameters written in **XML format**.
        
        
## 2. 용어 정리 

> 출처 : [daddynkidsmakers](http://daddynkidsmakers.blogspot.com/2015/08/ros-2.html)

### 2.1 마스터 

- 노드와 노드사이의 연결과 메시지 통신을 위한 네임 서버와 같은 역활을 한다. 
- `roscore`가 실행 명령어이다. 
- 실행하면, 각 노드의 이름을 등록하고, 필요에 따라 정보를 받을 수 있다. 

- 통신 방식 : 마스터는 접속하는 **슬래이브**들과의 접속 상태를 유지하지 않는 HTTP 기반의 프로토콜인 **XMLRPC**를 이용하여 슬레이브들과 통신한다. 
    - 이로 인해, 슬레이브가 다운되더라도, 마스터는 다운되는 일이 없으며, 슬레이브의 센서 노드 등이 일부 종료하더라도, 전체 시스템이 다운되는 일은 없다. 
    - 심지어, 다운된 센서 노드가 다시 기동하면, 이 데이터를 받는 노드는 다시 데이터를 받아 처리하도록 되어 있다. 이는 토픽-메시지 기반 데이터 처리 방식이기 때문에 가능한 것이다. 

- 마스터는 사용자가 정해 놓은 ROS_MASTER_URL변수에 기재된 **URI주소**와 포트를 가진다. 
    - 사용자가 설정해 놓지 않으면, URI주소로 현재 로컬 IP를 사용하고, 11311포트를 이용한다.

### 2.2 노드 

- ROS 최소 단위의 실행 프로세서이다. 

- 노드는 생성과 함께 마스터에 노드와 퍼블리셔, 서브스크라이버, 토픽, 서비스의 각 이름, 메시지 형태, URI 주소와 포트를 등록한다. 

- 이 정보들을 기반으로 각 노드는 노드끼리 토픽과 서비스를 이용해 메시지를 주고 받는다.

- 통신 방식 : 노드는 마스터와 통신할 때는 XMLRPC를 이용하고, 노드간 통신에는 XMLRPC나 TCPROS를 사용한다. 
    - 노드간 접속과 질의 응답은 XMLRPC를 사용하고, 메시지 통신은 노드간 직접 통신이므로 TCPPROS를 이용한다. 
    - URI주소는 노드가 실행중인 컴퓨터에 저장된 ROS_HOSTNAME 환경 변수 값으로 URI주소를 사용한다.

### 2.3 패키지 

- ROS 기본단위이다. 
    - 공식 패키지 : www.ros.org/debbuild/indigo.html
    - 사용자 개발 공개 패키지 : http://rosindex.github.io/stats/

- 메타 패키지 - 공통 목적을 지닌 여러 패키지를 모아둔 패키지 집합이다. 

> ros 패키지를 설치 URL정보 : `/etc/apt/sources.list.d/ros-lastest.list` 

### 2.4 메시지 

- 노드 간에 데이터를 주고 받는 단위이다. 

- 정수, 실수 변수의 구조체이며, 배열을 사용할 수 있다. 

- 단방향 메시지 송수신 방식의 토픽과 양방향 메시지 요청/응답 방식의 서비스를 이용한다.

### 2.5 토픽 

- 스토리이며, 퍼블리셔(publisher) 노드가 하나의 스토리에 대해, 토픽이란 이름으로 마스터에 등록한 후, 이 스토리에 대해 메시지 형태로 퍼블리쉬한다. 

- 토픽을 수신받기 위해서, 서브스크라이버(subscriber) 노드가 마스터에 등록된 토픽의 이름에 해당하는 퍼블리셔 노드의 정보를 받는다. 

- 이를 기반으로 서브스크라이버 노드와 퍼블리셔 노드가 직접 연결해 메시지를 토픽으로 송수신한다.

### 2.6 서비스 

- 동기화된 메시지 처리 방식이다.** 일회성 메시지** 통신으로, 서비스 요청과 응답이 완료되면 연결된 두 노드의 접속은 끊긴다. 

### 2.7 캐킨(catkin) 

- ROS빌드 시스템이다. 기본적으로 CMake를 이용한다. 

- ROS는 CMake를 ROS에 맞도록 특화된 캐킨 빌드 시스템을 만들었다.

### 2.8 etc. 

- ROS빌드 - rosbuild는 catkin 빌드 시스템 이전에 사용된 것이다. 

- roscore - 멀티 roscore를 지원하는 특수한 경우를 제외하고, 같은 네트워크에서 하나만 구동된다. 

- 파라미터 - 노드에서 사용하는 파라메터이다. 

- 파라미터 서버 - 패키지에서 파라미터를 사용할 때, 각 파라미터를 등록하는 서버를 말한다. 

- rosrun - ROS 기본 실행 명령이다. 

- roslaunch - 여러 노드를 실행하는 명령이다. 패키지의 파라미터, 노드 이름, 노드 네임스페이스, ROS_ROOT, ROS_PACKAGE_PATH, 환경변수 등 옵션을 제공한다. 
    - *.launch파일을 이용해 실행 노드에 대한 설정을 한다. 

- bag - 주고 받는 메시지의 데이터를 저장할 수 있다. 기록 및 재생이 가능하다.

- 리포지터리 - 리포지터리 패키지가 저장된 웹상의 URL 주소이다. git 등으로 다운로드 가능하다. 

- package.xml - 패키지 정보를 담은 XML파일로, 이름, 저작자, 의존성 패키지 등을 기술하고 있다. 

![](http://barraq.github.io/fOSSa2012/media/topic_protocol.png)

## 3. ROS 파일 시스템 

- 설치 폴더 : `/opt/ros`, 그 안에 roscore등 핵심 유틸리티 등이 설치된다. 
    - /bin 실행 파일
    - /etc ROS 및 catkin 설정 파일
    - /include
    - /lib
    - /share ROS 패키지
    - env.* 환경설정파일
    - setup.* 환경설정파일

- 사용자 작업 폴더: `catkin workspace`
    - /build catkin 빌드 관련 파일
    - /devel caktin 메시지, 서비스 헤더 파일과 사용자 패키지 라이브러리, 실행 파일
    - /src 사용자 패키지

- 사용자 `package` 폴더의 패키지는 다음과 같이 구성된다.
    - /include
    - /launch roslaunch 에 사용되는 launch파일
    - /node rospy용 스크립트
    - /msg 메시지 파일
    - /src 코드 소스
    - /srv 서비스 파일
    - CMakeLists.txt 빌드 설정
    - package.xml  패키지 설정

## 4. 명령어 

> http://wiki.ros.org/ROS/CommandLineTools, [CheatSheet(pdf)](http://services.informatik.hs-mannheim.de/~ihme/robotik_2014ws/01_ROScheatsheet.pdf)

roscd: 지정한 ROS패키지로 디렉토리 이동
rosls: ROS 패키지 목록 확인

roscore
rosrun
roslaunch
rosclean: 로그파일 삭제
rosnode: rosnode list (활성화된 노드 목록 확인), rosnode ping (지정된 노드와 연결 테스트), rosnode machine (해당 PC에서 실행되는 노드 목록 확인), rosnode kill(지정된 노드 실행 종료), rosnode cleanup (유령 노드 삭제)
rostopic: rostopic list -v (토픽 리스트), rostopic echo (토픽 메시지 내용 실시간 확인), rostopic bw (토픽 메시지 데이터 대역폭 확인), rostopic hz (토픽 데이터 퍼블리쉬 주기 표시), rostopic info (토픽 정보 표시), rostopic pub (토픽 메시지 퍼블리쉬), 
rosservice
rosparam: rosparam list, rosparam get /, rosparam dump ~/parameters.yaml, rosparam set, rosparam load
rosbag: rosbag record, rosbag info, rosvbag play
rosmsg: rosmsg show,, rosmsg package
rossrv
rosversion

catkin_create_pkg
catkin_make
catkin_eclipse
catkin_find

rospack
rosinstall: ros 추가 패키지 설치
rosdep: ros+dep. 해당 패키지의 의존성 파일 설치


## 5. 도구 

### 5.1 RViz

- 데이터 가시화를 위한 rviz 프로그램이 있다.

```
$ sudo apt-get install ros-indigo-rviz
$ rosrun rviz rviz
```

### 5.2 rqt

```
$ sudo apt-get install ros-indigo-rqt ros-indigo-rqt-common-plugins
$ rqt_plot
$ rqt

$ sudo apt-get install ros-indigo-uvc-camera
$ rosrun uvc_camera uvc_camera_node
$ rqt

$ rosrun turtlesim turtlesim_node
$ rosrun turtlesim turtle_teleop_key
$ rosrun uvc_camera uvc_camera_node

$ rosrun turtlesim turtlesim_node
$ rqt

$ rqt_plot /turtle1/pos

$ rosrun turtlesim turtle_teleop_key

$ rosrun uvc_camera uvc_camera_node
$ rosbag record /image_raw
$ rqt
```

### 5.3 Gazebo

- 3차원 시뮬레이션을 위한 gazebo 프로그램이 있다.
