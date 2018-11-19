## 1. Simulation Description Format(`*.sdf`) 

> [[홈페이지]](http://sdformat.org/), [[메뉴얼]](http://sdformat.org/spec?ver=1.6&elem=model)

### 1.1 정의 

- SDF is an XML format that describes objects and environments for robot simulators, visualization, and control

- 로봇/물체의 외형, 동작, 기능 정의 , 

- [Youtube-Simulation Description Format (SDF) intro in Gazebo](https://www.youtube.com/watch?v=sHzC--X0zQE)





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



```xml
<!-- model.config파일 -->
<model>
     <version>1.0</version>
     <name>bot_assembly.URDF</name>
     <sdf>urdf/bot_assembly.urdf</sdf>
     <description>your description here</description>
</model>
```

## URDF파일 

## `*.world`


##  `*.xacro` 


## `*.dag `


