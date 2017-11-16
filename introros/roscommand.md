## 4. 명령어

> [http://wiki.ros.org/ROS/CommandLineTools](http://wiki.ros.org/ROS/CommandLineTools), [CheatSheet\(pdf\)](http://services.informatik.hs-mannheim.de/~ihme/robotik_2014ws/01_ROScheatsheet.pdf)

### 4.1 ROS BASH

- `rosls`: ROS 패키지 목록 확인

- `roscd <package_name>` : 지정한 ROS패키지로 디렉토리 이동  #$ROS_PACKAGE_PATH 안의 패키지만 검색

- `rospack find {package_name}` : Find ROS package 

- `rosrun` : run executables of a ros package

```
rospd - pushd equivalent of roscd
rosd - lists directories in the directory-stack
rosed - edit a file in a package
roscp - copy a file from a package
```


### 4.2 ROS Package관련 

- rosinstall : ros 추가 패키지 설치  
- rosdep : ros+dep. 해당 패키지의 의존성 파일 설치


### 4.2 ROS 필수 




roscore  
rosrun  
roslaunch  
rosclean: 로그파일 삭제  
rosnode: rosnode list \(활성화된 노드 목록 확인\), rosnode ping \(지정된 노드와 연결 테스트\), rosnode machine \(해당 PC에서 실행되는 노드 목록 확인\), rosnode kill\(지정된 노드 실행 종료\), rosnode cleanup \(유령 노드 삭제\)  
rostopic: rostopic list -v \(토픽 리스트\), rostopic echo \(토픽 메시지 내용 실시간 확인\), rostopic bw \(토픽 메시지 데이터 대역폭 확인\), rostopic hz \(토픽 데이터 퍼블리쉬 주기 표시\), rostopic info \(토픽 정보 표시\), rostopic pub \(토픽 메시지 퍼블리쉬\),   
rosservice  
rosparam: rosparam list, rosparam get /, rosparam dump ~/parameters.yaml, rosparam set, rosparam load  
rosbag: rosbag record, rosbag info, rosvbag play  
rosmsg: rosmsg show,, rosmsg package  
rossrv  
rosversion

catkin\_create\_pkg  
catkin\_make  
catkin\_eclipse  
catkin\_find








