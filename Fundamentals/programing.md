## 2. ros programing(패키지 생성 하기 )

- [참고 코드 :roscd rospy_tutorials](http://wiki.ros.org/rospy_tutorials)

> [Youtube](https://www.youtube.com/watch?v=-GZP81bTuO8), 

### 2.1 초기 작업 

패키지 개발 환경 구축 
- cd ~/catkin_ws/src
- `catkin_create_pkg <새 패키지 이름> <패키지 Dependencies>`
  - python사용을 위해서는 <패키지 Dependencies>에 **rospy**지정 필요 
  - `<패키지 Dependencies>`는 생략 후 나중에 package.xml로 지정 가능 
- `rospack list`로 확인 가능 



파일들 생성 확인 `~/catkin_ws/src/{패키지명}` 
- include 폴더 : 헤더 파일
- launch 폴더 : *.launch파일들 보관 
- src 폴더 : 소스 코드 (eg. Hello_world_node.py)
- CMakeLists.txt : 빌드 설정 파일 (eg.실행 파일 생성, 의존성 패키지 우선 빌드, 링크 생성 등을 설정)
- packages.xml :  패키지 설정 파일 (eg. 패키지의 이름, 저작자, 라이선스, 의존성 패키지 등을 기술)
  - numpy사용시 추가 : `<build_depend>python-numpy</build_depend>` `<run_depend>python-numpy</run_depend>`

### 2.2 빌드파일 설정(`CMakeLists.txt`) 

vi CMakeLists.txt

```
add_message_files(FILES my.msg)  #생성할 메시지 정의

#add_executable(<생성될 실행 파일명>   <생성시 사용할 소스코드> )
add_executable(topic_publisher src/my_topic_publisher.py)  #송신기 정의
add_executable(topic_subscriver src/my_topic_subscriver.py) #수신기 정의
```

### 2.3 메시지 / 실행 파일 생성

vi {패키지명}/msg/my_msg.msg
```
time stamp
int32 data
```

vi {패키지명}/src/my_topic_publisher.py
```python
## Hello_world_node.py
#! /usr/bin/env python
import rospy

rospy.init_node('ObiWan')
print "hello world"
```

vi {패키지명}/src/my_topic_subscriber.py
```

ooo
```




### 2.4 빌드 하기 

- `rospack profile` 선작업으로 로스 패키지의 프로파일을 갱신
- `cd ~/catkin_ws && catkin_make` 실제 빌드 

결과물 
```
~/catkin_ws/build : 설정 내용
~/catkin_ws/devel/lib/{패키지명} : 실행파일
~/catkin_ws/devel/include/{패키지명} : 메시지 헤더 파일
```
    
### 2.5 노드 실행 : 
 - `rosrun {패키지명} my_topic_publisher `
 - `roslaunch <package_name> <*.launch file>`   


    


```bash 
### 작업 폴더 catkin_ws만들기
1. mkdir -p ~/catkin_ws/src
2. cd ~/catkin_ws/src
3. catkin_init_workspace # 
2. cd ~/catkin_ws/
3. catkin_make #build, devel, src 폴더 자동 생성 
4. source devel/setup.bash 실행 # ROS 팩키지 변수가 저장 `echo #ROS_PACKAGE_PATH`

```

```
## 3rd package Install
cd ~/catkin_ws/src
git clone https://github.com/udacity/self-driving-car

cd ~/catkin_ws/src/{package}/
rosdep install --from-paths ./ --ignore-src --rosdistro indigo -y  ##depent package install

cd ~/catkin_ws
catkin_make
source ./devel/setup.sh

rospack profile
rospack list | grep {velodyne}
rospack find {}

```



## 1. ROS roslaunch 

- 하나의 노드 실행은 `rosrun`, 여러개의 노드 실행은 `roslaunch`

- `~/catkin_ws/src/{패키지명}/launch/my.launch`

### 1.1 `launch`파일 작성 

- The launch file contains the **nodes **to be started and their parameters written in **XML format**.

```
<launch>

  <group ns="ns1">
    <node pkg="oroca_ros_tutorials" type="ros_tutorial_msg_publisher"   name="msg_publisher"/>
    <node pkg="oroca_ros_tutorials" type="ros_tutorial_msg_subscriber"  name="msg_subscriber"/>
  </group>

  <group ns="ns2">
    <node pkg="oroca_ros_tutorials" type="ros_tutorial_msg_publisher"  name="msg_publisher"/>
    <node pkg="oroca_ros_tutorials" type="ros_tutorial_msg_subscriber"  name="msg_subscriber"/>
  </group>

</launch>
```  
- `<launch>` 는 로스런치 태그로써 이 태그안에는 로스런치에 필요한 태그들이 기술된다.

- `<node>` 는 로스런치로 실행할 노드를 기술하게 된다. 
  - pkg는 패키지의 이름
  - type는 **실제 실행할 노드의 이름**
  - name은 type를 실행하되 실행할때 붙여지는 이름이다.  

- `<group>` 는 지정된 노드를 그룹으로 묶어주는 태그이다. 
  - 옵션으로는 ns(namespace): 그룹의 이름을 지칭
  
  
- 그외 기타 태그들 
  
  ```
  
  <launch> 로스런치 구문의 시작과 끝을 가르킨다.
  <node> 노드 실행에 대한 태그이다. 패키지, 노드명, 실행명을 변경할 수 있다.
  <machine> 노드를 실행하는 PC의 이름, address,  ros-root,  ros-package-path 등을 설정할 수 있다.
  <include> 다른 패키지 및 같은 패키지에 속해있는 다른 로스런치를 불러와 하나의 파일처럼 실행 시킬 수 있다.
  <remap> 토픽 이름 등의 노드에서 사용중인 로스변수의 이름을 변경할 수 있다. 
  <env> 환경 변수를 변경한다.
  <param> 로스 매개변수를 변경한다
  <rosparam> 로스런치에서 rosparam 의 명령어를 이용하는 태그이다.
  <group> 실행되는 노드를 그룹화할때 사용되는 태그이다.
  <test> 노드를 테스트할 때 사용되는 태그로 <node> 와 비슷하지만 테스트에 사용되는 옵션들이 추가되어 있다.
  <arg> 로스런치에서 사용되는 변수를 정의할 수 있고, 로스런치에서 변수처럼 재사용되어 주소 및 대체 이름등에 사용된다.
  ```


### 1.2 `launch`파일 실행 

- roslaunch {패키지명} {my.launch}
- `roslaunch {패키지명} {my.launch} --screen `





