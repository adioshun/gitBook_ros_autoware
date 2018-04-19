> http://wiki.ros.org/pcl_ros

rosrun pcl_ros bag_to_pcd <input_file.bag> <topic> <output_directory>


[ROS/pcl/Tutorials](http://wiki.ros.org/pcl/Tutorials)

---


## 2. 활용

예제

```python
import pcl
import numpy as np
p = pcl.PointCloud(np.array([[1, 2, 3], [3, 4, 5]], dtype=np.float32))
seg = p.make_segmenter()
seg.set_model_type(pcl.SACMODEL_PLANE)
seg.set_method_type(pcl.SAC_RANSAC)
indices, model = seg.segment()
```


# 시각화 툴 

## PCD 시각화 (Jupyter + Potree)
- [point cloud visualization with jupyter/pcl-python/and potree](https://www.youtube.com/watch?v=s2IvpYvB7Ew) : Youtube
- 참고 : [The PCD (Point Cloud Data) file format](http://pointclouds.org/documentation/tutorials/pcd_file_format.php)

## pcap 시각화 (veloview)
- [GUI 툴 다운로드](https://www.paraview.org/veloview/#vvusers): Windows, Mac, Linux

## Rosbag 시각화 (Rviz)

- [Didi challenge 코드 활용](https://github.com/jokla/didi_challenge_ros)


[PCD File Format](http://www.jeffdelmerico.com/wp-content/uploads/2014/03/pcl_tutorial.pdf): slide 12