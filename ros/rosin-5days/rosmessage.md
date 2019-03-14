# multi array mesg





## [Float32MultiArray](https://gist.github.com/jarvisschultz/7a886ed2714fac9f5226#file-matrix_sender-py)


```python 

## float64_Array
from std_msgs.msg import Float32MultiArray
from std_msgs.msg import MultiArrayDimension

def rqt_plot(tracker_input):
    msg = Float32MultiArray()
    """
    msg.layout.dim.append(MultiArrayDimension())
    msg.layout.dim.append(MultiArrayDimension())    
    msg.layout.dim[0].label = "height"
    msg.layout.dim[1].label = "width"
    msg.layout.dim[0].size = 3
    msg.layout.dim[1].size = 3    
    msg.layout.dim[0].stride = 3*3
    msg.layout.dim[1].stride = 3
    msg.layout.data_offset = 0
    """
    msg.data = [0]*9       
    msg.data[0] = tracker_input[0]  
    msg.data[1] = tracker_input[1] 
    return msg


msg = rqt_plot(tracker_table)
pub = rospy.Publisher('sent_matrix', Float32MultiArray, queue_size=1)
pub.publish(msg)

## float64

from std_msgs.msg import Float64


pub = rospy.Publisher('cos', Float64)

msg = Float64()
msg.data = math.cos(4*time.time())
pub.publish(msg)

```


## numpy msg (정상 동작 확인 못함)

```python

from rospy.numpy_msg import numpy_msg

self.js_sub = rospy.Subscriber("joint_state_check", numpy_msg(Float32MultiArray), self.js_cb)
self.js_pub = rospy.Publisher("collision_check", Int16, queue_size = 10)



#!/usr/bin/env python
PKG = 'numpy_tutorial'
import roslib; roslib.load_manifest(PKG)

import rospy
from rospy.numpy_msg import numpy_msg
from rospy_tutorials.msg import Floats

import numpy
def talker():
pub = rospy.Publisher('floats', numpy_msg(Floats))
rospy.init_node('talker', anonymous=True)
r = rospy.Rate(10) # 10hz
while not rospy.is_shutdown():
a = numpy.array([1.0, 2.1, 3.2, 4.3, 5.4, 6.5], dtype=numpy.float32)
pub.publish(a)
r.sleep()

if __name__ == '__main__':
talker()

```

