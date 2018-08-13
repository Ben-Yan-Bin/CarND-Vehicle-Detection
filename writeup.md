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
[image1]: ./output_6_1.png
[image2]: ./output_11_1.png
[image3]: ./output_14_1.png
[image4]: ./output_14_2.png
[image5]: ./output_14_3.png
[image6]: ./output_14_4.png
[image7]: ./output_14_5.png
[image8]: ./output_14_6.png

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the cell 2 to 7. In cell 2, I defined a function to help extract hog features from list of images, cv2.HOGDescriptor is used. to get hog features. In cell 3, I read all images of cars and not cars. In cell 5, the hog features of all the data was extracted, and consumed 27 seconds. 

Below is an example car image, with the raw features and scaled features.

![alt text][image1]


#### 2. Explain how you settled on your final choice of HOG parameters.

I used the same parameters shown in the course, and it is working fine. I also use "YUV" as my color space. When I use "RGB" as color space, it will cause more false positives on the advanced lane line finding videos (the lane lines are painted green).

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I tried SVM and Mutiple-Layer-Perceptron, and found MLP works better. (MLP=0.99 acc, and SVM=0.95)

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I defined the slide window function in cell 10, start with the window size 64, since from the image, I think size 64 is a reasonable size of cars that are far away enough, also I use 0.5 as the overlap which is a balance of performance and accuracy. 

![alt text][image2]

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Here are some example of the test images and the contours of the detection, and bounding boxes of the detections:   
I set Y start = 400, and Y stop = 700 to focus on the lower part of the image to have better performance.

![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]
![alt text][image7]
![alt text][image8]

---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./test1_ouput.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:


---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

One issue in the project is to setup good shape of HOG parameters and slide window sizes. And how to balance the performance and accurary is also a challenge. Some experiments are needed to find proper parameters.   
Second issue is how to deal with false positives, in which I referred to some resources from the internet, and also the usages of cv2 functions, like cv2 contour, boundingRect, groupRectangles.   
Another issue is the performance of the pipeline, it takes around 0.7 sec for one frame, which should be improved by better slide window design, or some other complete Deep Learning E2E solution like YOLO.
