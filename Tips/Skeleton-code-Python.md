```python 

#!/usr/bin/env python3
# coding: utf-8

from __future__ import print_function
#import warnings; warnings.simplefilter('ignore')
import os
import sys
sys.path.append("/workspace/include")

import rospy
from sensor_msgs.msg import PointCloud2
from std_msgs.msg import Float32MultiArray
import sensor_msgs.point_cloud2 as pc2

import numpy as np
import pcl
import pcl_msg


import pcl_helper, filter_helper, bg_helper, markers_helper

import message_filters #read multiple msg

#kalman
from kalmanfilter import KalmanFilter
from kalmandatapoint import DataPoint
from fusionekf import FusionEKF
from kalmantools import polar_to_cartesian



from clustering import dbscan as cluster
#from clustering import rbnn_snu as cluster
#from clustering import optics as cluster

from centroidtracker import CentroidTracker


import ConfigParser
config = ConfigParser.RawConfigParser()
config.read('config.cfg')
baselinefile = config.get('bg_removal', 'baseline_file')

def people_tracker(data, labels): 

    n_clusters = len(set(labels)) - (1 if -1 in labels else 0)
    people_table = np.zeros((n_clusters,5),float) # [x1,y1,x2,y2,confi]
    location_table = np.zeros((n_clusters,4),float) # [x,y,w,h]  

    if n_clusters == 0:
        return people_table, location_table 

    concate = np.column_stack([data, labels])  #데이터 + 라벨 데이터셋 생성
    input_arr = concate[concate[:,3] != -1]  #outlier 제거

    # label별로 배열 생성 
    for GROUP_ID in range(0,n_clusters):
        globals()['label_{}'.format(int(GROUP_ID))] = input_arr[input_arr[:,3] == int(GROUP_ID)]

        exec('x1 = np.min(label_{}[:,0:1])'.format(GROUP_ID))
        exec('y1 = np.max(label_{}[:,1:2])'.format(GROUP_ID))
        exec('x2 = np.max(label_{}[:,0:1])'.format(GROUP_ID))
        exec('y2 = np.min(label_{}[:,1:2])'.format(GROUP_ID))  

        # without Group ID        
        exec('people_table[{0},0]={1:6.2f}'.format(int(GROUP_ID),x1)) 
        exec('people_table[{0},1]={1:6.2f}'.format(int(GROUP_ID),y1+0.5)) #박스 크기 확장용 
        exec('people_table[{0},2]={1:6.2f}'.format(int(GROUP_ID),x2+0.5)) #박스 크기 확장용 
        exec('people_table[{0},3]={1:6.2f}'.format(int(GROUP_ID),y2))
        exec('people_table[{0},4]=1.0'.format(int(GROUP_ID)))

        exec('location_table[{0},0]={1:6.2f}'.format(int(GROUP_ID),x1+(x2-x1)/2.))#(x1+x2)/2)) #x
        exec('location_table[{0},1]={1:6.2f}'.format(int(GROUP_ID),y1+(y2-y1)/2.))#(y1+y2)/2)) #y
        exec('location_table[{0},2]={1:6.2f}'.format(int(GROUP_ID),x2-x1))#x2-x1)) #w
        exec('location_table[{0},3]={1:6.2f}'.format(int(GROUP_ID),y2-y1))#y1-y2)) #h

    return people_table, location_table



class SensorFusion:
    def __init__(self):
        self.sub_lidar = rospy.Subscriber('/velodyne_points', PointCloud2, self.lidar_callback)
        #self.sub_radar = rospy.Subscriber('/radar_track', PointCloud2, self.radar_callback)
        self.sub_radar = rospy.Subscriber("/radar_track", Float32MultiArray, self.radar_callback)

        self.pub_radar = rospy.Publisher("/radar_track", PointCloud2, queue_size=1)
        self.pub_lidar = rospy.Publisher("/lidar_track", PointCloud2, queue_size=1)
        self.pub_fusion = rospy.Publisher("/fusion_track", PointCloud2, queue_size=1)


        self.all_sensor_data = []
        self.all_state_estimations = []   


        lidar_R = np.matrix([[0.01, 0], 
                            [0, 0.01]])

        radar_R = np.matrix([[0.01, 0, 0], 
                            [0, 1.0e-6, 0], 
                            [0, 0, 0.01]])

        lidar_H = np.matrix([[1, 0, 0, 0],
                            [0, 1, 0, 0]])

        P = np.matrix([[1, 0, 0, 0],
                    [0, 1, 0, 0],
                    [0, 0, 1000, 0], 
                    [0, 0, 0, 1000]])

        Q = np.matrix(np.zeros([4, 4]))
        F = np.matrix(np.eye(4))

        d = {
        'number_of_states': 4, 
        'initial_process_matrix': P,
        'radar_covariance_matrix': radar_R,
        'lidar_covariance_matrix': lidar_R, 
        'lidar_transition_matrix': lidar_H,
        'inital_state_transition_matrix': F,
        'initial_noise_matrix': Q, 
        'acceleration_noise_x': 5, 
        'acceleration_noise_y': 5
        }
        self.EKF = FusionEKF(d)
    



    def lidar_callback(self, input_lidar):
        
        
        pcl_xyzrgb = pcl_helper.ros_to_pcl(input_lidar) #ROS 메시지를 PCL로 변경

        #배경 제거 
        #pcl_xyzrgb = bg_helper.background_removal(pcl_xyzrgb)  
 

        #ROS Filter
        pcl_xyzrgb = filter_helper.do_passthrough(pcl_xyzrgb, 'x', -2.0,2)
        pcl_xyzrgb = filter_helper.do_passthrough(pcl_xyzrgb, 'y', -0.5, 2.5)
        
        #publish 
        bg_ros_msg = pcl_helper.pcl_to_ros(pcl_xyzrgb) #PCL을 ROS 메시지로 변경 
        pub_bg = rospy.Publisher("/velodyne_bg", PointCloud2, queue_size=1)
        pub_bg.publish(bg_ros_msg)

        
        #클러스터링
        pcl_xyz = pcl_helper.XYZRGB_to_XYZ(pcl_xyzrgb)
        data = np.asarray(pcl_xyz)   
        data, labels = cluster(data)
        people_table, location_table = people_tracker(data, labels)    
       
        objects = ct.update(people_table[:,0:4])
        for (objectID, centroid) in objects.items():
            markers_helper.marker_bbox(centroid[0], centroid[1], 0.1, 0.3, 0.3, 0.1) #박스 크기 고정
            markers_helper.marker_text(centroid[0], centroid[1]-0.5, 0.0, 0.2, 0.2, 0.2,"ID : {}".format(objectID))
        
        #timestamp = input_lidar.header.stamp.to_nsec()/int(1000)
        timestamp = rospy.get_time()#추후 메시지 수신 시간으로 변경 필요 
        
        if centroid.size != 0:

            sensor_data = DataPoint({ 
            'timestamp': int(timestamp),
            'name': 'lidar',
            'x': float(centroid[0]), 
            'y': float(centroid[1])
            }) 

            self.all_sensor_data.append(sensor_data)
       

        self.fusion()
        

    def radar_callback(self, input_radar):     
        #input_radar = [timestmap, Tid, rho,phi,drho]    
        sensor_data = DataPoint({ 
          'timestamp': int(input_radar.data[0]),
          'name': 'radar',  
          'rho': input_radar.data[2], 
          'phi': input_radar.data[3],
          'drho': input_radar.data[4]
        }) 
        self.all_sensor_data.append(sensor_data)




    def fusion(self):
                     
        data = self.all_sensor_data.pop()
        
        self.EKF.process(data)
        x = self.f.get()
        px, py, vx, vy = x[0, 0], x[1, 0], x[2, 0], x[3, 0]

        state_estimation = DataPoint({
            'timestamp': data.get_timestamp(),
            'name': 'state',
            'x': px,
            'y': py,
            'vx': vx,
            'vy': vy 
        })  
        self.all_state_estimations.append(state_estimation)

        target_data = np.zeros([1,3],dtype=np.float32)
        datas = np.array([px,py,0])
        target_data[0,]=datas

        pc = pcl.PointCloud(target_data)
        pcl_xyzrgb = pcl_helper.XYZ_to_XYZRGB(pc, [255,255,255])  
        out_ros_msg = pcl_helper.pcl_to_ros(pcl_xyzrgb)
        self.pub_fusion.publish(out_ros_msg)




 



        
        



if __name__ == '__main__':

    rospy.init_node('People_detection', anonymous=True)


    ct = CentroidTracker()
    sf = SensorFusion()
    
    rospy.spin()
```