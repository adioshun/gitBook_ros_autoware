# 패키지 생성 하기 



> [Youtube](https://www.youtube.com/watch?v=-GZP81bTuO8), [강의자료](http://www.theconstructsim.com/ros-for-beginners/)


-  패키지 개발 환경 구축 `catkin_create_pkt <새 패키지 이름> <패키지 Dependencies>`
    - `rospack list`로 확인 가능 


- 파일들 생성 확인 `~/catkin_ws/src/{패키지명}` 
    - include 폴더
    - launch 폴더 : *.launch파일들 보관 
    - src 폴더 : 소스 코드 (eg. Hello_world_node.py)
    - CMakeLists.txt : 실행 파일 생성, 의존성 패키지 우선 빌드, 링크 생성 등을 설정
    - packages.xml :  패키지의 이름, 저작자, 라이선스, 의존성 패키지 등을 기술

```python
## Hello_world_node.py
#! /usr/bin/env python
import rospy

rospy.init_node('ObiWan')
print "hello world"

```

- 빌드 하기 
    - `rospack profile` 선작업으로 로스 패키지의 프로파일을 갱신
    - `cd ~/catkin_ws && catkin_make` 실제 빌드 
    
    
- 노드 실행 : 
   - `rosrun my_first_ros_pkg hello_world_node `
   - `roslaunch <package_name> <*.launch file>`   


    


```bash 
# 작업 폴더 catkin_ws만들기 
1. mkdir -p ~/catkin_ws/src
2. cd ~/catkin_ws/src
3. catkin_init_workspace # 
2. cd ~/catkin_ws/
3. catkin_make #build, devel, src 폴더 자동 생성 
4. source devel/setup.bash 실행 # ROS 팩키지 변수가 저장 `echo #ROS_PACKAGE_PATH`

```






    

