[ROS의 시리얼 통신 패키지 rosserial사용 및 OpenCR 보드의 IMU와 통신 예제 따라해보기](https://pinkwink.kr/1060)

pip install pyserial
sudo apt-get install ros-melodic-rosserial
rosrun rosserial_python serial_node.py _port:=/dev/ttyACM1 _baud:=115200
/opt/ros/melodic/lib/rosserial_python/serial_node.py