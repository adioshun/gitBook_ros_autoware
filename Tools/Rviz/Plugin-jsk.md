# [JSK Plugin](https://jsk-visualization.readthedocs.io/en/latest/jsk_rviz_plugins/index.html)

> [ROS wiki](http://wiki.ros.org/jsk_rviz_plugins), [깃허브](https://github.com/jsk-ros-pkg/jsk_visualization)

## 1. 개요 



## 2. 설치 

```python 
$ apt-get install -y ros-melodic-jsk-visualization
$ roslaunch /opt/ros/melodic/share/jsk_rviz_plugins/launch/boundingbox_sample.launch
$ rviz -d /opt/ros/melodic/share/jsk_rviz_plugins/config/bounding_box_sample.rviz #Click "PieChart" or "Plotter2D" 

```

## 3. [활용](https://github.com/jsk-ros-pkg/jsk_visualization/tree/master/jsk_rviz_plugins/samples)

### 3.1 

std_msgs::Flaot32only display type messages. It is not possible with Flaot64 or Int32.

### 3.2 [Overlay_text ](https://github.com/jsk-ros-pkg/jsk_visualization/blob/master/jsk_rviz_plugins/samples/overlay_sample.py)


## text 


```python 
try:
  from jsk_rviz_plugins.msg import *
except:
  import roslib;roslib.load_manifest("jsk_rviz_plugins")
  from jsk_rviz_plugins.msg import *


def jsk_rviz_text(input_text):

    text = OverlayText()
    text.width = 400
    text.height = 600
    text.left = 10
    text.top = 10
    text.text_size = 12
    text.line_width = 2
    text.font = "DejaVu Sans Mono"

    text.text = input_text
    text.fg_color = ColorRGBA(25 / 255.0, 1.0, 240.0 / 255.0, 1.0)
    text.bg_color = ColorRGBA(0.0, 0.0, 0.0, 0.2)
    text_pub.publish(text)

def callback(input_ros_msg):

	text_number = "{} = This is OverlayText plugin= {}.".format(time.time(),2)

	text_number = """This is OverlayText plugin.
	The update rate is %d Hz.
	You can write several text to show to the operators.
	New line is supported and automatical wrapping text is also supported.
	And you can choose font, this text is now rendered by '%s'
	You can specify background color and foreground color separatelly.
	Of course, the text is not needed to be fixed, see the counter: %d.
	You can change text color like <span style="color: red;">this</span>
	by using <span style="font-style: italic;">css</style>.
  	""" % (rate, text.font, counter)


    jsk_rviz_text(text_number)


if __name__ == "__main__":
    
    rospy.init_node('DA_people_detection', anonymous=True



    text_pub = rospy.Publisher("text_sample", OverlayText, queue_size=1)

    rospy.spin()
```


---

## bbox 

```python 
from jsk_recognition_msgs.msg import BoundingBox

def jsk_rviz_bbox(frame_id, pos_x, pos_y, pos_z,scale_x,scale_y,scale_z ):
    counter = 0
    r = rospy.Rate(24)
    box_a = BoundingBox()
    
    box_a.label = 2


    now = rospy.Time.now()
    box_a.header.stamp = now

    
    box_a.header.frame_id = frame_id

    
    q = quaternion_about_axis((counter % 100) * math.pi * 2 / 100.0, [0, 0, 1])
    box_a.pose.orientation.x = q[0]
    box_a.pose.orientation.y = q[1]
    box_a.pose.orientation.z = q[2]
    box_a.pose.orientation.w = q[3]

    box_a.pose.position.x = pos_x
    box_a.pose.position.y = pos_y
    box_a.pose.position.z = pos_z
    
    box_a.dimensions.x = scale_x#1
    box_a.dimensions.y = scale_y#1
    box_a.dimensions.z = scale_z#1
    box_a.value = (counter % 100) / 100.0
    

    bbox_pub.publish(box_a)

    r.sleep()
    counter = counter + 1




def callback(input_ros_msg):
    jsk_rviz_bbox("velodyne", pos_x, pox_y, pos_z,1,1,1)


if __name__ == "__main__":
    
    rospy.init_node('DA_people_detection', anonymous=True



    bbox_pub = rospy.Publisher("bbox", BoundingBox)

    rospy.spin()

```


----

```python 

from jsk_recognition_msgs.msg import BoundingBoxArray

def jsk_rviz_bbox_array(frame_id):
    counter = 0
    r = rospy.Rate(24)
    now = rospy.Time.now()

    box_a = BoundingBox()    
    box_a.label = 2    
    box_a.header.stamp = now    
    box_a.header.frame_id = frame_id

    q = quaternion_about_axis((counter % 100) * math.pi * 2 / 100.0, [0, 0, 1])
    box_a.pose.orientation.x = q[0]
    box_a.pose.orientation.y = q[1]
    box_a.pose.orientation.z = q[2]
    box_a.pose.orientation.w = q[3]
    box_a.dimensions.x = 1
    box_a.dimensions.y = 1
    box_a.dimensions.z = 1
    box_a.value = (counter % 100) / 100.0
            
    box_b = BoundingBox()
    box_b.label = 5
    box_b.header.stamp = now    
    box_b.header.frame_id = frame_id
    
    box_b.pose.orientation.w = 1
    box_b.pose.position.y = 2
    box_b.dimensions.x = (counter % 10 + 1) * 0.1
    box_b.dimensions.y = ((counter + 1) % 10 + 1) * 0.1
    box_b.dimensions.z = ((counter + 2) % 10 + 1) * 0.1
    box_b.value = 1 - (counter % 100) / 100.0
    

    box_arr = BoundingBoxArray()
    box_arr.header.stamp = now
    box_arr.header.frame_id = frame_id

    box_arr.boxes.append(box_a)
    box_arr.boxes.append(box_b)
    bbox_array_pub.publish(box_arr)

    r.sleep()
    counter = counter + 1

def callback(input_ros_msg):
    jsk_rviz_bbox_array("velodyne")


if __name__ == "__main__":
    
    rospy.init_node('DA_people_detection', anonymous=True



    bbox_array_pub = rospy.Publisher("bbox_array", BoundingBoxArray)

    rospy.spin()

```












---

- [Display information on rviz with ROS course 23 jsk plugin](https://qiita.com/srs/items/96d1facf8ddfb56d97a4)

- [[eBook] Programming ROS-Robot application development with Python (O'Reilly Japan)](https://myenigma.hatenablog.com/entry/2015/10/30/223023)
