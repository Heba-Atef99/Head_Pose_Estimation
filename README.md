# Head_Pose_Estimation
![GitHub top language](https://img.shields.io/github/languages/top/Heba-Atef99/Head_Pose_Estimation?color=%2300)

Our problem is estimating the head pose using the three angles: (pitch, yaw, roll). As our input is an image of the face and the output is an image with three axes defining its position.

<div align="center">
    <img src="https://github.com/Heba-Atef99/Head_Pose_Estimation/blob/main/images/1.png" width="350" height="350" >  
    <img src="https://github.com/Heba-Atef99/Head_Pose_Estimation/blob/main/images/2.png" width="350" height="350" >  
</div>  

# Dataset
We will use AFLW2000 dataset which is a dataset of 2000 images that have been annotated with image-level 68-point 3D facial landmarks.

You can refer to the dataset [here](http://www.cbsr.ia.ac.cn/users/xiangyuzhu/projects/3DDFA/main.htm)
<div align="center">
    <img src="https://github.com/Heba-Atef99/Head_Pose_Estimation/blob/main/images/dataset.jpg">  
</div>  

# Solution Pipleline
<div align="center">
    <img src="https://github.com/Heba-Atef99/Head_Pose_Estimation/blob/main/images/pipeline.png">  
</div>  

## 1. Extract Features
We will use MediaPipe Face Mesh solution to extract the 2D semantic contours of the face.

MediaPipe Face Mesh is a solution that estimates 478 3D face landmarks in real-time even on mobile devices. It employs machine learning (ML) to infer the 3D facial surface

You can learn more about mediapipe from [here](https://google.github.io/mediapipe/solutions/face_mesh.html)

## 2. Normalize The Landmarks
We bormalize the features by changing its center to be the nose and divide by the largest distance which is between the chin and the forehead.

This will help the features to be invariant with the translation of the face along the picture so that the model would be more robust.

We tried to train the model only using scaled features using min max scaler, in training and testing it performed pretty well. But when trying the model with a video of a face moving in different directions, it performed badly as it failed when the face was totaly upward or when it was tilted. So normalizing the features made a big difference

## 3. Predict With Multi-output SVR
After training multiple models such as (Random Forest, Ridge, Lasso, Decision Tree), SVR outperformed them all.
<div align="center">

|Metric         |Ridge|Lasso|Random Forest|SVR|
|---------------|:----|:----|:-------------|:-----------------|
|Accuracy       |0.43 |0.75 |0.63|0.84|
|Training RMSE  |0.38 |0.5  |0.29|0.34|
|Validation RMSE|0.25 |0.17 |0.21|0.13|
</div>

## 4. Draw Axis
Finally we draw three axis: Pitch, Yaw, and Roll to estimate the head position
