# 두개 이상의 Topic 수신 하기 

## 1. class 활용 

```
#!/usr/bin/env python3
# coding: utf-8

import rospy
from sensor_msgs.msg import PointCloud2

class BgRemoval:
    def __init__(self):
        self.lidar_1 = rospy.Subscriber('/lidar_201/velodyne_points', PointCloud2, self.callback1) 
        self.lidar_2 = rospy.Subscriber('/lidar_201/velodyne_points', PointCloud2, self.callback2) 

    def callback1(self, lidar1):
        print("Lidar1")

    def callback2(self, lidar2):
        print("Lidar2\n")

if __name__ == '__main__':
    rospy.init_node('Background Removal', anonymous=True)
    bg = BgRemoval()
    rospy.spin()

```
 
---
   
## 2. [message_filters](http://docs.ros.org/melodic/api/message_filters/html/python/index.html) 활용 

> [Study ROS message_filters](http://robonchu.hatenablog.com/entry/2017/06/11/121000) : Python, Cpp

### 2.1 Time Synchronizer

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
    
    
### 2.2 ApproximateTime Policy

> The message_filters::sync_policies::ApproximateTime policy uses an adaptive algorithm to match messages based on their timestamp.

```python 

import message_filters
from std_msgs.msg import Int32, Float32

def callback(mode, penalty):
  # The callback processing the pairs of numbers that arrived at approximately the same time

mode_sub = message_filters.Subscriber('mode', Int32)
penalty_sub = message_filters.Subscriber('penalty', Float32)

ts = message_filters.ApproximateTimeSynchronizer([mode_sub, penalty_sub], queue_size=5, slop=0.1, allow_headerless=True)
ts.registerCallback(callback)
rospy.spin()
```