https://prieuredesion.github.io/robotics/blog/2018/03/04/odom-to-path.html

https://answers.ros.org/question/278616/how-to-create-a-publisher-about-trajectory-path-then-show-it-in-rviz/

https://www.youtube.com/watch?v=fsV4VyP7N2o


nav_msgs/Path
```python 

std_msgs/Header header
    uint32 seq
    time stamp
    string frame_id
geometry_msgs/PoseStamped[] poses
    std_msgs/Header header
        uint32 seq
        time stamp
        string frame_id
    geometry_msgs/Pose pose
        geometry_msgs/Point position
            float64 x
            float64 y
            float64 z
        geometry_msgs/Quaternion orientation
            float64 x
            float64 y
            float64 z
            float64 w


def publish_path(self, ps):
    path = Path()
    path.header.frame_id = self.camera_frame_id
    path.header.stamp = ps.header.stamp
    self.path_list.append(ps)
    path.poses = self.path_list
    self.path_pub.publish(path)
    return
        
def alvarcb(self, markers):
    rospy.logdebug("Detected markers!")
    # can we find the correct marker?
    for m in markers.markers:
        if m.id == self.marker_id:
            odom_meas = Odometry()
            odom_meas.header.frame_id = self.frame_id
            m.pose.header.frame_id = self.camera_frame_id
            odom_meas.child_frame_id = self.base_frame_id
            odom_meas.header.stamp = m.header.stamp
            m.pose.header.stamp = m.header.stamp
            # now we need to transform this pose measurement from the camera
            # frame into the frame that we are reporting measure odometry in
            pose_transformed = self.transform_pose(m.pose)
            if pose_transformed is not None:
                odom_meas.pose.pose = pose_transformed.pose
                # Now let's add our offsets:
                odom_meas = odom_conversions.odom_add_offset(odom_meas, self.odom_offset)
                self.meas_pub.publish(odom_meas)
                self.send_transforms(odom_meas)
                self.publish_path(m.pose)
    return

# https://github.com/NU-MSR/overhead_mobile_tracker/blob/master/src/mobile_tracker.py

```