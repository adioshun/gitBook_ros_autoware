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

실행 
1. rosrun plotjuggler PlotJuggler
2. Go to streaming and start ros_topic_streaming
3. simply select your topics you wanna stream