# [Dynamic dynamic reconfigure python](https://github.com/pal-robotics/ddynamic_reconfigure_python)

> '.cfg' files and 'CMakeLists.txt' 없이 쉽게 하는법 

## 1.  ddynamic_reconfigure추가 

- [ddynamic_reconfigure.py](https://gist.githubusercontent.com/adioshun/07d78f1400cc80b66950494263883482/raw/f9b12ab220b10f8c069f8775da892b190d1f7a5c/ddynamic_reconfigure.py) 파일을 `include`에 추가 

## 2. 적용할 동적 변수/ 추가 

ddynamic_reconfigure.py의 [DyConfigure 클래스] 변경 

```python 
# Add variables (name, description, default value, min, max, edit_method)
self.ddr.add_variable("decimal", "float/double variable", 0.0, -1.0, 1.0)
self.ddr.add_variable("integer", "integer variable", 0, -1, 1)
self.ddr.add_variable("bool", "bool variable", True)
self.ddr.add_variable("string", "string variable", "string dynamic variable")
enum_method = self.ddr.enum([ self.ddr.const("Small",      "int", 0, "A small constant"),
                   self.ddr.const("Medium",     "int", 1, "A medium constant"),
                   self.ddr.const("Large",      "int", 2, "A large constant"),
                   self.ddr.const("ExtraLarge", "int", 3, "An extra large constant")],
                 "An enum example")
self.ddr.add_variable("enumerate", "enumerate variable", 1, 0, 3, edit_method=enum_method)


```
## 3. Main 함수 

```python
import sys
sys.path.append("/workspace/include")
from ddynamic_reconfigure import DDynamicReconfigure 
from ddynamic_reconfigure import DyConfigure

def callback(input_pcl_xyzrgb):    
    EPS = E.dy_eps
    db = DBSCAN(eps=EPS, min_samples=5)

if __name__ == "__main__":
    mot_tracker = Sort()
    rospy.init_node('height_people_detection', anonymous=True)

    E = DyConfigure() #init_node다음에 
    rospy.Subscriber('/velodyne_bg', PointCloud2, callback)

    rospy.spin()

```

---

## sample 

> `ddynamic_reconfigure.py`를 따로 설정시 

```python 

#!/usr/bin/env python3
# coding: utf-8

import sys
sys.path.append("/workspace/include")

from ddynamic_reconfigure import DDynamicReconfigure 

import rospy
from sensor_msgs.msg import PointCloud2
import sensor_msgs.point_cloud2 as pc2

import numpy as np
import pcl
import pcl_msg

import pcl_helper
import filter_helper
import time

import argparse

#from ddynamic_reconfigure import DyConfigure 
class DyConfigure(object):
    def __init__(self):
        # Create a D(ynamic)DynamicReconfigure
        self.ddr = DDynamicReconfigure("class_example")

        # Add variables (name, description, default value, min, max, edit_method)
        self.ddr.add_variable("leaf_size", "float/double variable", 0.3, 0.0, 1.0)
        self.ddr.add_variable("resolution", "float/double variable", 0.1, 0.0, 1.0)
        #self.ddr.add_variable("dy_eps", "DBSCAN eps (float/double)", 1.0, 0.1, 10.0)
        #self.ddr.add_variable("min_samples", "min_samples(integer", 5, 1, 100)
        #self.ddr.add_variable("bool", "bool variable", True)
        #self.ddr.add_variable("string", "string variable", "string dynamic variable")
        enum_method = self.ddr.enum([ self.ddr.const("Small",      "int", 0, "A small constant"),
                           self.ddr.const("Medium",     "int", 1, "A medium constant"),
                           self.ddr.const("Large",      "int", 2, "A large constant"),
                           self.ddr.const("ExtraLarge", "int", 3, "An extra large constant")],
                         "An enum example")
        self.ddr.add_variable("enumerate", "enumerate variable", 1, 0, 3, edit_method=enum_method)

        self.add_variables_to_self()

        self.ddr.start(self.dyn_rec_callback)

    def add_variables_to_self(self):
        var_names = self.ddr.get_variable_names()
        for var_name in var_names:
            self.__setattr__(var_name, None)

    def dyn_rec_callback(self, config, level):
        rospy.loginfo("Received reconf call: " + str(config))
        # Update all variables
        var_names = self.ddr.get_variable_names()
        for var_name in var_names:
            self.__dict__[var_name] = config[var_name]
        return config


class BgRemoval:
    def __init__(self):
        self.lidar_1 = rospy.Subscriber('/lidar_201/velodyne_points', PointCloud2, self.callback1) 
        self.lidar_2 = rospy.Subscriber('/lidar_201/velodyne_points', PointCloud2, self.callback2) 
        self.E = DyConfigure()
        
        

    def callback1(self, lidar1):
        leaf_size = self.E.leaf_size
        print("Lidar1:", leaf_size)

    def callback2(self, lidar2):
        print("Lidar2\n")





if __name__ == '__main__':
    rospy.init_node('BackgroundRemoval', anonymous=True)
    
    
    bg = BgRemoval()
    rospy.spin()
    
```




