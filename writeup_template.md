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

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I used gaussian blur function to blur the gray images. Next step is to use Canny edge detection to look for strong gradient. Then a region of interest is selcted to apply an image mask. Lastly, I used Hough transform to detect lane lines only and the results are plotted on top of the original images. In the cases of the video tests, lane line detection function is run on the sequence of images and each result is plotted while video is playing.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by seperating line segments by their slope to left and right. those in the left would have a positive slope and those in the right would have a negative slope. To plot a single line, an average value of all left lane lines is calculated. However some of these slope values were deemed to be unrealistic, possibly caused by other objects or noise in the original images. After a bit trial and error tests, the threshold value is set between 0.51 and 0.69. Similarly for the right lanes, a range of -0.86 to -0.62 is deemed acceptable values. These values were first calculated using an average of all left or right line segmnets from the test example images, then fine tuned to better fit the video images. To plot a single best fit line, two points was needed and their y-coordinates were set at the bottom of the image (y = img.shape[0]) and somewhere near the middle (y = img.shape[0]/2 + 70). x-coordinates were then caluclated from the slope and intercept average values from each group's line segments.

### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when lane lines aren't straight, similar to the video shown in the challenge video. Current best fit line method works well when lanes were straight and right in front of the car.

Another shortcoming could be when there are other objects in the frame (area of interests). This could be part of car in the adjacent lane, or something near the road sholder. The edges from these objects could be picked and identified as lane lines incorrectly.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to better identify curved lane lines, similar to those shown in the challenge video. Current process only works well on straight or nearly straight lane lines. A best fit curve method should be developed to work better around corners.

Another potential improvement could be to better filter and mask out other objects in the area of interests. This could be light shadow, double yellow lines, other cars in the frame or even dirty road. In current pipeline if these objects reside in the region and have a distinct edge, it will be counted towards a lane line. Perhaps colour selection method could help a bit in this case.
