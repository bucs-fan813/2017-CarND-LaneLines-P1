**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps:
1) I converted the images to grayscale
2) process the grayscale image with a canny edge detection
3) Mask out the areas of the image that we are not concerned with
4) use gaussian blur to smooth out the canny image
5) perfom a hough transform to detect the lanes
6) overlay the hough tranform on original image


In order to draw a single line on the left and right lanes, I modified the draw_lines() function by:
1) splitting the array of lines into two distinct arrays
    a) lines with positive slopes are considered right hand lane lines
    b) lines with negative slopes are considered left hand lane lines (this seems backwards)
2) it is necessary to get ride of any horizontal or vertical lines since they may crash the program and lanes will never be perfectly vertical or horizontal
3) next make sure points for left lanes are on the left side of the image and points for right lanes are on the right side of the image. (this is not sound logic but for now, without this, edge cases throw of the slope of the line)
4) next convert each lanes array into a numpy matrix so that we can easily perform calculations
    a) take the appropriate max and min values of each (x,y) pair that makes up a line (I had to guess which was the correct combination because the coordinate system is flipped along the y axis
5) to draw a single line to represent each lane I took the (x,y) coordinates from the points at the lower, innermost and upper outtermost points of the step one's arrays

###2. Identify potential shortcomings with your current pipeline
My current pipline will not draw lanes all the way down to the bottom of the image. Also my pipline will not correctly detect lanes with hard turns or elevation changes. 1) the masked area goes from left to center and right to center and meet at about 60% of the height. These mask dimentions will work for this project but may not work for others. 2) the currect pipline relies on exceptionally good weather/picture quality to see the road lanes. This pipline will not work in offroad conditions.


###3. Suggest possible improvements to your pipeline

A possible improvement would be to detect objects and other vehicles. Also, detecting the types of lanes is very important so distinguishing solid vs dashed and yellow vs white lines. Also various turning arrows and crosswalk lines is good for urban driving.