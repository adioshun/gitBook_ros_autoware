# RQT

> 참고 : [오로카](http://cafe.naver.com/openrt/2963)

## 1. 개요 

- 플러그인 기반의 개발/분석 툴 

    - 주요 플러그인 : qt_bag, rqt_plot, rqt_graph

- 설치 방법 : Desktop-full은 기본 설치, `sudo apt-get install ros-indigo-rqt ros-indigo-rqt-common-plugins`

- 실행 : `rqt` or `rosrun rqt_gui rqt_gui`

- 기타 플러그인 
    - 9.1 액션 (Action)
        - Action Type Browser | Action 타입의 데이터 구조를 확인하는 플러그인 
    - 9.2 구성 (Configuration)
        - Dynamic Reconfigure | 노드들에서 제공하는 설정값 변경을 위한 GUI 설정값 변경 플러그인
        - Launch | roslaunch 의 GUI 플러그인, 로스런치의 이름 및 구성이 생각안날때 매우 유용하다.
    - 9.3 내성 (Introspection)
        - Node Graph | 구동중인 모든 노드들의 관계도 및 메시지의 흐름을 확인할 수 있는 그래프 뷰 형식의 플러그인
        - Package Graph | 노드의 의존 관계를 표시하는 그래프 뷰 형식의 플러그인
        - Process Monitor | 현재 실행중인 노드들의 CPU사용률, 메모리사용륭, 스레드수 등을 확인 가능하다.
    - 9.4 로깅 (Logging)
        - Bag | ROS 데이터 로깅 관련 플러그인
        - Console | 노드들에서 발생되는 경고(Warning), 에러(Error) 등의 메시지들을 한 화면에서 확인 가능한 플러그인
        - Logger Level | ROS의 Debug, Info, Warn, Error, Fatal 로거 정보를 선택하여 표시할 수 있는 GUI 툴
    - 9.5 다양한 툴 (Miscellaneous Tools)
        - Python Console | 파이썬 콘솔 화면 플러그인
        - Shell | 쉘(shell)을 구동하는 플러그인
        - Web | 웹 브라우저를 구동하는 플러그인
    - 9.6 로봇 (Robot)
        - 사용하는 로봇에 따라 계기판(dashboard) 등의 플러그인을 이곳에 추가하면 된다. 
    - 9.7 로봇툴 (Robot Tools)
        - Controller Manager | 컨트롤러 제어에 필요한 플로그인
        - Diagnostic Viewer | 로봇 디바이스 및 에러 확인 플러그인
        - Moveit! Monitor | 로봇 팔 계획에 사용되는 Moveit! 데이터를 확인하는 플러그인
        - Robot Steering | 로봇 조정 GUI 툴, 원격 조정에서 이 GUI 툴을 이용하여 로봇 조종하기에 유용함.
        - Runtime Monitor | 실시간으로 노드들에서 발생되는 에러 및 경고를 확인가능한 플러그인
    - 9.8 서비스 (Services)
        - Service Caller | 구동중인 서비스 서버에 접속하여 서비스를 요청하는 GUI 플러그인, 서비스 테스트에 유용하다.
        - Service Type Browser | 서비스 타입의 데이터 구조를 확인하는 플러그인
    - 9.9 토픽 (Topics)
        - Easy Message Publisher | 토픽을 GUI 환경에서 발행가능 한 플러그인
        - Topic Publisher | 토픽을 생성하여 발행가능한 GUI 플러그인, 토픽 테스트에 매우 유용하다.
        - Topic Type Browser | 토픽 타입의 데이터 구조 확인 플러그인, 토픽 타입 확인에 매우 유용하다.
        - Topic Monitor | 현재 사용중인 토픽을 나열, 그 중에서 사용자가 선택한 토픽의 정보를 확인하는 플러그인
    - 9.10 시각화 (Visualization)
        - Image View | 카메라의 영상 데이터를 확인 가능한 플러그인, 간단한 카메라 데이터 테스트에 유용하다.
        - Navigation Viewer | 로봇 네비게이션의 위치 및 목표지점 확인하는 플러그인
        - Plot | 2차원 데이터 플롯 GUI 플러그인, 2차원 데이터의 도식화에 매우 유용하다.
        - Pose View | 현재 tf의 위치 및 모델의 위치 표시 플러그인
        - RViz | 3차원 시각화 툴인 RViz 플러그인
        - TF Tree | tf 관계를 트리로 나타내는 그래프 뷰 형식의 플러그인
    
## 주요 플러그인 : rqt_plot 

rqt_plot 은 2차원 데이터 플롯 툴이다. 플롯이라하면 좌료를 그리다라는 의미이다. 즉, ROS 메시지를 받아서 이를 좌표에 뿌리게 되는 것을 의미한다. 






![](http://wiki.ros.org/rqt_graph?action=AttachFile&do=get&target=snap_rqt_graph_moveit_demo.png)

[rqt_graph](http://wiki.ros.org/rqt_graph) displays a visual graph of the processes running in ROS and their connections.- 