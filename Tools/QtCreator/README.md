# ROS IDE QtCreator with ROS plug-in

http://wiki.ros.org/IDEs#QtCreator

- [ ROS Qt Creator Plug-in wiki](https://ros-industrial.github.io/ros_qtc_plugin/)

## 1. 설치 

> 참고 : [ROS Qt Creator Plug-in 4.4](https://ros-industrial.github.io/ros_qtc_plugin/_source/How-to-Install-Users.html)

- Installation Procedure for Ubuntu 16.04
```
sudo add-apt-repository ppa:levi-armstrong/qt-libraries-xenial
sudo add-apt-repository ppa:levi-armstrong/ppa
sudo apt update && sudo apt install qt59creator
sudo apt install qt57creator-plugin-ros
```

- Installation Procedure for Ubuntu 14.04
```
sudo add-apt-repository ppa:levi-armstrong/qt-libraries-trusty
sudo add-apt-repository ppa:levi-armstrong/ppa
sudo apt-get update && sudo apt-get install qt59creator
sudo apt-get install qt57creator-plugin-ros
```