
# [message_filters](http://docs.ros.org/melodic/api/message_filters/html/python/index.html)

> [Study ROS message_filters](http://robonchu.hatenablog.com/entry/2017/06/11/121000) : Python, Cpp

## Time Synchronizer

> The TimeSynchronizer filter synchronizes incoming channels by the timestamps contained in their headers, and outputs them in the form of a single callback that takes the same number of channels. The C++ implementation can synchronize up to 9 channels.

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
    
    
## ApproximateTime Policy

> The message_filters::sync_policies::ApproximateTime policy uses an adaptive algorithm to match messages based on their timestamp.

```python 

import message_filters
from std_msgs.msg import Int32, Float32

def callback(mode, penalty):
  # The callback processing the pairs of numbers that arrived at approximately the same time

mode_sub = message_filters.Subscriber('mode', Int32)
penalty_sub = message_filters.Subscriber('penalty', Float32)

ts = message_filters.ApproximateTimeSynchronizer([mode_sub, penalty_sub], 10, 0.1, allow_headerless=True)
ts.registerCallback(callback)
rospy.spin()
```