# [About the analysis of high-precision map ROI filter](https://mp.weixin.qq.com/s?__biz=MzI1NjkxOTMyNQ==&mid=2247485710&idx=1&sn=07ba741effb95e10d40e175ba61cd3d1&chksm=ea1e1b7cdd69926a42ff1a809e4791810661f9a15a7c590b0924eb0a3728ba9a365622a3f068&mpshare=1&scene=23&srcid=0306ludMb9jq80wVdt7JKWxi#rd)


## 1. high-resolution map ROI filter

### 1.1 데이터 수신(ROS) & 변환(PCL)

- 가장 먼저 수행 된다. `The high-resolution map ROI filter is the first process of callback. `


- 배경, 도로변, 빌딩, 나무등을 제거 한다. `This process processes lidar points outside the ROI, removing background objects such as roadside buildings and trees. The remaining point clouds are reserved for subsequent processing. `

- 입력은 ROS의 포인트 클라우드 이다. `As can be seen from the initial module framework diagram, the input data type accepted by the LidarProcessSubnode subnode is ROS original point cloud data type, sensor_msgs::PointCloud2`

- 실제 작업은 PCL로 이루어 지므로 변환 작업이 필요 하다. `There is also a detail, the point cloud obtained by Lidar is ROS original sensor_msgs::PointClouds type , and the actual processing used more is the PCL library pcl::PointCloud type , you need to do a conversion in the code, use The pcl::fromROSMsg and pcl::toROSMsg functions of pcl_conversions can be easily converted to each other.` 

```python 
"""
[Sensor_msgs::PointCloud 와 Sensor_msgs::PointCloud2의 차이]

Sensor_msgs::PointCloud2 has some differences from the first version of sensor_msgs::PointCloud, supports arbitrary N-dimensional data, and supports arbitrary basic data types, as well as dense storage. 

As can be seen from the above description, PointCloud2 can support 2D data structure, each point N-dimensional, X lines per line, a total of H columns, can store image information. 

Fields store the names of the various channels, such as x, y, z, r, g, b, and so on. 
"""

Header header

# 2D structure of the point cloud. If the cloud is unordered, height is
# 1 and width is the length of the point cloud.
uint32 height
uint32 width

# Describes the channels and their layout in the binary data blob.
PointField[] fields

bool    is_bigendian # Is this data bigendian?
uint32  point_step   # Length of a point in bytes
uint32  row_step     # Length of a row in bytes
uint8[] data         # Actual point data, size is (row_step*height)

bool is_dense        # True if there are no invalid points
```

### 1.2 Data conversion and ROI generation



센서 정보 저장시 중요한 작업 두가지 `The two points worthy of attention in the code of the stored procedure are`
- the conversion matrix **velodyne_trans of the sensor** to **the world coordinate system** and 
- the conversion of `sensor_msgs::PointCloud2` to `PCL::PointCloud`


```cpp
/// file in apollo/modules/perception/obstacle/onboard/lidar_process_subnode.cc
void LidarProcessSubnode::OnPointCloud(const sensor_msgs::PointCloud2& message) {
  ...
  /// get velodyne2world transfrom
  std::shared_ptr<Matrix4d> velodyne_trans = std::make_shared<Matrix4d>();
  if (!GetVelodyneTrans(kTimeStamp, velodyne_trans.get())) {
    ...
  }
  out_sensor_objects->sensor2world_pose = *velodyne_trans;

  PointCloudPtr point_cloud(new PointCloud);
  TransPointCloudToPCL(message, &point_cloud);
}
```

다음 절차는 포인트 클라우드에서 ROI영역을 추출 하는 것이다. `the next step is to retrieve the ROI areas from the point cloud , which contain the driving areas of the road and intersection.`

```cpp
/// file in apollo/modules/perception/obstacle/onboard/lidar_process_subnode.cc
void LidarProcessSubnode::OnPointCloud(const sensor_msgs::PointCloud2& message) {
  /// get velodyne2world transfrom
  if (!GetVelodyneTrans(kTimeStamp, velodyne_trans.get())) {
  	...
  }
  /// call hdmap to get ROI
  HdmapStructPtr hdmap = nullptr;
  if (hdmap_input_) {
    PointD velodyne_pose = {0.0, 0.0, 0.0, 0};  // (0,0,0)
    Affine3d temp_trans(*velodyne_trans);
    PointD velodyne_pose_world = pcl::transformPoint(velodyne_pose, temp_trans);
    hdmap.reset(new HdmapStruct);
    hdmap_input_->GetROI(velodyne_pose_world, FLAGS_map_radius, &hdmap);
    PERF_BLOCK_END("lidar_get_roi_from_hdmap");
  }
}
```


도로를 파악하기 위해서는 고정밀 지도가 이용된다. `The driving area of ​​the road surface and intersection needs to be inspected by high-precision map. `

이 단계에서 At this stage, the coordinate system is transformed by tf (the transformation matrix of the lidar coordinate system to the world coordinate system), and the velodyne_pose_world is calculated with velodyne_pose (lidar is in the world coordinate system). Coordinates).

The real ROI is using the `GetROI` function . 


