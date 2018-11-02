# [Dynamic dynamic reconfigure python](https://github.com/pal-robotics/ddynamic_reconfigure_python)

> '.cfg' files and 'CMakeLists.txt' 없이 쉽게 하는법 

## 1.  ddynamic_reconfigure추가 

- 아래 파일을 `include`에 추가 하고 `from ddynamic_reconfigure import DDynamicReconfigure`
- (또는) wget https://raw.githubusercontent.com/pal-robotics/ddynamic_reconfigure_python/master/src/ddynamic_reconfigure_python/ddynamic_reconfigure.py

```python 
#!/usr/bin/env python

"""
Dynamic dynamic reconfigure server.

Just register your variables for the dynamic reconfigure
and call start with a callback.

Author: Sammy Pfeiffer
"""

from dynamic_reconfigure.server import Server
from dynamic_reconfigure.parameter_generator_catkin import ParameterGenerator
from dynamic_reconfigure.encoding import extract_params
from rospkg import RosPack
import rospy


class DDynamicReconfigure(ParameterGenerator):
    """Dynamic reconfigure server that can be instanced directly."""

    def __init__(self, name=None):
        global id
        self.group = self.Group(self, "Default", "", True, 0, 0)
        id = 1
        if name is None:
            self.name = rospy.get_name() + "_dyn_rec"
        else:
            self.name = name
        self.constants = []
        rp = RosPack()
        self.dynconfpath = rp.get_path('dynamic_reconfigure')

    def get_type(self):
        class TypeClass(object):
            def __init__(self, config_description):
                self.config_description = config_description
                self.min = {}
                self.max = {}
                self.defaults = {}
                self.level = {}
                self.type = {}
                self.all_level = 0

                for param in extract_params(config_description):
                    self.min[param['name']] = param['min']
                    self.max[param['name']] = param['max']
                    self.defaults[param['name']] = param['default']
                    self.level[param['name']] = param['level']
                    self.type[param['name']] = param['type']
                    self.all_level = self.all_level | param['level']
        return TypeClass(self.group.to_dict())

    def add_variable(self, name, description, default=None, min=None, max=None, edit_method=""):
        """Register variable, like gen.add() but deducting the type"""
        if type(default) == int:
            if edit_method == "":
                self.add(name, "int", 0, description, default, min, max)
            else: # enum
                self.add(name, "int", 0, description, default, min, max, edit_method)
        elif type(default) == float:
            self.add(name, "double", 0, description, default, min, max)
        elif type(default) == str:
            self.add(name, "str", 0, description, default)
        elif type(default) == bool:
            self.add(name, "bool", 0, description, default)

        return default


    def get_variable_names(self):
        """Return the names of the dynamic reconfigure variables"""
        names = []
        for param in self.group.parameters:
            names.append(param['name'])
        return names


    def start(self, callback):
        self.dyn_rec_srv = Server(self.get_type(), callback, namespace=self.name)
```


## 2. main 함수 수정 


```python
import rospy

import sys
sys.path.append("/workspace/include")


#from ddynamic_reconfigure_python.ddynamic_reconfigure import DDynamicReconfigure
from ddynamic_reconfigure import DDynamicReconfigure

def dyn_rec_callback(config, level):
    #rospy.loginfo("Received reconf call: " + str(config))
    rospy.loginfo("""Reconfigure Request: {decimal}""".format(**config))
    return config

if __name__ == '__main__':
    rospy.init_node('test_ddynrec')

    # Create a D(ynamic)DynamicReconfigure
    ddynrec = DDynamicReconfigure("example_dyn_rec")

    # Add variables (name, description, default value, min, max, edit_method)
    ddynrec.add_variable("decimal", "float/double variable", 0.0, -1.0, 1.0)
    ddynrec.add_variable("integer", "integer variable", 0, -1, 1)
    ddynrec.add_variable("bool", "bool variable", True)
    ddynrec.add_variable("string", "string variable", "string dynamic variable")
    enum_method = ddynrec.enum([ ddynrec.const("Small",      "int", 0, "A small constant"),
                       ddynrec.const("Medium",     "int", 1, "A medium constant"),
                       ddynrec.const("Large",      "int", 2, "A large constant"),
                       ddynrec.const("ExtraLarge", "int", 3, "An extra large constant")],
                     "An enum example")
    ddynrec.add_variable("enumerate", "enumerate variable", 1, 0, 3, edit_method=enum_method)

    # Start the server
    ddynrec.start(dyn_rec_callback)

    rospy.spin()


```

---

# dynamic_reconfigure

- [ROS홈페이지](http://wiki.ros.org/dynamic_reconfigure)

- [튜토리얼](http://wiki.ros.org/dynamic_reconfigure/Tutorials)

- [Youtube: How to work with ROS Dynamic Reconfigure](https://www.youtube.com/watch?v=YKZkZSVcsnI&t=0s&list=WL&index=26)

- cfg파일 / CMakeList.txt 설정 없이 python에서 바로 사용 하는 법 : [Dynamic dynamic reconfigure python](https://github.com/pal-robotics/ddynamic_reconfigure_python)

## GUI :

`rqt_reconfigure`
`sudo apt-get install ros-$DISTRO-rqt-reconfigure`

## CUI :

`$ rosrun dynamic_reconfigure dynparam {COMMAND}`

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
```
vi ./node/server.py
```python
#!/usr/bin/env python

import rospy

from dynamic_reconfigure.server import Server
from dynamic_tutorials.cfg import TutorialsConfig #설정 파일
#The name TutorialsConfig is automatically generated by appending Config to the 3rd argument in gen.generate

def callback(config, level):
    rospy.loginfo("""Reconfigure Request: {int_param}, {double_param},\
          {str_param}, {bool_param}, {size}""".format(**config))
    return config

if __name__ == "__main__":
    rospy.init_node("dynamic_tutorials", anonymous = True)

    srv = Server(TutorialsConfig, callback)
    rospy.spin()
```

## 3. 실행

```python
chmod +x nodes/server.py
rosrun rqt_gui rqt_gui -s reconfigure
```

vi ./node/client.py

```python
줄 번호 보이기/숨기기
#!/usr/bin/env python

import rospy

import dynamic_reconfigure.client

def callback(config):
    rospy.loginfo("Config set to {int_param}, {double_param}, {str_param}, {bool_param}, {size}".format(**config))

"""
We then define a callback which will print the config returned by the server.
. There are two main differences between this callback and the servers, one it does not need to return an updated config object, two it does not have the "level" argument.
. This callback is optional.
"""


if __name__ == "__main__":
    rospy.init_node("dynamic_client")

    client = dynamic_reconfigure.client.Client("dynamic_tutorials", timeout=30, config_callback=callback)

    r = rospy.Rate(0.1)
    x = 0
    b = False
    while not rospy.is_shutdown():
        x = x+1
        if x>10:
            x=0
        b = not b
        client.update_configuration({"int_param":x, "double_param":(1/(x+1)), "str_param":str(rospy.get_rostime()), "bool_param":b, "size":1})
        r.sleep()

"""
Lastly we initialize ROS and our Client.
. Our main loop runs once every ten seconds and simply calls update_configuration on the client every time.
. Note that you need not use a full configuration and could also pass in a dictionary with only one of the parameters as well.
"""

```

Run It!

Once again make the node executable: chmod +x nodes/client.py
Launch a core and your server node, and launch your new client node.
If you've done everything correctly, both your client and server should begin to print out matching configurations every ten seconds.


