# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I defined the kernel size and applied the Gaussian smoothing. Afterwards I computed the canny edge with a 1:3 proportion. Further on I defined a four sided polygon in order to create and apply a mask and in the end I twicked the Hough transform parameters.
In order to draw a single line on the left and right lanes, I modified the draw_lines() function by averaging the slope and the endpoints of detected line segments within the image and then computed the 'b' element from the Hough space formula (b = y - m * x). Following this I arranged the points together for constructing the current lane endpoints and appended the values to the list container.
In order to achieve smother transition between the video frames I calculated the average for the previous 5 frames lane endpoints. Afterwards I proceed forward with the lines drawing if the current lane endpoints are within 0.95 and 1.05 of the old values average. Otherwise I use the old values average and pop the current values from the list container. Also I skip this procedure within the first 5 frames of each video. In addition I protected the container by appending the current values before computing the average so I don't mess up the average if for some reason the values are completely offset, also being able to pop it from the list if this is the case.

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when there is a sharp bend radius and the average of the previous lines is  
significantly offset with respect to the current values and the frame rate is not high enough to cover the 5 frames without a large offset in-between. However if I reduce the number of previous frames taken into account, I will end up with a rougher transition between the frames.   

Another shortcoming could be the canny edge/hough transform processing when the road tarmac has significant color changes or light variations. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be deep learning:)

Another potential improvement could be further tiwcks to get rid of less likely scenarios which includes certain slope values, chances of having vertical lines or completly ignore lines which are far from the median.
