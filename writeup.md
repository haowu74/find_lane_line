# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on the work in a written report


[//]: # (Image References)

[image1]: ./test_images_result/1_solidWhiteCurve.jpg "Grayscale"
[image2]: ./test_images_result/2_solidWhiteCurve.jpg "Gaussian Blur"
[image3]: ./test_images_result/3_solidWhiteCurve.jpg "Canny"
[image4]: ./test_images_result/4_solidWhiteCurve.jpg "Triangle"
[image5]: ./test_images_result/5_solidWhiteCurve.jpg "Hough"

---

### Reflection

### 1. Pipeline

My pipeline consisted of 5 steps:  

1) Convert RGB picture to grey by calling grayscale() 

![Grayscale][image1]

2) Apply Gaussian Blur to the grayscale picture by calling gaussian_blur(), with kernel_size = 5

![Gaussian Blur][image2]

3) Use Canny to highlight the boundary. After a few fine-tune, I am using:
    low_threshold = 100
    high_threshold = 125  

![Canny][image3]  

4) Cut off the region we are interested, since the lane will only show in the lower half of the picture, mostly a traingle area, we use the vertices of [[0, ysize-1], [xsize-1, ysize-1], [xsize*3/8, ysize*3/8]]

![Triangle][image4]

5) Use Hough Transform to find the lines.
    rho = 1
    theta = np.pi /180
    threshold = 60
    min_line_len = 100
    max_line_gap = 200
I adjusted the function draw_lines() to put a condition before draw a line:
    if abs((x1-x2)/(y1-y2)) < 2:
It will remove the horizontal-alike lines, since we are focusing on the lane, which should not be horizontal in the picture.

![Hough][image5]

At last, we call weighted_img to put the detected lines onto the initial picture for display purpose.

### 2. Potential shortcomings with the current pipeline

1) As far as I can see from the result of 'challenger' video, the shortcomings of the current pipeline is the yellow lines are too close to white / grey paved road surface when being transformed to gray scale. 

2) The current pipeline cannot deal with the very curly roads - the Hough Transform only detects the straight line, the car will have trouble when drive into roundabout. 


### 3. Suggest possible improvements to the pipeline

1) In terms to the shortcoming 1) I think we should use some algorithm to increase the yellow in the image so that Canny algothms can detect it without introduing too much noise.

2) I think the Hough Transform needs to be used to detect curve to make car be able to pass through the very curly road or roundabout.