# 지원 파일들 


## 1. Simulation Description Format(`*.sdf`) 

> [[홈페이지]](http://sdformat.org/), [[파일포맷]](http://sdformat.org/spec?ver=1.6&elem=model), [[참고-Youtube]](https://www.youtube.com/watch?v=sHzC--X0zQE)

- 정의 : SDF is an XML format that describes objects and environments for robot simulators, visualization, and control
  - 로봇/물체의 외형, 동작, 기능 정의 

- 구성 : It contains a complete description for everything from the world level down to the robot level, including:
  - Scene: Ambient lighting, sky properties, shadows.
  - Physics: Gravity, time step, physics engine.
  - Models: Collection of links, collision objects, joints, and sensors.
  - Lights: Point, spot, and directional light sources.
  - Plugins: World, model, sensor, and system plugins.


```xml
<!-- sdf파일 -->

<?xml version="1.0" ?>
<sdf version="1.5">
  <world name="default">
    <physics type="ode">
     ...
    </physics>

    <scene>
     ...
    </scene>  

    <light>
     ...
    </light>
  </world>

  <model name="box">
    <pose>0 0 0.5 0 0 0</pose>
    <static>false</static>

    <link name="link">
      <pose>0 0 0 0 0 0</pose>
      ...
    </link>

    <joint type="revolute" name="my_joint">
      ...
    </joint>

    <plugin filename="libMyPlugin.so" name="my_plugin"/>

  </model>
</sdf>

```

## 2. Unified Robot Description Forma (`*.urdf`)

> [ROS Tutiroai](http://wiki.ros.org/urdf/Tutorials), [Gazebo Tutorial](http://gazebosim.org/tutorials/?tut=ros_urdf), [URDF Examples](http://wiki.ros.org/urdf/Examples)

- 정의 : An XML format for representing a robot model

- 구성 : The description of a robot consists of 
  - a set of link (part) elements, and 
  - a set of joint elements connecting the links together
  
- 시각화(rviz) :  roslaunch urdf_tutorial display.launch model:=multipleshapes.urdf

[URDF Reference](http://enesbot.me/urdf-reference.html)

[이타인클럽-Gazebo Mobile 로봇 모델 만들기](https://opentutorials.org/course/2845/16603)

[ROS_Lesson9-URDF](https://u.cs.biu.ac.il/~yehoshr1/89-685/Fall2015/ROS_Lesson9.pdf): ppt


[Introduction to URDF](https://industrial-training-master.readthedocs.io/en/melodic/_source/session3/Intro-to-URDF.html)


### 2.1  관련 툴 

#### A. URDF 문법검사
- 설치 : sudo apt-get install liburdfdom-tools
- 방법1 $ check_urdf myrobot.urdf
- 방법2 $ rosrun urdf_parser check_urdf myrobot.urdf

#### B. URDF 그래프표시
- 방법1 $ urdf_to_graphiz myrobot.urdf $ evince myrobot.pdf
- 방법2 $ rosrun urdf_parser urdf_to_graphiz brazo.urdf


```xml
<!-- urdf파일 -->

<?xml version="1.0"?>
<robot name="my_robot">

   <link name="link">
     <visual>
       <origin rpy="1.57075 0 0" xyz="0 0 0"/> 
     </visual>
    ...
   </link>

   <joint name="joint1" type="fixed">
    ...
   </joint>

   <plugin filename="libMyPlugin.so" name="my_plugin"/>

</robot>

```

### [참고] [SDF vs. URDF ](https://github.com/modulabs/gazebo-tutorial/wiki/URDF-vs.-SDF)

- URDF는 하나의 'robot' 모델에 대해 기술하는 포맷이며, 'world'상에서 'robot'의 포즈를 결정 할 수 없는 단점이 있다.

- SDF는 'world'상에서 여러개의 'robot' 모델의 포즈를 정의할 수 있을 뿐 만 아니라, 모델간의 'friction' 및 'light', 'heightmaps' 등의 'world'속성 정의도 가능하다.


- URDF-SDF converter
  - Gazebbo 7.0 이전 버전 $ gzsdf print urdfname.urdf > newname.sdf
  - Gazebbo 7.0 이전 버전 $ gz sdf --print my_urdf.urdf > my_sdf.sdf
  - alt) $ /usr/local/Cellar/gazebo8/8.1.0/bin/gz sdf --convert simple_box.urdf


## 3.  XML Macros (`*.xacro` )

> SDF, URDF를 표현 방식 포맷 [[ROS Wiki]](http://wiki.ros.org/xacro)

- 정의 : Xacro 는 XML macro 언어로써, 반복되는 작업을 모듈화하여 재사용성이 용이하도록 도와준다.
  - you can construct shorter and more readable XML files by using macros that expand to larger XML expressions.

- 변환 : rosrun xacro xacro.py model.xacro > model.urdf

[Using Xacro to Clean Up a URDF File](http://wiki.ros.org/urdf/Tutorials/Using%20Xacro%20to%20Clean%20Up%20a%20URDF%20File)


[Understanding URDF and XACRO](https://ni.www.techfak.uni-bielefeld.de/files/URDF-XACRO.pdf): ppt

[Workcell XACRO](https://industrial-training-master.readthedocs.io/en/melodic/_source/session3/Workcell-XACRO.html)

## 4. `*.world`


```xml
<!-- model.config파일 -->

<?xml version="1.0" ?>
<sdf version ="1.4">
  <world name="default">
       <!-- A ground light source -->
   <include>
     <uri>model://sun</uri>
   </include>

       <!-- A ground plane -->
   <include>
     <uri>model://ground_plane</uri>
   </include>

       <!-- Add YOUR ROBOT -->
   <include>
     <uri>model://(package_name)</uri>
     <posd>0.0 0.0 0.0 0.0 0.0 0.0</pose>
   </include>

  </world>
</sdf>


```





## `*.dag `


---


## [Tip] [Download a 3D model and load it into a Gazebo simulation](https://www.youtube.com/watch?v=aP4sDyrRzpU)

1. 다운로드 (*.skp)
2. 스케치업툴로 Load
3. 수정 후 *.dae파일로 저장
4. Blender툴로 load
5. ~/.gazebo/models 폴더 이동 ./test/mesh폴더 생성후 Blender로 생성한 dae파일 저장
6. model.config / model.sdf 파일 생성
- copy ../spoon/* ./ (다른 폴더-spoon-에서 복사 후 에디터로 수정)
- model.config : 모델 이름, 작성자 이름, 간략한 설명 포함
- model.sdf : 무게, 관성(inertia) 정보, collision geometry **WITH `*.dae` File **
7. gazebo 툴 실행 - insert - 모델 찾아 넣기

> The collision element is a direct subelement of the link object, at the same level as the visual tag
> The collision element defines its shape the same way the visual element does, with a geometry tag
> Inertia is the resistance of any physical object to any change in its state of motion, including changes to its speed and direction

```xml
<!-- model.config파일 -->
<model>
<version>1.0</version>
<name>bot_assembly.URDF</name>
<sdf>urdf/bot_assembly.urdf</sdf>
<description>your description here</description>
</model>
```