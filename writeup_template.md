# **Finding Lane Lines on the Road** 

## Writeup Template

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Create a pipeline to find lanes on a road
* Reflect on the work for shortcomings and potential areas of improvement 


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection


**Pipeline to detect lane lines in a video** 

*First*, I converted the images to grayscale. 

*Second*, I applied a gaussian blur with a kernel size of 3 over the images to smooth out the noise. 

*Third*, I used the canny edge detection function to strip away everything but the edges of the picture. I tuned the parameters to a low          threshold of 100 and a high threshold of 250 to a reasonable amount of satisfaction. 

*Fourth*, I created a quadraleteral region of interest to focus my efforts on. Beginning with the corners of the images and the top               end going up to 330 on the y axis, I was able to obtain the necessary regional space to focus on the edges of lane lines.

*Fifth*, I used the hough transform function to convert the edge points to lane line overlays. I optimized the parameters to capture the 
         relevant information and discard incongruencies. 
 
**Solid Lane Lines** 

After the process of drawing lane lines on a video was successfully completed, I sought to draw a single left and right lane line. ultimately, this would help us bound a self-driving car. However, to do this a rather simple approach was taken. 

I left the draw_lines() function as it is, and instead created a separate function called new_lines(), where I built my approach to create single solid lines. This function takes the set of edges returned by the canny edge detection function and returns two single solid line parameters which are then fed into the draw_lines() function. 

The new lines function collects all the variables of the canny edge lines individually. It then proceeds to average the individual parameters such as x1, y1, and slopes of the lines. The obtained parameters are then averaged for both the left and right lanes, following which an intercept b is derived. We now have the intercept b.

To draw a line we need the x and y values for the top and bottom. The y values are derived using the boundaries of our region of interest. The bottom is taken simply from the bottom edge of the picture/boundary. The top is a little more complicated, I had to use the meshgrid function to identify all the white lines in the canny image and minimize the y value to get the top edge. 

Now, with the y values, the averaged slope of all edges m, and averaged intercept of all edges b, we can use the y = mx + b equation to derive x values for the lines. We return these x and y values for the left and right lines into the draw_lines() function within the hough transform function to draw our single solid lines. 

If you'd like to include image

![pipeline flowchart][Pipeline.PNG]


### Potential Shortcomings

Ultimately, the pipeline closely follows and overlays solid lines on the lane lines. Ignoring outliers quite well, the function produces solid lines to a good degree of satisfaction. However, one potential shortcoming would be that the lines overlayed on the video are quite shaky. If we order a self driving car to follow these solid lanes, we might be faced with a nauseating amount of steering jerk. 

Another potential shortcoming is what happens when the car is faced with a curving road of greater radius? We've transformed the lane lines to single solid lines using the line equation. This inherently doesn't fit well to curves. 


### Potential areas of improvement

A possible improvement would be to smooth out the solid lines that are drawn. Perhaps, our data is overfit, which is why we see such shaky lines. We could use filters to find just vertical edges and then use them for our solid lines and consider averaging over a several frames to further smoothen the jerks. 

Another potential improvement could be to use spline fits or polynomial fits to track the curves in the road better. 

Thank you! 
Hope you enjoyed this project as much as I did :) 
