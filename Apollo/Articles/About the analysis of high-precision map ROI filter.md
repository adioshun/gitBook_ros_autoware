# [About the analysis of high-precision map ROI filter](https://mp.weixin.qq.com/s?__biz=MzI1NjkxOTMyNQ==&mid=2247485710&idx=1&sn=07ba741effb95e10d40e175ba61cd3d1&chksm=ea1e1b7cdd69926a42ff1a809e4791810661f9a15a7c590b0924eb0a3728ba9a365622a3f068&mpshare=1&scene=23&srcid=0306ludMb9jq80wVdt7JKWxi#rd)


## 1. high-resolution map ROI filter

- 가장 먼저 수행 된다. `The high-resolution map ROI filter is the first process of callback. `


- 배경, 도로변, 빌딩, 나무등을 제거 한다. `This process processes lidar points outside the ROI, removing background objects such as roadside buildings and trees. `

The remaining point clouds are reserved for subsequent processing. 

- 입력은 ROS의 포인트 클라우드 이다. `As can be seen from the initial module framework diagram, the input data type accepted by the LidarProcessSubnode subnode is ROS original point cloud data type, sensor_msgs::PointCloud2`