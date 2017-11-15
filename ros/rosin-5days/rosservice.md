## Chapter 3a: ROS Services #Part 1

### 3a.1 Topics - Services - Actions

- 이전장의 Topic만 가지고도 많은 일을 할수 있다. 
    - Topics are insufficient or just too cumbersome to use
    - 좀더 쉽게 일처리 하기 위해 `service`사용 
    
- 서비스를 이해하기 전에 topic과 action을 비교 할수 있어야 한다. 

- **Services** are** Synchronous**. 
    - When your ROS program calls a service, 
    - your program **can not continue** until it receives a result form the service 

- **Actions** are** Asynchronous**. 
    - It's like launching a new **thread**
    - When your ROS program calls an action, your program can perform other tasks while the action is being performed in another thread. 
    
###### [실습] start_demo.launch

- `roslaunch iri_wam_aff_demo start_demo.launch` 실행

- The launch file has launched two nodes 
    - `/iri_wam_reproduce_trajectory` : provides the `/execute_trajectoru` **service**
    - `/iri_wam_aff_demo` : Performs call to that service. 
    
  
- To list the available service `rosservice list` 
- To get detailed inforamtion `rosservice info {name_service}`

   
    

### 3a.2 Services Introduction

### 3a.3 How to call a ROS Service

- `rosservice call /the_service_name "TAB + TAB" ` 


###### [실습] How to know the structure of the service message used by the service 

1. you can do a `rosservie info` to know the type of service message that it uses `rosservice infor /name_of_the_service`
2. This will return the `name_of_the_package/Name_of_Service_message`
3. Then you can explore the structure of that service message with the `rossrv show Name_of_the_package/Name_of_Service_message`
	- `rossrv show gazebo_msgs/DeleteModel`

###### [참고] Service message have TWO pars

```
REQUEST
--- 
RESPONSE
```
- REQUEST contains a string called `model_name`
- `---` : Three dashes means service message 
- RESPONSE is composed of a boolen named `success`, and a string named `status_message`


![](https://i.imgur.com/iyPW8Em.png)


## Chapter 3b: ROS Services #Part 2


### 3b.1 How to give a Service

![](https://i.imgur.com/vcqgbx7.png)

- `rospy.Service('/my_service', Empty, my_callback)` 
	- `my_service` 라는 이름의 서비스 생성 
    - `Empty` 라는 이름의 메시지 `Name of the service that we will use`
    - `my_callback` 서비스 호출시 실행할 함수


### 3b.3 How to create your own service message


> [Youtube](https://youtu.be/KnoIJq7n3m4?t=10m27s)


### 3b.3 Custom Service Compilation


