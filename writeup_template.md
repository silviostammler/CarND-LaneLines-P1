# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:

* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images/solidWhiteRight.jpg "Original image"

[image2]: ./test_images_output/solidWhiteRight_grayscale.jpg "Grayscale"

[image3]: ./test_images_output/solidWhiteRight_blur_gray.jpg "Blurred image"

[image4]: ./test_images_output/solidWhiteRight_canny_edge.jpg "Canny edge detected image"

[image5]: ./test_images_output/solidWhiteRight_masked_edges.jpg "Masked image"

[image6]: ./test_images_output/solidWhiteRight_laneLines.jpg "LaneLines image"

[video1]: ./test_videos_output/solidWhiteRight.mp4 "Video with annotated lane lines"

[video2]: ./test_videos_output/solidYellowLeft.mp4 "Video with annotated lane lines"
---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. All intermediate images within this process will be visualized. As an example the original image "solidWhiteRight.jpg" is used. 

![alt text][image1]


First, I converted the images to grayscale. The output looks like follows:

![alt text][image2]


Then I applied Gaussian blurring to smooth the grayscale image. The output looks like follows:

![alt text][image3]


After that the Canny edge detection algorithm is applied to the blurred image. The following output is generated:

![alt text][image4]


The fourth step consists of region masking to get rid of unimportant information in the images (only consider area including lane lines). The resulting binary image is subsequently shown:

![alt text][image5]


Finally, the last step is to apply Hough transform which produces line segments of the lane lines. To extrapolate these line segments to full lane lines (red) the function draw_lines() is utilized. It will be explicitly described later on. After applying weighted_image() the outcome of this last step which is also the final output is a drawing of the left and right lane lines on the original images. It looks like follows:

![alt text][image6]



In order to draw a single line on the left and right lanes, the draw_lines() function had to be modified. Depending on the slope of each line segment ($m<0$ respectively $m>0$) found by the Hough transform the x,y coordinates are assigned to left lane respectively right lane. Then the full lane lines are obtained by solving the least squares problem mx+b. Given the lower and upper bound (y coordinate) of the region of interest and the parameters m and b it is possible to calculate the intersecting x coordinates of the lane lines with the region of interest. Finally, the full lane lines can be drawn onto the image.


Based on the pipeline described above the lane lines are also detected for the video stream.
The resulting video of "solidWhiteRight.mp4" looks like follows:

![alt text][video1]


On the other hand the resulting video of "solidYellowLeft.mp4" is subsequently shown:

![alt text][video2]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming could be difficult detection of lane lines in a video stream if the speed of the car is high and it goes along a strong curve as it is the case for the challenging video.

Another shortcoming could be shadows that hide lane lines, thus making the detection much more inaccurate.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to adapt the region of interest depending on the speed of the car and the curvature radius.

Another potential improvement could be a perspective transform applied to the image to get a birds eye of view and then fit a polynomial to the lane lines. At the end the detected lane lines could be drawn back onto the original image. Especially for detecting lane lines in curves this approach should be much more accurate.
