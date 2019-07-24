
# [TF](http://wiki.ros.org/tf)


## 1. [[ROS] Introduction to tf](http://wiki.ros.org/tf/Tutorials/Introduction%20to%20tf)

- [TF tutorials](http://wiki.ros.org/tf/Tutorials)
- [Introduction to tf](http://wiki.ros.org/tf/Tutorials/Introduction%20to%20tf) 

There are essentially two tasks that any user would use tf for, listening for transforms and broadcasting transforms.

Anyone using tf will need to listen for transforms:

-   **Listening for transforms**  - Receive and buffer all coordinate frames that are broadcasted in the system, and query for specific transforms between frames. Check out the writing a tf listener tutorial  [(Python)](http://wiki.ros.org/tf/Tutorials/Writing%20a%20tf%20listener%20%28Python%29)     

-   **Broadcasting transforms**  - Send out the relative pose of coordinate frames to the rest of the system. A system can have many broadcasters that each provide information about a different part of the robot. Check out the writing a tf broadcaster tutorial  [(Python)](http://wiki.ros.org/tf/Tutorials/Writing%20a%20tf%20broadcaster%20%28Python%29) 
    
> Once you are finished with the basic tutorials, you can move on to learn about tf and time. The tf and time tutorial  [(Python)](http://wiki.ros.org/tf/Tutorials/tf%20and%20Time%20%28Python%29)[(C++)](http://wiki.ros.org/tf/Tutorials/tf%20and%20Time%20%28C%2B%2B%29)  teaches the basic principles of tf and time. The advanced tutorial about tf and time  [(Python)](http://wiki.ros.org/tf/Tutorials/Time%20travel%20with%20tf%20%28Python%29)  [(C++)](http://wiki.ros.org/tf/Tutorials/Time%20travel%20with%20tf%20%28C%2B%2B%29)  teaches the principles of time traveling with tf.


---



```xml
publish tf message defining relative pose of two lidars static_transform_publisher x y z yaw pitch roll frame_id child_frame_id period_in_ms static_transform_publisher 1 0 0 0 0 0 velodyne_1 velodyne_2 10

# tf.launch

<launch>
	<node pkg="tf" type="static_transform_publisher" name="link1_broadcaster" args="0.1 0 0 0.0 0.1 0.1 0.1 velodyne_1 velodyne_2 10" />
</launch>
```

### 세번쨰 방법 

`rosrun tf static_transform_publisher 0 0 0 0 0 0 velodyne velodyne_201 10`

static_transform_publisher x y z yaw pitch roll frame_id child_frame_id period_in_ms
- Publish a static coordinate transform to tf using an x/y/z offset in meters and yaw/pitch/roll in radians. 
- (yaw is rotation about Z, pitch is rotation about Y, and roll is rotation about X). 
- The period, in milliseconds, specifies how often to send a transform. 100ms (10hz) is a good value.

static_transform_publisher x y z qx qy qz qw frame_id child_frame_id  period_in_ms
- Publish a static coordinate transform to tf using an x/y/z offset in meters and quaternion. 
- The period, in milliseconds, specifies how often to send a transform. 100ms (10hz) is a good value.


- Rotation matrix Vs. Euler angle 변환 [코드](https://www.learnopencv.com/rotation-matrix-to-euler-angles/), [웹사이트](https://www.andre-gaschler.com/rotationconverter/) ,[시각화검증](http://danceswithcode.net/engineeringnotes/rotations_in_3d/demo3D/rotations_in_3d_tool.html)

---

# tf interactive marker

[tf_interactive_marker.py](https://gist.github.com/awesomebytes/2aa18ba3b821b2f580a2)


---

[Transform](http://harderthan.co.kr/2019/05/08/ros-tf/) : 한글 



[[ROS.org] Setting up your robot using tf](http://wiki.ros.org/navigation/Tutorials/RobotSetup/TF)


[TF ROS - Full Course for Beginners]() : Youtube, 영문, 1:27분

[ROS study notes - the use of tf](https://www.twblogs.net/a/5bde026a2b717720b51b323d)

