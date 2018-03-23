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

My pipeline consisted of the following steps. 
* Convert to gray scale
* Apply Gaussian Blur
* Apply Canny Edge Detection
* Select region of interest
* Use Hough Transform to draw lines on the image
* Overlay the edge detected image to the original image

**In order to draw a single line on the left and right lanes, I modified the draw_lines() function as follows:

* Identified the slope of every line and separated them to positive and negative slopes. This will separate the lines to left and right hand side lanes.

* Filtered out the lines where the slope was in the range [-0.3, 0.3] to avoid any horizontal lines taking part in the lane finding logic. These horizontal lines can appear due to different factors like any objects on the road, shadow etc.

* The rest of the line segments in each of the sides were averaged out to polyfit function was used to identify the slope and intercept of both the lanes.

* The y co-ordinates of both the annotated lanes start from the y_max position(the maximum length of the image). We need to find the x co-ordinates of the annotated lines. We do this by using the formula 
	y = mx + c 
which is rearranged to get x using
	x = (y - c)/m

* Below formula is used to find the start and end x co-ordinates for the lane with positive slope
    pos_startX = (startY - pos_intercept) / pos_slope
    pos_endX = (endY - pos_intercept) / pos_slope
	
* Similarly, below formula is used for the lane with negative slope
    neg_startX = (startY - neg_intercept) / neg_slope
    neg_endX = (endY - neg_intercept) / neg_slope
	

### 2. Potential shortcomings with the current pipeline


One potential shortcoming would be to handle curved lanes and lanes with shadows. I have handled the situation where there could be objects in horizontal position on the road. But if there are other objects which are not in horizontal position, it will impact the logic used to find the lane co-ordinates. 


### 3. Possible improvements to the pipeline

Need to apply further filtering logic to handle situations described in the above mentioned shortcomings. If the x and y co-ordinates from the Hough lines are away from the fit line, they should be filtered out during the polyfit line construction.
