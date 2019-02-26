
## [message_filters](http://wiki.ros.org/message_filters?fbclid=IwAR3El7FSLZTlQheTAG-BxScYOJkba_BEyKK7djtYqCUyF-30uNoyRD0syd0#Time_Synchronizer)

```python 

import message_filters



def callback(velodyne_obstacles, velodyne_clear):
    pcl_obstacles = pcl_helper.ros_to_pcl(velodyne_obstacles)
    pcl_clear = pcl_helper.ros_to_pcl(velodyne_clear)

    arr_obstacles = pcl_obstacles.to_array()
    arr_clear = pcl_clear.to_array()

    result = np.concatenate((arr_obstacles, arr_clear),axis = 0)
   


if __name__ == "__main__":
    rospy.init_node('height_combine', anonymous=True)
    
    velodyne_obstacles = message_filters.Subscriber('/velodyne_obstacles', PointCloud2)
    velodyne_clear = message_filters.Subscriber('/velodyne_clear', PointCloud2)

    ts = message_filters.TimeSynchronizer([velodyne_obstacles, velodyne_clear], 10)
    ts.registerCallback(callback)
    
    rospy.spin()
    
```
    
    