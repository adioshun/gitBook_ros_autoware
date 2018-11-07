https://prieuredesion.github.io/robotics/blog/2018/03/04/odom-to-path.html



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
```