
# [TF](http://wiki.ros.org/tf)

```xml
publish tf message defining relative pose of two lidars static_transform_publisher x y z yaw pitch roll frame_id child_frame_id period_in_ms static_transform_publisher 1 0 0 0 0 0 velodyne_1 velodyne_2 10

# tf.launch

<launch>
	<node pkg="tf" type="static_transform_publisher" name="link1_broadcaster" args="0.1 0 0 0.0 0.1 0.1 0.1 velodyne_1 velodyne_2 10" />
</launch>
```

