# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/solidWhiteRight.jpg "Original"
[image2]: ./examples/solidWhiteRightGrayscale.jpg "Grayscale"
[image3]: ./examples/solidWhiteRightBlured.jpg "Blured"
[image4]: ./examples/solidWhiteRightCanny.jpg "Canny"
[image5]: ./examples/solidWhiteRightMasked.jpg "Masked"
[image6]: ./examples/solidWhiteRightHough.jpg "Hough"
[image7]: ./examples/solidWhiteRightDraw.jpg "Draw"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps as follows:

![Original Image][image1]

1. Convert the image to grayscale

![Grayscale Image][image2]

2. Apply a Gaussian kernel to remove noise

![Blured Image][image3]

3. Apply the Canny transform to detect edges

![Canny Edges][image4]

4. Apply a triangle image mask to get the masked edges

![Masked Edges][image5]

5. Utilize the Hough Transform to detect straight lines

![Hough Lines][image6]

6. Drawn the Hough lines on the original image

![Overlaid Image][image7]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by:

1. Seperate line segments by their slope ((y2-y1)/(x2-x1)) to decide which segments are part of the left line (slope < 0) vs. the right line (slope > 0).
2. Remove horizontal line segments by setting a threshold on the slope. Lines with slopes below the threshold are seen as noise and removed.
3. Calculate the slopes and intercept of the extrapolated left and right lane lines by using the median of the slopes and intercepts of the line segments each side.
4. Obtain the two lower end pixel coordinates of the lane lines at the image bottom with the above slope and intercept each side, and the two upper end pixel coordinates with the upmost detected line segments.
5. Draw a line connecting the lower end point and upper end point each side as lane line.

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming is that the detected lane would become really short or disappear when sharp curve is ahead. Because the draw_lines() assume the slope of the lane line is a constant for each frame, i.e. the result would be a straight line rather than a curve.

Another shortcoming could be that the current parameter settings might not be robust to different weathers that affect the color hue, saturation and light of the image.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to make the transition of the detected lane lines smoother among neighboring frames using tools like Kalman Filter.

Another potential improvement could be abandon the assumption of constant lane line slope in draw_lines() to adapt to sharp curves.
