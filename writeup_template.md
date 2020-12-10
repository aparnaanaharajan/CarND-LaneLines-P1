# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve.jpg "Lane detected Image1"
[image2]: ./test_images_output/solidWhiteRight.jpg "Lane detected Image2"
[image3]: ./test_images_output/solidYellowCurve.jpg "Lane detected Image3"
[image4]: ./test_images_output/solidYellowCurve2.jpg "Lane detected Image4"
[image5]: ./test_images_output/solidYellowLeft.jpg "Lane detected Image5"
[image6]: ./test_images_output/whiteCarLaneSwitch.jpg "Lane detected Image5"
---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I used following functions .... 

**gaussian_blur**: Apply a Gaussian blur to the grayscale image using cv2.GaussianBlur method.

**canny**: Use a Canny edge detection method to find edges on the blurred image using cv2.Canny method.
           
**region_of_interest**: Apply region of interest mask to edge detected image from **canny** function using trapezoid with defined vertices.


**houghLines**: Use a Hough transformation to find the lines on the masked image using cv2.cv2.HoughLinesP. It also adjusts lines from the Hough transformation in order to have a clearer-two-line lane representation of the road lines, which is defined in **draw_lines** function.
   **draw_lines**: First step in draw lines is to separate left and right lane line segments using slope and the filter value.
                   Second step is to apply average to the points from each lane line segments.
                   Third step is to detect the outliers from averaged line segments of left and right lane using **detectOutlier** function.
                   Next step is to fit line to both averaged line segments (no outliers) of left and right lane using np.polyfit function.
                   Final step is define min and max points of x and predict y using slope and intercept returned by polyfit and draw lines.
                   
   **detectOutlier**: Eliminate parts of the image that are not interesting for lane detection, by using standard deviation of error between actual points and fitted line (using polyfit function)


**weighted_img**: Merges the output of **houghLines** with the original image to represent the lines on the image

After processing image using above pipeline, the lane detected images look like this:

![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]


### 2. Identify potential shortcomings with your current pipeline

It would be better to make the pipeline work for curve driving cases when ego car is turning on the road. While changing lanes, the pipeline also needs to be adapted by automating the region of interest mask so that lanes still go detected independent of scenario. The unwanted points around the lane are masked by outlier model and hence for worst cases - where road contains multiple markings around lane (which is not interesting for us), example videos are still needed to verify the efficiency of the current pipeline.


### 3. Suggest possible improvements to your pipeline

Possible improvements are to define a GUI tool to find the region of interest mask for lane changing scenarios and to use polyfit with a degree of 2 polynomial that would help for curved driving scenarios. 
