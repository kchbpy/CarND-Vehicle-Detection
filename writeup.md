
**Vehicle Detection Project**



[//]: # (Image References)
[image1]: ./output_images/color.png
[image2]: ./output_images/hog.png
[image3]: ./output_images/s1.png
[image4]: ./output_images/s2.png
[image5]: ./output_images/e.png

[video1]: ./project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
## Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

## Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the code cell 1 through 5 code cell of the IPython notebook named `mypip1.ipynb` 

I started by reading in all the `vehicle` and `non-vehicle` images. 

I then explored different color spaces looks like.

![alt text][image1]

I then explored different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Here is an example using the `YCrCb` color space and HOG parameters of `orientations=12`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:

![image2]

#### 2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters and find that more HOG features means more information,but the more features I get the more memory I need. When I set `pixels_per_cell=(4,4)` the features run out of my computer's memory. So finally I set `orientations=12`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`.

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using color features and HOG features.
I only used HOG features in my first training, but the score of my classifier is less than 0.9. I tried a lot of combinations of the features. But none of them can train a good classifier. So at last I used the features combination just like the lesson used (YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector).

## Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

The code for this step is contained in the code cell 3 of the IPython notebook named `mypip2.ipynb`.I sliding the window in different regions depend on the scale. If the scale is less than 1.5 I search in the small region, if greater than 1.5 I search in the large region.
I measured the car's size, and compared withe 64*64 to get the suitable scale.

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched on 4 scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result. I also used the `lable` method in single image. Here are some example images:

![alt text][image3]![image4]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./out_video.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a series of frames of video (top left), the heatmap after using threshold (top right), the result of `scipy.ndimage.measurements.label()` (bottom left) and the bounding boxes then overlaid on the last frame of video(bottom right):

![image5]



---

## Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?
 
If I were going to pursue this project further I may use the CNN mathod rather than SVM, I thing the CNN can provide a more accurate classifier.

