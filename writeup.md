# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

---

### Reflection

### 1. The pipeline and why it looks like it does

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I gaussian blur them, then use canny edge detector to highlight edges in the image like the lane markings. Then anything outside a trapezoid that is located where I expect the road to be in the image. Then Hough line transform is used to find line segements from eges detected by canny, which are inside the trapezoid.

To recognise lanes from the lines from the hough transform, I have modified draw_lines() to partition the lines as left or right depending on what half of the screen they are on. A line is also filtered away if its slope is to horizontal or if it is sloping to the wrong direction for being on its side. For each side (left and right), I add the two endpoints for each lines to a set as points. Then fit a line through the set using np.polyfit. That is done inside function fit_lane(), the variable invslope describe the slope and invslope of 0 mean a vertical line. I choose that instead of tracing the inverse where 0 means a horizontal line, because I imagine that it will lead to less numerical problems since lanes or more vertical than horizontal.

### 2. Improvements and possible improvements

An improvement would be to fit the line through the points by minimising the sqared shortest distance to the line to the center instead of the x distance.

The color of the line can also be used when deciding what are the lines. Using some other color space than RGB could be benificial, like HLS or HSV. That is probably necessary if there is a car in the lane. Handling curved lanes by mapping some other more complex curve than just a straight line.

Some robust estimation method like RANSAC can be useful, instead of just minimizing MSE with fitPolyl to better handle false positive lines.

The size of the line can also be weighted in the fitting to benefit more clear lines, but I think that punish the lines futher away unnecessarily.
