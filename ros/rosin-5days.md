# ROS in 5 Days.


## 0. Chapter 0: Course Preview 

### 0.1 Topics Unit (Chapters 3 and 4)

- ROS handles almost all its communications through topics. 

- Even more complex communication systems, such as services or actions, rely, at the end, on topics. 

```
roslaunch publisher_example move.launch
```



### 0.2 Services Units (Chapters 5 and 6)
- Services allow you to code a specific functionaility for your robot
    - Service Server, which provides the functionality to anyone who wants to use it (call it). `roslaunch service_demo service_launch.launch`
    - Service Client, who is the one who calls/requests the service functionality.`rosservice call /service_demo "{}"`
    
    

### 0.3 Actions Unit (Chapters 7 and 8)


- 액션과 서비스는 함수를 코딩할수 있게 한다는 점에서 같다. `Actions are similar to services, in the sense that they also allow you to code a functionality for your robot, and then provide it so that anyone can call it. `

- 가장큰 차이점은 `The main difference between actions and services is that`
    - 서비스는 다른무엇간를 하기 전에 해당 서비스가 종료 되어야 한다.` when you call a service, the robot has to wait until the service has ended before doing something else.`

 -  반면에 액션은 해당 동작을 하면서 다른것도 할수 있따. `On the other hand, when you call an action, your robot can still keep doing something else while performing the action.`
 
- 또 다른 점은 액션은 피드백 기능이 있다. `There are other differences, such as an action allowing you to provide feedback while the action is being performed.`


```
# Execute in WebShell #1
roslaunch action_demo action_launch.launch

# Execute in WebShell #2
roslaunch action_demo_client client_launch.launch
```

### 0.4 Debugging Tools Unit (Chapter 9)

