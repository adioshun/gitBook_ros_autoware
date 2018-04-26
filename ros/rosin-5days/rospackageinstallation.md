# ROS 패키지 관리 

패키지 설치 확인 
- `rospack list`
- `rospack find {}`
- `rospack depends-on {}` : 지정된 패키지를 이용하는 패키지 
- `rospack depends {}` : 지정한 패키지가 실행시 필요한 패키지 
- `roslocate info {}` # apt-get install python-rosinstall

패키지 설치 

- apt 설치 : apt-get install ros-{버젼명}-{패키지명}
- Source 설치 : git -> catkin_make 


ROS 패키지 명령어 
- rosinstall 
- rosdep
- rosmake

패키지 인덱스 재 구축
- `rospack profile`


---

If you encount following error msg. 

```
Could not find a package configuration file provided by "Qt5Core"
  (requested version 5.0) with any of the following names:

    Qt5CoreConfig.cmake
    qt5core-config.cmake

  Add the installation prefix of "Qt5Core" to CMAKE_PREFIX_PATH or set
  "Qt5Core_DIR" to a directory containing one of the above files.  If
  "Qt5Core" provides a separate development package or SDK, be sure it has
  been installed.
```

run `apt-file search Qt5CoreConfig.cmake`

review the result

```
qtbase5-dev: /usr/lib/x86_64-linux-gnu/cmake/Qt5Core/Qt5CoreConfig.cmake
qtbase5-gles-dev: /usr/lib/x86_64-linux-gnu/cmake/Qt5Core/Qt5CoreConfig.cmake
```

Install the missing package `sudo apt install qtbase5-dev`

> ref [What package do I need to build...](https://askubuntu.com/questions/374755/what-package-do-i-need-to-build-a-qt-5-cmake-application/374775)
