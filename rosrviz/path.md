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
# https://github.com/NU-MSR/overhead_mobile_tracker/blob/master/src/mobile_tracker.py

```

## ROS Code

```python 
from nav_msgs.msg import Path
from geometry_msgs.msg import PoseStamped

def tranjectory_path(input_path):
        input_path = input_path[0]   
        print("X {}, Y, {}".format(input_path[0], input_path[1]))
        
        pose = PoseStamped()
        pose.header.frame_id = "velodyne"
        pose.pose.position.x = input_path[0]
        pose.pose.position.y = input_path[1] 
        pose.pose.position.z = 0.1 
        #pose.pose.orientation.x = 0.1 
        #pose.pose.orientation.y = 0.1 
        #pose.pose.orientation.z = 0.1 
        #pose.pose.orientation.w = 0.1 
        #print(pose.pose.position.z)
        pose.header.seq = path.header.seq + 1
        path.header.frame_id = "velodyne"
        path.header.stamp = rospy.Time.now()
        pose.header.stamp = path.header.stamp
        path.poses.append(pose)
        
        #print(path)
        pub_path.publish(path)

        return path      

def callback(data):
        tranjectory_path(data)



if __name__ == "__main__":	
	rospy.init_node('people_detection', anonymous=True)

	path = Path() 
	pub_path = rospy.Publisher('/path', Path, queue_size=1)

    	rospy.Subscriber('/velodyne_bgremoval', PointCloud2, callback) #velodyne_points 

    	rospy.spin()
```