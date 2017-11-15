## 2. The operations of main functionalities 

> [youtube](https://www.youtube.com/watch?v=NDNcy0C-Has)

### 2.1 replay of demo data
- Replay the rosbag demo data downloaded from Nagoya University.
    1. Setup of demo data
    2. Launch Autoware
    3. Prepare to play rosbag
    4. Localization
    5. Mission planning
    6. Motion following
    

#### A. Setup of demo data


- 데이터셋 준비 

```
cd ~
mkdir .autoware
cd ~/.autoware


# A script for generating a demo launch file:
wget http://db3.ertl.jp/autoware/sample_data/my_launch.sh

#Map/calibration/path data (Moriyama area, 175MB):
wget http://db3.ertl.jp/autoware/sample_data/sample_moriyama_data.tar.gz

#rosbag data (3GB):
wget http://db3.ertl.jp/autoware/sample_data/sample_moriyama_150324.tar.gz
# Note that this ROSBAG data do not contain the video data. Therefore the object detection is not supported

# Unpack the demo data as follows.
tar xfz sample_moriyama_data.tar.gz
tar xfz sample_moriyama_150324.tar.gz
```

- Run my_launch.sh

```
# Run the following script to generate launch files.
sh my_launch.sh

# my_launch/폴더에 아래 파일들 생성 확인
my_map.launch		　　　　　# Load PointClouds and vector maps
my_sensing.launch	　        # Load device drivers
my_localization.launch	　        # Localozation
my_detection.launch	　        # Object detection
my_mission_planning.launch	　# Path planning
my_motion_planning.launch	　# Path following


```

#### B. Launch Autoware

```
/src/Autoware/ros
bash ./run
```

### 2.2 Autoware on a real vehicle
Drive a real vehicle with Autoware autonomously.





### 2.3 rosbag replay
Replay the data corrected on the procedure on left.

