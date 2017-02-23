#**Finding Lane Lines on the Road** 

##Daniel Kim


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
- Get 2 images (Grayscale and blue image)
- Gaussian filter
- Canny edge
- Region of interest on Canny image
- Hough transform to get lines
- Draw 2 lines given above lines.

My draw_lines function processes arrays of lines (defined by 4 integers - x1,y1,x2,y2) in following ways
- Divide them by 2 groups by its x location (left vs. right) : Any line which crosses the boundary will be discarded
- Filter out lines that are clearly not oriented to the lane. E.g. Horizontal line, or line with opposite gradient.
- Serialize lines into arrays of x and y coordinates.
- Run a linear polynomial fit on the points and get the gradient and m.
- Draw the lines on the image.  

![alt text][image1]


###2. Identify potential shortcomings with your current pipeline
(Not a shortcoming) Handling different sizes of images are handled using ratios instead of pixel values. (e.g. Region of interest)

However the pipeline can fail depending on image color profile since Canny parameters assumes that the lane vs road will guarantee a certain difference in color value. This was evident in the last challenge problem where the lane had very similar brightness compared to the road causing existing pipeline to fail badly. I mitigated this limitation by using blue image (since color yellow lacks blue component), however it's still far from perfect.

One more limitation is that the canny and Hough transforms are very sensitive to artifacts. (e.g. shadows or tire marks on the road) This is especially hard to handle when their color profiles are similar to lanes. Tightening Hough parameter to filter out shorter lines has a risk of false negatives by discarding short lanes.


###3. Suggest possible improvements to your pipeline

It might be better to explore the parameter space more throughly (maybe via some sort of automated tests) so that we can find the optimum set give the training data. However, I think this has the risk of overfitting for the training data (which I ran across multiple times during this project) unless given the vast amount of training data to play with.

It would be nice for the pipeline to be "shape-aware" to reject artifacts. Lanes have distinctive shapes which can provide more information to the detection algorithm.

