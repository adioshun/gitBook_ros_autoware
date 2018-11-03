# [Rviz Tools for python](https://github.com/DavidB-CMU/rviz_tools_py)

![](https://camo.githubusercontent.com/b054746bd80be86bdc706b6ae91cdf6ce8bf0737/68747470733a2f2f7261772e6769746875622e636f6d2f4461766964422d434d552f7276697a5f746f6f6c735f70792f6d61737465722f64656d6f5f6d61726b657273312e706e67)

rviz marker 기능을 쉽게 사용


```python 
#------------marker-----------------

import roslib
from geometry_msgs.msg import Pose, Point, Quaternion, Vector3, Polygon
from tf import transformations # rotation_matrix(), concatenate_matrices()
import rviz_tools_py as rviz_tools

#------------------------------------

```

```python
#------------marker---------------------
markers = rviz_tools.RvizMarkers('/velodyne', 'visualization_marker')

# Text:

# Publish some text using a ROS Pose Msg
P = Pose(Point(3,-1,0),Quaternion(0,0,0,1))
scale = Vector3(0.2,0.2,0.2)
markers.publishText(P, 'This is some text_korean', 'white', scale, 5.0) # pose, text, color, scale, lifetime


# Cube / Cuboid:

# Publish a cuboid using a numpy transform matrix
T = transformations.translation_matrix((0.6,2.2,0))
scale = Vector3(1.5,5.2,0.2)
markers.publishCube(T, 'red', scale, 5.0) # pose, color, scale, lifetime

#-------------------------------

```

---
## Visualising the real time trajectory path using markers

https://answers.ros.org/question/279558/visualising-the-real-time-trajectory-path-using-markers/


https://youtu.be/0BlFQ8F5vhc