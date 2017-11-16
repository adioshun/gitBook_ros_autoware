# ROS Services

## 1. 차이점 (Topics - Services - Actions)

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
    

## 2. Service 구현 하기 
   
### 2.1 서비스 파일 작성 

- 보통 `./{패키지이름}/srv/*.srv`이름으로 생성

```
REQUEST
--- 
RESPONSE
```
- REQUEST contains a string called `model_name`
- `---` : Three dashes means service message, 요청과 응답 부분을 구분 해주는 구분자
- RESPONSE is composed of a boolen named `success`, and a string named `status_message`





### 2.2 클라이언트 노드 작성 `How to call a ROS Service`



![](https://i.imgur.com/iyPW8Em.png)










### 2.3 서버 노드 작성 `How to give a Service`

![](https://i.imgur.com/vcqgbx7.png)

- `rospy.Service('/my_service', Empty, my_callback)` 
	- `my_service` 라는 이름의 서비스 생성 
    - `Empty` 라는 이름의 메시지 `Name of the service that we will use`
    - `my_callback` 서비스 호출시 실행할 함수
    

### 2.4 노드들 빌드 

- cd ~/catkin_ws  
- catkin_make 

### 2.5 노드 실행 

- 서버 : `rosrun {패키지명} {노드명-서버}
- 클라이언트 
    - `rosrun {패키지명} {노드명-클라이언트}
    - `rosservice call /the_service_name "TAB + TAB" ` 
    - GUI툴 이용하는 법 : RQT ServiceCaller


--- 

###### [참고] Service 관련 명령어 

 
  
- To list the available service `rosservice list` 
- To get detailed inforamtion `rosservice info {name_service}`



###### [참고] How to create your own service message : [Youtube](https://youtu.be/KnoIJq7n3m4?t=10m27s)

###### [참고] 서비스에 사용되는 메시지 포맷 확인 하는 법 

How to know the structure of the service message used by the service 

1. you can do a `rosservie info` to know the type of service message that it uses `rosservice infor /name_of_the_service`
2. This will return the `name_of_the_package/Name_of_Service_message`
3. Then you can explore the structure of that service message with the `rossrv show Name_of_the_package/Name_of_Service_message`
	- `rossrv show gazebo_msgs/DeleteModel`
	








