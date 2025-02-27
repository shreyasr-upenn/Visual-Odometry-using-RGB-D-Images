# Visual-Odometry-using-RGB-D-Images
In this project, we aim to accurately estimate the pose of a moving camera at every time step and subsequently its trajectory, from RGB-D images only.

## Members
1. Shreyas Ramesh
2. Sharon Richu Shaji
3. Manasa Sathyan
## Overview
In this project, we implement a visual odometry and mapping system using RGB-D images inspired by the paper on *"Fast visual odometry and mapping from RGB-D data"*. We estimate the 6-DoF trajectory of a moving camera by aligning sparse features obtained from RGB-D images against a persistent model set consisting previous features. A Kalman Filter is used to dynamically update the model set by incorporating new features obtained from each RGB-D frame. A Gaussian mixture model is implemented to handle the uncertainty in the sparse RGB-D image features. The results of this project show that our implementation estimates the pose of the camera fairly well and is also able to close small-scale loops in indoor environments.

 <p align="middle">
   <img src="Results/pipeline.png" width=820 height=370 />
 </p>


## Dataset
The dataset used is the [RGB-D SLAM Dataset](https://vision.in.tum.de/data/datasets/rgbd-dataset/download#freiburg1_room) from TU Munich’s Computer Vision Group. The dataset contains color and depth images at multiple time steps captured by a Microsoft Kinect sensor along with the ground-truth trajectory of the sensor. There exists multiple series of datasets covering different environments such as office spaces and day-to-day objects. We used the *"freiburg1_room"* sequence dataset for this project. 

Along with the RGB and depth images, the corresponding rgb, depth, and ground-truth .txt files contain the timestamps for each image frame. Due to the lack of an inherent 1:1 correspondence between the data, the associated RGB, depth and, ground truth data needs to be found. 

Finally, the dataset also provides the camera intrinsic parameters for calibration - generate 3D coordinates in the camera coordinate frame.

## Procedure
  * Run the Visual_Odometry.ipynb notebook with the appropriate path to the downloaded TUM RGB-D dataset.
  * In \__main__\, select the feature extraction algorithm 
  
    ```
    # select feature detector
    feature_detector = "GFTT"       #"GFTT" or "SIFT" or "ORB"
    ```
  * Create the appropriate folders to save the resulting trajectory and plots. 

## Experimental Results 
### Comparison with Ground Truth 
We analysed the performance of our implementation by primarily plotting and calculating the error between the estimate and the ground-truth. This is a standard way of measuring performance which is the most intuitive. 
  1. <ins>__Overall Trajectory in 3D:__</ins> The two plots below show the complete trajectory of the Kinect camera that moves around a room.

  <p align="middle">
    <img src="Results/est_full_traj.png" width=290 height=270 hspace="20"/>
    <img src="Results/gt_full_traj.png" width=290 height=270 /> 
  </p>


  2. <ins>__Effect of Initial Transformation:__</ins> After performing thorough experimentation with different initial transformation values, we realised that the ICP algorithm heavily depends on sufficient overlap and initial alignment of two point clouds. If the initial pose of the camera in the fixed frame (initial transformation) is very different from the actual pose in the fixed frame, then ICP finds it very hard to converge leading to bad pose estimations. Therefore, finding a good initial transformation is crucial for this algorithm to give good estimations. The graphs below show the difference in estimations when two different initial transformations were given.

  <p align="middle">
    <img src="Results/Yaw_wrong.png" width=320 height=250 hspace="20"/>
    <img src="Results/Yaw_gftt.png" width=320 height=250 /> 
  </p>
  

  3. <ins>__Absolute Trajectory Error (ATE):__</ins> The Absolute Trajectory Error (ATE) measures the error between the estimated trajectory of the camera and the ground-truth trajectory at different timesteps. The below plots and table show the results obtained. Finding a good initial estimate of transformation should result in better trajectory estimations. 

  <p align="middle">
    <img src="Results/Tx_gftt.png" width=320 height=250 />
    <img src="Results/Ty_gftt.png" width=320 height=250 />
    <img src="Results/Tz_gftt.png" width=320 height=250 /> 
  </p> 
  
  <div align="center"> 
 
  | Performance Metric           | Value (in meters)       |
  |:----------------------------:|:-----------------------:| 
  | Root Mean Square Error       | 1.547102621353284       | 
  | Mean Error                   | 1.4696188668223706      |   
  | Median of Error              | 1.4703254181759537      |  
  | Standard Deviation of Error  | 0.4834737917177455      |   
  | Minimum Error                | 0.46653817360354705     |  
  | Maximum Error                | 2.3009297495923917      |
 
  </div>
  

  4. <ins>__Error in Orientation:__</ins> To measure the error in orientation we plotted the estimated Roll, Pitch, and Yaw against the ground-truth as shown below. Finding a good initial estimate of transformation should result in better orientation estimations.
  
  <p align="middle">
    <img src="Results/Roll_gftt.png" width=320 height=250 />
    <img src="Results/Pitch_gftt.png" width=320 height=250 />
    <img src="Results/Yaw_gftt.png" width=320 height=250 /> 
  </p> 



### Performance of different Feature Detectors
Here we tested our algorithm with different feature detectors, namely, Shi-Tomasi (baseline), SIFT, and ORB. The performance was tested based on computational speed. The below table summarises the time taken for the execution of the algorithm with the different feature detectors. We see that the Shi-Tomasi feature detector is faster and leads to lesser overall computational time as compared to the other two. 

  <div align="center">
  
  | Feature Detector           | Computation Time (in hours)      |
  |:----------------------------:|:-----------------------:| 
  | Shi-Tomasi (baseline)      | 0.64117       | 
  | SIFT     |0.77934       | 
  | ORB     | 0.71473      | 
  
  </div>
  
  

