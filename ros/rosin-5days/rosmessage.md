# multi array mesg

@2010글 : The ROS msg IDL only supports 1D arrays. The reshape approach is the correct approach to ensure compatibility in multiple languages.

## [Float32MultiArray](https://gist.github.com/jarvisschultz/7a886ed2714fac9f5226#file-matrix_sender-py)

```python
## float64_Array
from std_msgs.msg import Float32MultiArray
from std_msgs.msg import MultiArrayDimension

#subscriber
def radar_callback(input_radar):
    print(input_radar.data)

if __name__ == '__main__':    
    rospy.init_node('listener')
    rospy.Subscriber("/radar_track", Float32MultiArray, radar_callback)
    rospy.spin()


## publisher 
def talker():
    r = rospy.Rate(10) # 10hz
    while not rospy.is_shutdown():
        msg = Float32MultiArray()
        msg.data = []
        msg.data = [1,2,3,4]
        pub.publish(msg)
        r.sleep()

if __name__ == '__main__':    
    rospy.init_node('talker', anonymous=True)
    pub = rospy.Publisher('/radar_track', Float32MultiArray, queue_size=1)
    talker()
```

---

## numpy\_msg

[http://wiki.ros.org/rospy\_tutorials/Tutorials/numpy](http://wiki.ros.org/rospy_tutorials/Tutorials/numpy)

```python
from my_msgs.msg import Floats
from rospy.numpy_msg import numpy_msg

#subscriber
rospy.Subscriber("lidar_track", numpy_msg(Floats), callback)

def callback(data):
    print rospy.get_name(), "I heard %s"%str(data.data)
    data = data.data
    dim = int(data.size)/int(3)
    data = data.reshape(dim,3)
    print(data)
"""
def callback(data):
    print rospy.get_name(), "I heard %s"%str(data.data)
    data = data.data
    #dim = int(data.size)/int(3)
    data = data.reshape(data.size/3,3)
    print(data)

def listener():
    rospy.init_node('listener')
    rospy.Subscriber("lidar_track", numpy_msg(Floats), callback)

    rospy.spin()

if __name__ == '__main__':
    listener()
    """


## publisher 
pub_track = rospy.Publisher('/lidar_track', numpy_msg(Floats), queue_size=1)

msg = []
for i in range:
    msg.append([centroid[0], centroid[1], 0.0])        

msg = np.asarray(msg ,dtype=np.float32)  
pub_track.publish(msg.reshape(-1)) #1D로 변경 하여 전송


"""
def talker():
    pub = rospy.Publisher('lidar_track', numpy_msg(Floats),queue_size=10)
    rospy.init_node('talker', anonymous=True)
    r = rospy.Rate(10) # 10hz
    while not rospy.is_shutdown():
        a = numpy.array([1.0, 2.1, 3.2, 4.3, 5.4, 6.5], dtype=numpy.float32)
        print("a:",a.shape)
        pub.publish(a)
        r.sleep()

if __name__ == '__main__':
    talker()

"""
```

## [Tools for converting ROS messages to and from numpy arrays. Contains two functions:](https://github.com/eric-wieser/ros_numpy)

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



