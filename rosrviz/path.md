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
```