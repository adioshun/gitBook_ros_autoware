# ROS PCL 

설치(ubuntu 14) : [PCL for ROS](https://github.com/adioshun/System_Setup/wiki/7_PCL#3-pcl-for-ros), [[홈페이지]](http://wiki.ros.org/pcl_ros)


## 1. Tools 

### 1.1 bag_to_pcd

Velodyne Lidar로 생성한 bag파일을 PCD포맷으로 변경 :`rosrun pcl_ros bag_to_pcd <input_file.bag> /<topic> <output_directory>`
- `Reads a bag file, saving all ROS point cloud messages on a specified topic as PCD files.`

여러개의 [PCD포맷](http://pointclouds.org/documentation/tutorials/pcd_file_format.php) 파일들 생성 

### 1.2 convert_pcd_to_image

PCD를 이미지로 변경 : `rosrun pcl_ros convert_pcd_to_image <cloud.pcd>`
- `Read the point cloud in <cloud.pcd> and publish it in ROS image messages at 5Hz.`

파일 형태로 이미지를 저장하진 않고 **Topic**으로 출력 (확인필요)
- output (sensor_msgs/Image) : A stream of images generated from the PCD file.

> [에러] "no rgb field"로 실행 안됨 - 추후 해결필요 

###### [PCD시각화 방법] 

 `pcl-tools` 패키지 설치 

- $ `rosrun perception_pcl pcd_viewer <filename>`
- $ `pcd_viewer <filename>` 
- $ `pcl_viewer -multiview 1 <pcd_filepath>`

## 1.3 convert_pointcloud_to_image


## 1.4 pcd_to_pointcloud

## 1.5 pointcloud_to_pcd


> 좀더 자세한 내용 참고 : [ROS/pcl/Tutorials](http://wiki.ros.org/pcl/Tutorials)

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

[PCD File Format](http://www.jeffdelmerico.com/wp-content/uploads/2014/03/pcl_tutorial.pdf): slide 12

[ROS/pcl/Tutorials](http://wiki.ros.org/pcl/Tutorials)




