[rosbag](http://wiki.ros.org/rosbag) is a command-line tool for recording and playing back messages into "bag" files.

[rqt\_bag](http://wiki.ros.org/rqt_bag) is a visualizer that lets you see data recorded in bag files.

[https://youtu.be/pwlbArh\_neU](https://youtu.be/pwlbArh_neU)

* bag file \(ROSBAG\)
  * In ROS, all the messages of topics are recorded and time-stamped into a **.bag** file `called ROSBAG`. 
  * This file can be used for replaying the messages on **RViz** as same timing as recording. 

# ROSbag

## 1. 개요

## 2. CMD 툴


- Save : `rosbag record -o chatter.bag /chatter`

- Play : `rosbag play /{path-to-file}/bagfile_name.bag`
  - 반복 -l or --loop

- Dislplay : roscore -> `$ rostopic echo /vehicle/gps/fix`
```

This should output something like this: header:

```
  seq: 7318
  stamp:
    secs: 1492883544
    nsecs: 965464774
  frame_id: ''
status:
  status: 0
  service: 1
latitude: 37.4267093333
longitude: -122.07584
altitude: -42.5
position_covariance: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]
position_covariance_type: 0
```

## 3. GUI 툴

### 3.1 rviz이용

1. CMD툴로 ROSbag play 
2. rviz 실행 
   * Switch the **Fixed Frame** to `velodyne` so that we can show point clouds
   * Add the **ROS nodes** that you want to visualize.
   * Select the **By Topic** tab and choose the topic that you want to visualize

> 간혹 용량을 줄이기 위해 `/velodyne_packets`만 있고, `/velodyne_points`가 안 보일경우 [컨버팅 툴](https://github.com/adioshun/Didi_challenge/wiki/Getting-Started#tip-convert-velodyne_packets-to-velodyne_points-출처) 사용



