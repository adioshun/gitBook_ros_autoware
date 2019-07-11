# [Apollo Perceptual Analysis Tracking Object Information Fusion](https://mp.weixin.qq.com/s?__biz=MzI1NjkxOTMyNQ==&mid=2247485916&idx=1&sn=5510b30bde81a10c0271e526151a8cac&scene=19#wechat_redirect)



아폴로 퓨전 모델은 두가지 구성 요소로 이루어져 있다. 관성 네비게션 + 칼만 필터 `In the fusion framework of the Apollo multi-sensor fusion positioning module,Including two parts: inertial navigation solution, Kalman filter;`

The results of the fusion positioning will in turn be used for the prediction of GNSS positioning and point cloud positioning;

퓨전 위치의 결과물은 다음과 같다. `The output of the fusion positioning is a 6-dof position and pose, and a covariance matrix.`
- 6DOF Posotion 
-