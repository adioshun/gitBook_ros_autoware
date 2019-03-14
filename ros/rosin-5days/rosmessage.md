# multi array mesg


@2010글 : The ROS msg IDL only supports 1D arrays. The reshape approach is the correct approach to ensure compatibility in multiple languages.


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
---

## [Tools for converting ROS messages to and from numpy arrays. Contains two functions:](https://github.com/eric-wieser/ros_numpy)

---

## numpy msg (정상 동작 확인 못함)

http://wiki.ros.org/rospy_tutorials/Tutorials/numpy


```python 
from my_msgs.msg import Floats
from rospy.numpy_msg import numpy_msg

rospy.Subscriber("floats", numpy_msg(Floats), callback)

... and the publisher equivalent

pub = rospy.Publisher('floats', numpy_msg(Floats))
a = numpy.array([1.0, 2.1, 3.2, 4.3, 5.4, 6.5], dtype=numpy.float32)
pub.publish(a)

```



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


---

## numpy n-D msg 

```python 
#!/usr/bin/env python
#This is modified from rospy/src/rospy/numpy_msg.py

import numpy
import roslib
roslib.load_manifest('rospy')
from std_msgs.msg import MultiArrayDimension

# TODO: we will need to generate a new type structure with
# little-endian specified and then pass that type structure into the
# *_numpy calls.

def _serialize_numpy(self, buff):
    """
    wrapper for factory-generated class that passes numpy module into serialize
    """
    # pass in numpy module reference to prevent import in auto-generated code
    if self.layout.dim == []:
        self.layout.dim = [ MultiArrayDimension('dim%d' %i, self.data.shape[i], self.data.shape[i]*self.data.dtype.itemsize) for i in range(len(self.data.shape))];
    self.data = self.data.reshape([1, -1])[0];
    return self.serialize_numpy(buff, numpy)

def _deserialize_numpy(self, str):
    """
    wrapper for factory-generated class that passes numpy module into deserialize    
    """
    # pass in numpy module reference to prevent import in auto-generated code
    self.deserialize_numpy(str, numpy)
    dims=map(lambda x:x.size, self.layout.dim)
    self.data = self.data.reshape(dims)
    return self
    
## Use this function to generate message instances using numpy array
## types for numerical arrays. 
## @msg_type Message class: call this functioning on the message type that you pass
## into a Publisher or Subscriber call. 
## @returns Message class
def numpy_nd_msg(msg_type):
    classdict = { '__slots__': msg_type.__slots__, '_slot_types': msg_type._slot_types,
                  '_md5sum': msg_type._md5sum, '_type': msg_type._type,
                  '_has_header': msg_type._has_header, '_full_text': msg_type._full_text,
                  'serialize': _serialize_numpy, 'deserialize': _deserialize_numpy,
                  'serialize_numpy': msg_type.serialize_numpy,
                  'deserialize_numpy': msg_type.deserialize_numpy
                  }

    # create the numpy message type
    msg_type_name = "Numpy_%s"%msg_type._type.replace('/', '__')
    return type(msg_type_name,(msg_type,),classdict)



if __name__ == '__main__':
  from std_msgs.msg import Float32MultiArray
  import rospy
  def cb(data):
     print "I heard\n",data.data

  rospy.init_node('mynode')
  pub = rospy.Publisher('mytopic', numpy_nd_msg(Float32MultiArray))
  rospy.Subscriber("mytopic", numpy_nd_msg(Float32MultiArray), cb)


  r=rospy.Rate(1);
  while not rospy.is_shutdown():
      a=numpy.array(numpy.random.randn(2,3), dtype=numpy.float32) #please ensure the dtype in identifical to the topic type
      print "sending\n", a
      pub.publish(data=a)
      r.sleep()

```

