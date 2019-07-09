## 4. Chapter 4a: ROS Actions #Part 1


### 4a.1 Playing with the Quadrotor simulation

- `rostopic pub /drone/takeoff std_msgs/Empty "{}"`

- `rosrun teleop_twist_keyboard teleop_twist_keyboard.py`

- `rostopic pub /drone/land std_msgs/Empty "{}"`


### 4a.2 What are ROS Actions

- Actions are like asynchronous call to service

- Actions are the same as services. 

- When you call an action, you are calling a functionality that another node is providing. 

- The difference is that when your node calls a service, it must wail the service finished. 
	- WHen your node calls an action, it doesn't necessarily have to wait for the action to complete. 
    
- Hence, an action is an asynchronous call to another node's functionality
	- The node that **provides** the functionality has to contain an **action server**. The action server allows other nodes to call that action functionality. 
    - The node that **calls** to the functionality has to contain an **action client**. The action clien allow a node to connect to the action server to another node 
    
    ![](https://i.imgur.com/Es8OdZE.png)
    
    
![](https://i.imgur.com/i4XM7p6.png)
		

- 서비스의 리스트를 볼때는 `rosservice list`
- 액션의 리스트를 볼때는 ~~`rosaction list`~~ 가 아니라 `rostopic list`

- `top`과 `action`을 어떻게 비교 하나? 일반적으로 아래의 5가지 액션 topic만 쓰는 구조이므로 이걸로 기억 하기 
	- `/{}_action_server/cancel`
	- `/{}_action_server/feedback`
	- `/{}_action_server/goal`
	- `/{}_action_server/result`
	- `/{}_action_server/status`


### 4a.3 Calling an Action Server


- calling an action server mean sending a message to it

- In the same way as with **topics** and **services**, it all works by passing messages around
	- The message of **topic** is composed of a single part : the inforatmion the topic provides
    - The message of a **service** has tow part : the goal and the response
    - The message of an **action** serviver is devided in tree part : the goal, the result, and the feedback. 

- All of the action messages used are defined in the **action directory** of their package
	- 해당 `package`에 가면 `action이란 폴더`가 있음. 해당 폴더 안에 `{}.action`이란 파일 존재 


![](https://i.imgur.com/2VOs6hT.png)

- goal : COnsist of variable called nseconds of type int32. 
	- This int32 type is a standard ROS message, therefore, it can be foun in the `std_msgs_package`
    - Because it's a standard package of ROS, it's net needed to indicate the package where the int32 can be found
    
- Result : Consis of a variable called **allPictures**, an array of type **CompressedImages[]** found in the `sensor_msgs_package`

- feedback : consist of a variable called **lastImage** of type **CompressedImages[]** found in the `sensor_msgs_package`
	- The feedback is a message that the action server generates every once in a while to indicate how the action is going 
    
    
###### [정리] How to call an action server

- The way you call an action server is by implementing an **action client**

![](https://i.imgur.com/XeHqf4S.png)
![](https://i.imgur.com/0bV3wBM.png)
    

> [코드설명](https://youtu.be/LoRXdNMuslQ?t=22m22s)


### 4a.4 Performing other tasks while the Action is in progress

- The simpleActionClient objects have two functions that can be used for knowing if the action that is being performed has finished, and how:
	- **wait_for_result()** 
    - **get_state()** : pending(0), active(1), done(2), warn(3), error(4), 2보다 낮으면 아직 작업중이므로...

###### [코드] wait_for_result()
![](https://i.imgur.com/dZ1cwAA.png)


###### [코드] get_state()
![](https://i.imgur.com/Lm6RSMg.png)





### 4a.5 The axclient

- 지금까지의 아래의 두 방법을 이용하여서 action server에 goal을 전송 하였다. 
	- Publishing directly into the **/goal** topic of the Action Server
    - Publishing the goal unsing Python **code **

- 좀더 편하고 쉬운 방법이 있다. axclient
	- The axclient is a GUI tool provided by the actionlib packages
    - that allows you to interact with an Action Server in a easy/Visual way
    - 실행법 `rosrun actionlib axclient.py /ardrone_action_server` 


![](https://i.imgur.com/U7Zg7ix.png)



## 4. Chapter 4b: ROS Actions #Part 2



### 4b.1 Writing an Action Server

- Directly 

```
#goal의 type 정보 읽어 오기 
rostopic info /fibonacci_as/goal

# pub로 실행
rostopic pub /fibonacci_as/goal actionlib_tutorials/FibonacciActionGoal "header" TAB + TAB`하여 전체 내용 출력 되면 값 수정 

# 값의 type은 src/..../action폴더의 *.action파일로 확인 가능 

```


- python코드로 실행 

![](https://i.imgur.com/wH6p0iN.png)
![](https://i.imgur.com/QJZMz5G.png)
![](https://i.imgur.com/khw4pqQ.png)

> [코드설명]()

### 4b.2 Creating your own Action Server Message


### 4b.3 Custom Action Messages compilation



