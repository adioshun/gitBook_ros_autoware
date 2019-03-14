# multi array mesg

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