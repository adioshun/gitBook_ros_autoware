# [Dynamic dynamic reconfigure python](https://github.com/pal-robotics/ddynamic_reconfigure_python)

> '.cfg' files and 'CMakeLists.txt' 없이 쉽게 하는법 

## 1.  ddynamic_reconfigure추가 

- [ddynamic_reconfigure.py](https://gist.githubusercontent.com/adioshun/07d78f1400cc80b66950494263883482/raw/f9b12ab220b10f8c069f8775da892b190d1f7a5c/ddynamic_reconfigure.py) 파일을 `include`에 추가 


## 2. 적용할 동적 변수/ 추가 

ddynamic_reconfigure.py의 [DyConfigure 클래스] 변경 

```python 
# Add variables (name, description, default value, min, max, edit_method)
self.ddr.add_variable("decimal", "float/double variable", 0.0, -1.0, 1.0)
self.ddr.add_variable("integer", "integer variable", 0, -1, 1)
self.ddr.add_variable("bool", "bool variable", True)
self.ddr.add_variable("string", "string variable", "string dynamic variable")
enum_method = self.ddr.enum([ self.ddr.const("Small",      "int", 0, "A small constant"),
                   self.ddr.const("Medium",     "int", 1, "A medium constant"),
                   self.ddr.const("Large",      "int", 2, "A large constant"),
                   self.ddr.const("ExtraLarge", "int", 3, "An extra large constant")],
                 "An enum example")
self.ddr.add_variable("enumerate", "enumerate variable", 1, 0, 3, edit_method=enum_method)

```
## 3. Main 함수 

```python
import sys
sys.path.append("/workspace/include")
from ddynamic_reconfigure import DDynamicReconfigure 
from ddynamic_reconfigure import DyConfigure

def callback(input_pcl_xyzrgb):    
    EPS = E.dy_eps
    db = DBSCAN(eps=EPS, min_samples=5)

if __name__ == "__main__":
    mot_tracker = Sort()
    rospy.init_node('height_people_detection', anonymous=True)

    E = DyConfigure() #init_node다음에 
    rospy.Subscriber('/velodyne_bg', PointCloud2, callback)

    rospy.spin()

```




