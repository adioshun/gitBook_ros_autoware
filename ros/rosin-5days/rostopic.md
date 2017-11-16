## Chapter 2a. ROS Topics - part 1



### 2a.1 Topic Publisher

- A topic is like a pipe. Nodes use topics to publish information for other nodes so they can communicate. 

- `rostopic list` 
- `rostopic list | grep '/{topic이름}` 
- `rostopic echo /Obiwan` # 실시간으로 정보 출력 


![](https://i.imgur.com/Dln3vPe.png)

- create a Publisher that keeps publishing into the '/counter' topic a sequence of integer.

- A **Publisher** is a node that keeps publishing a message into a topic

- A **topic** is a channel that acts as a pipe, where oter ROS nodes can either publish or read information. 소켓??
    
    - To get a list of available topics `rostopic list`
    
    - To read the information that is being published in a topic `rostopic echo <topic_name>`
    - 마지막 메시지만 출력 `rostopic echo <topic_name> -n1`
    - To get information about a certain topic `rostopic info <topic_name>`

### 2a.2 ROS Messages

- Topics handle information through messages

- Messages can be of many types. 

- Messages are defined in `.msg`files, which are located inside a `msg`directory of a package. 

    - To get information about a message `rosmsg show <message>` , `rosmsg show std_msgs/Int32`
    
 
  
     
    
## Chapter 2b. ROS Topics - part 2

### 2b.1 Topic Subscriber

- Publisher와 반대 개념, To read information from topic

- 테스트 전에 publish를 위해 임시 실행 `rostopic pub <topic_name> <message_type> <value>` eg. `rostopic pub /count std_msgs/int32 5`
    - This command will publish the message you specify with the value you specify, in the topic you specify. 
    
![](https://i.imgur.com/k7VWK0B.png)

- create a subscriber node that listens to the /counter topic and each time it reads something it calls a function that does a print of the msg. 

- 당장은 아무 반응 없음, But when you executed the rostopic pub command, you published a message into the /counter topic, so the function has printed number

### 2b.2 Custom Topic Message Compilation

- How to prepare `CMakeLists.txt` and `package.xml` for custom topic message compilation

1. Create a directory named `msg` inside your package
2. Inside this directory, create a file named `Name_of_your_message.msg` eg. `Age.msg`
3. Modify `CMakeLists.txt file`
4. Modify `package.xml` file
5. Compile 
6. Use in code 


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

> TutleSim을 이용하여 설명 

###### Core 실행 
- The roscore is the main process that manages all the ROS system
- You always need to have a roscore running in order to work with ROS : `roscore`


###### turtlesim 패키지의 turtlesim_node 실행

- `rosrun turtlesim turtlesim_node`

###### turtlesim 패키지의 turtle_teleop_key 실행

- `rosrun turtlesim turtle_teleop_key`

###### Topic이용 하여 조작 하기 

- 키보드 조작 하여 움직이기 

- `/turtle1/cmd_vel `

> rqt_graph 패키지의 rqt_graph 실행 `rosrun rqt_graph rqt_graph` 






