
# ROS Velodyne 

설치(ubuntu 14) : [Velodyne VLP16 Driver](https://github.com/adioshun/System_Setup/wiki/ROS-Setup#velodyne-installation)


```
# Velodyne 노드 실행 & 데이터 수집 
$ roslaunch velodyne_pointcloud VLP16_points.launch
# Now, the necessary nodes are running. You can check this with the following command

# 실행 노드 확인 
$ rosnode list
# You'll can see the messages being published and subscribed in the following topic:

# 메시지 출력 
$ rostopic echo /velodyne_points
# After That, launch rviz, with the "velodyne" as a fixed frame:

# rviz이용 시각화 
$ rosrun rviz rviz -f velodyne
# In the "displays" panel, click "Add", then select "Point Cloud2", then press "OK".
# In the "Topic" field of the new "Point Cloud2" tab, enter "/velodyne_points"
# Congratulations. Now, your Velodyne is ready to builds the "real" world inside your system. Enjoy it.
```


---

# PointGrey Camera 

[Installation](https://github.com/adioshun/System_Setup/wiki/10_Velodyne_PointGrey#pointgrey)







List detected cameras:`rosrun pointgrey_camera_driver list_cameras`

Launch camera:`roslaunch pointgrey_camera_driver camera.launch`

> ref : [pointgrey_camera_driver](http://wiki.ros.org/pointgrey_camera_driver)





