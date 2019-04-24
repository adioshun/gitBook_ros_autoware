
# [TF](http://wiki.ros.org/tf)

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


