## Writeup Template
### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./result/img1.png
[video1]: ./project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

First cell of Jupyter notebook  

I use hog function from skimage to extract hog feature. I used gray image as input, orientation = 9, pixel per cell = 8, cell per block = 2.
Result image:
![alt text][image1]

#### 2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters after I finished the entire project. Based on the best performance I found, I selected orientation = 32, pixel per cell = 16, cell per block = 2, color space of input image was YCrCb, and use all color channels to extract the hog feature. Since I believe the color feature and spatial feature won't help the vehicle detection, so I would use hog feature only.

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using 8500 samples for both vehicle and non-vehicle. 80% for training, 20% for testing. The accuracy rate was 97%. Code was at cell 6 of jupyter notebook.

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I used 96 x 96 window size and 0.5 overlap for both x and y. Since this was the default setting of the course, and the performance was also pretty good, so I won't change this.
![alt text][image2]

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

I have tried many parameters with different scales and size of searching area, but the performance wasn't that great even the accuracy rate of the model was 97%. The detector have a lot of false positive and true negative. After search online, I the window searching method proposed by Jeremy(https://github.com/jeremy-shannon/CarND-Vehicle-Detection) could perform relatively good for searching the car in video. The comparing with his model, I randomly tried many parameters with no rational explanation, so the accuracy rate of the output video depends on how lucky I was. But Jeremy explained the advantages of each combination of parameters and what is the combination looking for, so the result was much better than some random attempts. 

![alt text][image3]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./output_video.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I used the heatmap method introduced by the lecture. Every frame of the video will go through the pipeline as a single image. The pipeline will search on 8 combination of scales within different area and then output the true prediction windows. I added all prediction windows together to create a heatmap and then filter out the points with less than 2. Finally I used the function scipy.ndimage.measurements.label to output the final windows and draw it to the image, store the images into a list and compress the list of images into a mp4 video.

### Here is a sample heatmap:

![alt text][image4]

### Here the resulting bounding boxes are drawn onto the last frame in the series:
![alt text][image5]



---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

