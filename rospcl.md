# ROS PCL

설치\(ubuntu 14\) : [PCL for ROS](https://github.com/adioshun/System_Setup/wiki/7_PCL#3-pcl-for-ros), [\[홈페이지\]](http://wiki.ros.org/pcl_ros)

## 1. Tools

### 1.1 bag\_to\_pcd

Velodyne Lidar로 생성한 bag파일을 PCD포맷으로 변경 :`rosrun pcl_ros bag_to_pcd <input_file.bag> /<topic> <output_directory>`

* `Reads a bag file, saving all ROS point cloud messages on a specified topic as PCD files.`

여러개의 [PCD포맷](http://pointclouds.org/documentation/tutorials/pcd_file_format.php) 파일들 생성

### 1.2 convert\_pcd\_to\_image

PCD를 이미지로 변경 : `rosrun pcl_ros convert_pcd_to_image <cloud.pcd>`

* `Read the point cloud in <cloud.pcd> and publish it in ROS image messages at 5Hz.`

파일 형태로 이미지를 저장하진 않고 **Topic**으로 출력 \(확인필요\)

* output \(sensor\_msgs/Image\) : A stream of images generated from the PCD file.

> \[에러\] "no rgb field"로 실행 안됨 - 추후 해결필요

###### \[PCD시각화 방법\]

`pcl-tools` 패키지 설치

* $ `rosrun perception_pcl pcd_viewer <filename>`
* $ `pcd_viewer <filename>` 
* $ `pcl_viewer -multiview 1 <pcd_filepath>`

## 1.3 convert\_pointcloud\_to\_image

## 1.4 pcd\_to\_pointcloud

## 1.5 pointcloud\_to\_pcd

> 좀더 자세한 내용 참고 : [ROS/pcl/Tutorials](http://wiki.ros.org/pcl/Tutorials)

---

---

## GitBook

## \[실습\] 3D Pointcloud based

* [Point\_Cloud\_Data](https://adioshun.gitbooks.io/deep_drive/content/pointcloud-data.html)
  * [ROS bags-TO-Image.ipynb](https://gist.github.com/anonymous/4857f8920c9fc901121a429ead32a7db)
  * [ROS bags-TO-Point Clods.ipynb](https://gist.github.com/anonymous/e675ea14113252be321320be62248034)
  * [ROS bags-TO-Avi.ipynb](https://gist.github.com/anonymous/fb1e98efe187b2a35b6d91fb5df9e83b)
  * [KITTI Dataset Exploration.ipynb](https://github.com/hunjung-lim/awesome-vehicle-datasets/blob/master/vehicle/kitti/KITTI%2BDataset%2BExploration.ipynb)
  * [KITTI Dataset Visualizing.ipynb](https://github.com/hunjung-lim/awesome-vehicle-datasets/blob/master/vehicle/kitti/KITTI%2BDataset%2BVisualizing.ipynb)
  * [Read\_RGBD](https://adioshun.gitbooks.io/deep_drive/content/pointcloud-data/readrgbd.html)
  * [Velodyne\_LiDAR](https://adioshun.gitbooks.io/deep_drive/content/pointcloud-data/velodynelidar.html)



