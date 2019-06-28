# **Finding Lane Lines on the Road**

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps.

First, I used color and region masking as a practice and to see the shortcomings of using this method.

Then I converted the images to grayscale, as Canny Edge Detection algorithm uses greyscale images.

Then, I used gaussian blur. The blur removes some of the noise and spurious gradients by averaging.

Then I found the edges using Canny Edge Detection of opencv library. Edge detection algorithm returns points at the edge

I used region masking to filter the edges in region of interest (possible lane lines area)

Then I used hough transform to find line segments from points detected by Canny Edge Detection.

Lane lines at right or left skip lines. I've tunes some parameters to get accurate lane detection.  


In order to draw a single line on the left and right lanes, I modified the draw_lines():

At first, I calculated the slope of each line segments. If the slope is positive it is likely for the line segment to be from right lane.
If the slope is negative it is likely to be from left line. However, I need to provide a range of slopes: 0.52 to 0.72 for right lane and
-0.82 to -0.62 for left lane. The reason for this is, no matter I tuned the parameters of Canny Edge Detection and Hough transform I find
wrong lines. As I tried to calculate the average slope next, these wrong lines change the slope a lot. I needed to eliminate these lines.

Then I calculate the length of line segments. I thought if line segment is longer, the line detection would be more trustworthy. Then I
used the length of line segment as weight for calculating average lane slope. I've done this for right and left lanes separately.

To position a line, I need also a point. To calculate point, I also find the average x and y values.

I decided to extend the line that I will draw to the bottom of figure, so I used max y value as one endpoint's y coordinate, and I used
minimum y value of the region of interest as y coordinate of other endpoint. Then I found x coordinate of endpoints using slope, average
point and y coordinate of other endpoints.

Finally I draw these left and right lines.

The final image is like this ![image can't found][image1]

### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when:

* There is a slope on the road
* There is a sharp turn at the Road
* At night
* There is shadow on road
* Raining
* Direct sunlight
* When lane line is close to safety fence

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to clustering line segments and removing distant line segments.

Another potential improvement could be to use different color space at first, then convert to grayscale.

A potential improvement for video images would be averaging lines of previous image and current image to smooth the lane detection
(prevent jumps)

A potential improvement for video images is to check the position of the lane points and find the distance between consecutive lines.
If the distance is big, eliminate the one not consistent with previous lines.
