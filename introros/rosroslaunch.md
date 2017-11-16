# ROS roslaunch 

- 하나의 노드 실행은 `rosrun`, 여러개의 노드 실행은 `roslaunch`

- `~/catkin_ws/src/{패키지명}/launch/my.launch`

## 1. `launch`파일 작성 

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


## 2. `launch`파일 실행 

- roslaunch {패키지명} {my.launch}