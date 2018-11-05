# [rqt_multiplot](http://wiki.ros.org/rqt_multiplot)

- rqt 플러그인화 가능 
- scatter차트 작성 가능 (x,y좌표 시각화)

## 1. 설치법 
sudo apt-get install ros-kinetic-rqt-multiplot

> 설치가 좀 복잡하니 melodic 사용자는 kinetic버젼 docker사용 추천 

## 2. 사용법

사용법 : https://youtu.be/0xRofmn5iYw?t=456


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
# [plotjuggler](http://wiki.ros.org/plotjuggler)


![](https://facontidavide.github.io/PlotJuggler/images/PlotJuggler_terms.png)

최근 버젼에서 ROS Topic 지원 

```python
import rosy
from std_msgs.msg import Float64


rospy.init_node('plot_test')
pub = rospy.Publisher('cos', Float64)
while not rospy.is_shoutdown():
    msg = Float64()
    msg.data = math.cos(4*time.time())
    pub.publish(msg)
    time.sleep(0.1)

# $rqt_plot /sin/data /cos/data
```


설치 : `sudo apt-get install ros-kinetic-plotjuggler`

```bash
# melodic 버
sudo apt-get install qtbase5-dev libqt5svg5-dev ros-melodic-ros-type-introspection 

cd ~/catkin_ws/src

git clone https://github.com/facontidavide/PlotJuggler.git

cd ..
catkin_make
source devel/setup.bash

# $rosrun plotjuggler PlotJuggler 
```

실행 
1. rosrun plotjuggler PlotJuggler
2. Go to streaming and start ros_topic_streaming
3. simply select your topics you wanna stream