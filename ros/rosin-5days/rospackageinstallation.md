# ROS 패키지 관리 

패키지 설치 확인 
- `rospack list`
- `rospack find {}`
- `rospack depends-on {}` : 지정된 패키지를 이용하는 패키지 
- `rospack depends {}` : 지정한 패키지가 실행시 필요한 패키지 
- `roslocate info {}` 

패키지 설치 

- apt 설치 : apt-get install ros-{버젼명}-{패키지명}
- Source 설치 : git -> catkin_make 


ROS 패키지 명령어 
- rosinstall 
- rosdep
- rosmake

패키지 인덱스 재 구축
- `rospack profile`