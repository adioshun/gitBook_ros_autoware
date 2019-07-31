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

### 3.2 text display

---

- [Display information on rviz with ROS course 23 jsk plugin](https://qiita.com/srs/items/96d1facf8ddfb56d97a4)

- [[eBook] Programming ROS-Robot application development with Python (O'Reilly Japan)](https://myenigma.hatenablog.com/entry/2015/10/30/223023)