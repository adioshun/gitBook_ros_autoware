# ROSbag

> [rosbag](https://www.youtube.com/watch?v=pwlbArh_neU&feature=youtu.be): youtube

## 1. 개요

* In ROS, all the messages of topics are recorded and time-stamped into a **.bag** file `called ROSBAG`. 
* This file can be used for replaying the messages on **RViz** as same timing as recording.

## 2. CMD 

저장 : `rosbag record -o chatter.bag /chatter`
- `-a` : 모두 저장

재생 : `rosbag play /{path-to-file}/bagfile_name.bag`
- 반복 `-l` or `--loop`

자르기 
- `rosbag filter input.bag output.bag "t.secs >= 1531425960 and t.secs <= 1531426140"`

pcd로 저장 
- `rosrun pcl_ros bag_to_pcd <input_file.bag> /<topic> <output_directory>`  # pcl_ros 설치 필요 


## 3. 포맷 

```
# This should output something like this: header:

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



> 간혹 용량을 줄이기 위해 `/velodyne_packets`만 있고, `/velodyne_points`가 안 보일경우 [컨버팅 툴](https://github.com/adioshun/Didi_challenge/wiki/Getting-Started#tip-convert-velodyne_packets-to-velodyne_points-출처) 사용


* [ROS bags-TO-Image.ipynb](https://gist.github.com/anonymous/4857f8920c9fc901121a429ead32a7db)
* [ROS bags-TO-Point Clods.ipynb](https://gist.github.com/anonymous/e675ea14113252be321320be62248034)
* [ROS bags-TO-Avi.ipynb](https://gist.github.com/anonymous/fb1e98efe187b2a35b6d91fb5df9e83b)

