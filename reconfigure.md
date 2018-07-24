# dynamic_reconfigure

- [ROS홈페이지](http://wiki.ros.org/dynamic_reconfigure)

- [튜토리얼](http://wiki.ros.org/dynamic_reconfigure/Tutorials)
    -


## GUI : `rqt_reconfigure`

## CUI : `$ rosrun dynamic_reconfigure dynparam {COMMAND}`

- list : list configurable nodes, ` $ rosrun dynamic_reconfigure dynparam list`
- get : get node configuration, `$ rosrun dynamic_reconfigure dynparam get /node`
- set : configure node
    - Set the configurable parameter of a node to a value. , `$ rosrun dynamic_reconfigure dynparam set /node parameter_name value`
    - Set multiple configurable parameters of a node to a value.`$ rosrun dynamic_reconfigure dynparam set wge100_camera "{'camera_url':'foo', 'brightness':58}" `
- set_from_parameters : copy configuration from parameter server, `$ rosrun dynamic_reconfigure dynparam set_from_parameters /node`
- dump : dump configuration to file, `$ rosrun dynamic_reconfigure dynparam dump /node dump.yaml`
- load : load configuration from file, `$ rosrun dynamic_reconfigure dynparam load /node dump.yaml`


## 1. 설정 파일 지정

### 1.1 패키지 및 설정 파일 생성
```python
catkin_create_pkg --rosdistro {ROSDISTRO} {dynamic_tutorials} rospy roscpp dynamic_reconfigure
mkdir cfg
vi ./cfg/MY.cfg
```

### 1.2 설정 파일 작성

```python
#!/usr/bin/env python
PACKAGE = "dynamic_tutorials"

from dynamic_reconfigure.parameter_generator_catkin import *

gen = ParameterGenerator()

gen.add("int_param",    int_t,    0, "An Integer parameter", 50,  0, 100)  
gen.add("double_param", double_t, 0, "A double parameter",    .5, 0,   1)
gen.add("str_param",    str_t,    0, "A string parameter",  "Hello World")
gen.add("bool_param",   bool_t,   0, "A Boolean parameter",  True)
"""
gen.add(name, type, level, description, defaults, min, max)
- add function adds a parameter to the list of parameters
    . name - a string which specifies the name under which this parameter should be stored
    . type - defines the type of value stored, and can be any of int_t, double_t, str_t, or bool_t
    . level - A bitmask which will later be passed to the dynamic reconfigure callback. When the callback is called all of the level values for parameters that have been changed are ORed together and the resulting value is passed to the callback.
    . description - string which describes the parameter
    . default - specifies the default value
    . min - specifies the min value (optional and does not apply to strings and bools)
    . max - specifies the max value (optional and does not apply to strings and bools)
"""


size_enum = gen.enum([ gen.const("Small",      int_t, 0, "A small constant"),
                       gen.const("Medium",     int_t, 1, "A medium constant"),
                       gen.const("Large",      int_t, 2, "A large constant"),
                       gen.const("ExtraLarge", int_t, 3, "An extra large constant")],
                     "An enum to set size")

gen.add("size", int_t, 0, "A size parameter which is edited via an enum", 1, 0, 3, edit_method=size_enum)
"""
Here we define an integer whose value is set by an enum.
To do this we call gen.enum and pass it a list of constants followed by a description of the enum.
Now that we have created an enum we can now pass it to the generator.
Now the param can be set with "Small" or "Medium" rather than 0 or 1.
"""

exit(gen.generate(PACKAGE, "dynamic_tutorials", "Tutorials"))
"""
The last line simply tells the generator to generate the necessary files and exit the program.
. The second parameter is the name of a node this could run in (used to generate documentation only),
. the third parameter is a name prefix the generated files will get (e.g. "<name>Config.h" for c++, or "<name>Config.py" for python.
. 중요 : 3rd 파라미터는 cfg파일 이름과 같아야 한다(확장자 제외).
"""

```


### 1.3 설정 파일 사용 가능하게 permission 조정 CMake에 알림

```python
chmod a+x cfg/NY.cfg
python
```

vi  CMakeLists.txt

```python
#add dynamic reconfigure api
#find_package(catkin REQUIRED dynamic_reconfigure)
generate_dynamic_reconfigure_options(
  cfg/Tutorials.cfg
  #...
)
# make sure configure headers are built before any node using them
add_dependencies(example_node ${PROJECT_NAME}_gencfg)

```

## 2. 노드 생성


```python
cd ~/catkin_ws/src/{dynamic_tutorials}
mkdir src
mkdir node
vi ./src/server.py
```
