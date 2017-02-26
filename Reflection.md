#**Finding Lane Lines on the Road** 
## by Abelardo Marquez


The goals / steps of this project are the following:

- Make a pipeline that finds lane lines on the road
- Reflect on your work in a written report


---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted on the following 5 steps: 

1. Grayscale tranformation
2. Gaussian smoothing
3. Canny Edge Detection
4. Set Mask
5. Hough Transform
 - Linear Regression
 - Drawing of lines 

First the **Grayscale transformation** of the image is required in order to help on detecting the image edges. Then I applied a **gaussian smoothing** with a kernel of 3. Next comes the **Canny Edge Detection** algorithm where I choosed a range of (50,150) following John Canny suggestions of a ratio of 1:2 or 1:3.

I set a **polygon mask** based on the dimensions of the image and visuals, this points are defined on the `vertices` function.

Then I run the **Hough Transform** function with (1) calculates the lines inside the masked region (2) and *in order to display a single line* for each side of the road, I modify the `draw_lines` function by applying linear regression on the obtained points (3) and then draw the resulting line. 

Also in order to avoid that lines from one side coould affect the other side, I decide to select lines that not only had the same slope (positive or negative) but also lines from a certain side (left: x < Width/2 or right: x > width/2). the If snipped is presented here:

```
    for line in lines:
        for x1,y1,x2,y2 in line:
            
            slope = (y2-y1)/(x2-x1)
            
            if slope < 0 and x1 < (imshape[1]/2) and x2 < (imshape[1]/2) :
                xl.extend([x1,x2])
                yl.extend([y1,y2])
                
            elif slope > 0 and x1 > (imshape[1]/2) and x2 > (imshape[1]/2):
                xr.extend([x1,x2])
                yr.extend([y1,y2])
                
``` 
 This discrimination of left and right sides increased stability on whileline video and yellow line video.
 
 The result of the pipeline on an image is displayed here:
 
 ![resulting image](imageresult.png)






###2. Identify potential shortcomings with your current pipeline


A shortcoming could be if the lines are on a different color or on a not so contrating color, so a straight line would be hard to detect. 

Another shortcoming is that this calculation is restrained to the values of the image/video I was working with, but if the camera is moved to another position or the road ahead is not straight but curved, some lines might get detected and the resulting line would be wrong (as it happens on the optional challenge video)  

Also the presence of shadows seems to mess the calculations.


###3. Suggest possible improvements to your pipeline

A possible improvement would be to change the way I calculate the average resulting slope. Maybe using a calculation that instead of taking in account the points, take in account line with similar slopes. Perhaps using the mode of the slope values. 

Another potential improvement could be to somehow increase the contrast to accentuate the edges somehow. 

Detection of curves if the road ahead is not straight, as the current pipeline shows to not behave good on the optional challenge.

Also a way to detect shadow would be helpful to somehow avoid it on the calculation, if this is somehow possible. 