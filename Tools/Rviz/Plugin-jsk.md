# [JSK Plugin](https://jsk-visualization.readthedocs.io/en/latest/jsk_rviz_plugins/index.html)

> [ROS wiki](http://wiki.ros.org/jsk_rviz_plugins), [깃허브](https://github.com/jsk-ros-pkg/jsk_visualization)

## 1. 개요 



## 2. 설치 

```python 
$ apt-get install -y ros-kinetic-jsk-visualization
$ roslaunch /opt/ros/melodic/share/jsk_rviz_plugins/launch/boundingbox_sample.launch
$ rviz -d /opt/ros/melodic/share/jsk_rviz_plugins/config/bounding_box_sample.rviz #Click "PieChart" or "Plotter2D" 

```

## 3. [활용](https://github.com/jsk-ros-pkg/jsk_visualization/tree/master/jsk_rviz_plugins/samples)

### 3.1 

std_msgs::Flaot32only display type messages. It is not possible with Flaot64 or Int32.

### 3.2 [Overlay_text ](https://github.com/jsk-ros-pkg/jsk_visualization/blob/master/jsk_rviz_plugins/samples/overlay_sample.py)


```python 
try:
  from jsk_rviz_plugins.msg import *
except:
  import roslib;roslib.load_manifest("jsk_rviz_plugins")
  from jsk_rviz_plugins.msg import *

from std_msgs.msg import ColorRGBA, Float32


self.text_pub = rospy.Publisher("text_sample", OverlayText, queue_size=1)

text = OverlayText()
text.width = 400
text.height = 600
#text.height = 600
text.left = 10
text.top = 10
text.text_size = 12
text.line_width = 2
text.font = "DejaVu Sans Mono"


text.text = "{} = This is OverlayText plugin= {}.".format(time.time(),2)

text.text = """This is OverlayText plugin.
The update rate is %d Hz.
You can write several text to show to the operators.
New line is supported and automatical wrapping text is also supported.
And you can choose font, this text is now rendered by '%s'
You can specify background color and foreground color separatelly.
Of course, the text is not needed to be fixed, see the counter: %d.
You can change text color like <span style="color: red;">this</span>
by using <span style="font-style: italic;">css</style>.
  """ % (rate, text.font, counter)


text.fg_color = ColorRGBA(25 / 255.0, 1.0, 240.0 / 255.0, 1.0)
text.bg_color = ColorRGBA(0.0, 0.0, 0.0, 0.2)


text_pub.publish(text)

```



---

- [Display information on rviz with ROS course 23 jsk plugin](https://qiita.com/srs/items/96d1facf8ddfb56d97a4)

- [[eBook] Programming ROS-Robot application development with Python (O'Reilly Japan)](https://myenigma.hatenablog.com/entry/2015/10/30/223023)