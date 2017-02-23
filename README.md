#**Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

**Overview**
---

Here, we tried creating the front eyes for the Autonomous Driver Assistance System. When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project you will detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  


#**Reflection**
---
##1. Describe the pipeline##
    Many steps to the pipeline (Thankfully our Eyes don't need steps to leave an accident zone).
###A. Find Edge pixels
    To improve capture and eliminate noise, we perform below sequence of image preprocessing activities 
#####1. Remove near dark pixels from image. These are masked to black. 
#####2. Do Gaussian blurring of image. We used kernel-size of 3. 
#####3. Detect edges by running Canny's routine on the grayscale image, and also each color channel. Combine the edge maps found. 
###B. Mask the edges to region of interest in front of the car. 
###C. Perform Hough transform and fetch lines in the edge pixels. 
###D. Combine the lines into left and right lanes
#####0. Take in the detected Houghlines in masked edges-map
#####1. Assign weights to lines based on their lengths 
#####2. Extrapolate the lines to boundaries of masked quadrilateral 
#####3. Compute weighted average slope, passing-pt of lines with slope>0 (& < 0) to form rightLane (& leftLane)

###2. Identify any shortcomings
    The final lanelines are very jerky, and in some frames outright incorrect. 
####A. detected edges pixels seem fine with respect to the input images.
    ###B. Mask seemed to be proper wrt some static input images, but it is giving many spurious edges at the top. 
    ###C. Detected Lane lines are not consistently accurate. 

###3. Suggest possible improvements
    A. Region of Interest Mask to be customized based on the Car and mount position of the camera.
    B. Mask size to err towards lower size, to eliminate noise from detected edges. The final output is less sensitive to missing of some edge pixels near mask boundary, than it is to random noise in the region of interest. 
    C. Parameters for Hough line detection to be calibrated. Most error has crept in here, and this is currently the weakest point of the pipeline. 
    D. While combining lines, computation of mean slope to be best performed in angles, instead of y/x slope. 
    E. To combine the line segments length of the lines are taken as weights. Evaluate other options like no of edge pixels, or combination of line-length and frequence for weights. 
    F. Eliminate NaN values from computation. Write computations in such a way, that no NaN comes. Eliminating NaN by replacing with a fixed value, can extremely skew results. 

