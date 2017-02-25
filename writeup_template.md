#**Finding Lane Lines on the Road**

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps.
1. ![ Original image][https://github.com/itsymbal/CarND/blob/master/solidWhiteRight.jpg]
1. I converted the images to grayscale
2. Apply Gaussian blur with kernel size 5
3. Apply Canny edge detection with thresholds 100, 150 ![After applying Canny][https://github.com/itsymbal/CarND/blob/master/solidWhiteRight_cannied.jpg]
4. Apply region of interest mask to a height of 310 ![After applying region of interest][https://github.com/itsymbal/CarND/blob/master/solidWhiteRight2_cannied_trimmed.jpg]
5. Run Hough line detection with line drawing function.
![After applying Hoff detection ][https://github.com/itsymbal/CarND/blob/master/solidWhiteRight_hoffed.jpg]
![After combining with original ][https://github.com/itsymbal/CarND/blob/master/solidWhiteRight_combined.jpg]



In order to draw a single line on the left and right lanes, I modified the draw_lines() function as follows:
1. Calculate slope of the line.
1. For line segments that are part of the left side, filter only segments which are on the left side of video (X position < 500) and slope < -.3 (slopes with > -.3 are too high, so discard them)
1. Add X and Y coordinates to an array
1. Run a polyfit (polynomial least-squares regression) function over X and Y points. This gives me average slope and 'q' value for formula y = mX +q
2. Using the formula X = (y - q)/m , calculate X for Y position vertical_size_of_image - that's the lower point for line
3. Use max X value, and calculate Y value using formula f = np.poly1d(para), where 'para' is what's returned by linear regression. Max X value is the right-most line segment top. These X and Y become the upper end for the line.
4. Draw the line.

If you'd like to include images to show how the pipeline works, here is how to include an image:



###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when there are other high-contrast changes in road surface such as new asphalt, temporary lane markings subsequently removed.

Another shortcoming could be if a car or truck immediately in front is blocking much of the view.

Lane marked by cones, barrels, plastic tubes, other markers



###3. Suggest possible improvements to your pipeline

o A possible improvement would be to use color information from the image (white or yellow lanes color for example)
o Another potential improvement could be to give more weight to lane markings which are close then those which are far
o Identify non-painted markers using image detection and classification. Calculate center of base of cones, barrels etc. Use those points to plot line.
o Improve region of interest mask to remove some of the space between the lane markings from previous image.
o Carry information forward from one image to next.
o Keep and use an average of markings from several frames.
