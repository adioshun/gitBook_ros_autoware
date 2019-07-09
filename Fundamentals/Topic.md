## Chapter 2a. ROS Topics - part 1

[\[ROSpy\_tutorial\] Writing a Simple Publisher and Subscriber](http://wiki.ros.org/rospy_tutorials/Tutorials/WritingPublisherSubscriber)

CMD창에서 topic Pub 하기 : `$rostopic pub /{토픽이름} std_msgs/String "data : 'hi'" -r {초}

### 2a.1 Topic Publisher

* A topic is like a pipe. Nodes use topics to publish information for other nodes so they can communicate.

* `rostopic list`

* `rostopic list | grep '/{topic이름}` 
* `rostopic echo /Obiwan` \# 실시간으로 정보 출력 

![](https://i.imgur.com/Dln3vPe.png)

* create a Publisher that keeps publishing into the '/counter' topic a sequence of integer.

* A **Publisher** is a node that keeps publishing a message into a topic

* A **topic** is a channel that acts as a pipe, where oter ROS nodes can either publish or read information. 소켓??

  * To get a list of available topics `rostopic list`

  * To read the information that is being published in a topic `rostopic echo <topic_name>`

  * 마지막 메시지만 출력 `rostopic echo <topic_name> -n1`
  * To get information about a certain topic `rostopic info <topic_name>`

### 2a.2 ROS Messages

* Topics handle information through messages

* Messages can be of many types.

* Messages are defined in `.msg`files, which are located inside a `msg`directory of a package.

  * To get information about a message `rosmsg show <message>` , `rosmsg show std_msgs/Int32`

## Chapter 2b. ROS Topics - part 2

### 2b.1 Topic Subscriber

* Publisher와 반대 개념, To read information from topic

* 테스트 전에 publish를 위해 임시 실행 `rostopic pub <topic_name> <message_type> <value>` eg. `rostopic pub /count std_msgs/int32 5`

  * This command will publish the message you specify with the value you specify, in the topic you specify. 

![](https://i.imgur.com/k7VWK0B.png)

* create a subscriber node that listens to the /counter topic and each time it reads something it calls a function that does a print of the msg.

* 당장은 아무 반응 없음, But when you executed the rostopic pub command, you published a message into the /counter topic, so the function has printed number

### 2b.2 Custom Topic Message Compilation

* How to prepare `CMakeLists.txt` and `package.xml` for custom topic message compilation

* Create a directory named `msg` inside your package

* Inside this directory, create a file named `Name_of_your_message.msg` eg. `Age.msg`
* Modify `CMakeLists.txt file`
* Modify `package.xml` file
* Compile 
* Use in code 

```
# ./src/my_publisher/msg/Age.msg

float32 years
float32 months
float32 days
```

> CMakeList.txt 수정하는법 : [Youtube](https://youtu.be/GxpS18INc9s?t=16m30s)

## 통신

![](https://i.imgur.com/ekIY1NU.png)

```
Node 이름 
topic 이름 
메시지 타입 
주소
```

---

```python
#!/usr/bin/env python

import numpy as np

# Callback function for your Point Cloud Subscriber
def pcl_callback(pcl_msg):

  # Convert ROS msg to PCL data
  cloud = ros_to_pcl(pcl_msg) 

  # Create a cloud with each cluster of points having the same color
  clusters_cloud = pcl.PointCloud_PointXYZRGB()
  clusters_cloud.from_list(colored_points)

  # Publish the list of detected objects
  rospy.loginfo('Detected {} objects: {}'.format(len(detected_objects_labels), detected_objects_labels))
  detected_objects_publisher.publish(detected_objects)





if __name__ == '__main__':

  # Initialize ros node
  rospy.init_node('object_markers_pub', anonymous = True)

  # Create Subscribers
  subscriber = rospy.Subscriber("/sensor_stick/point_cloud", pc2.PointCloud2, pcl_callback, queue_size = 1)

  # Create Publishers
  object_markers_publisher = rospy.Publisher("/object_markers", Marker, queue_size = 1)
  detected_objects_publisher = rospy.Publisher("/detected_objects", DetectedObjectsArray, queue_size = 1)

  # Load Model From disk
  model = pickle.load(open('model.sav', 'rb'))
  classifier = model['classifier']
  encoder = LabelEncoder()
  encoder.classes_ = model['classes']
  scaler = model['scaler']

  # Initialize color_list
  get_color_list.color_list = []

  # Spin 
  while not rospy.is_shutdown():
rospy.spin()
```



