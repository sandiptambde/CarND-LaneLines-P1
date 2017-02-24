#**Finding Lane Lines on the Road** 

---

**The goals / steps of this project are the following:**
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg       "Grayscale"
[image_input]: ./examples/input.jpg      "Input Image"
[image_gray] : ./examples/gray.jpg       "GrayScale"
[image_gauss]: ./examples/gauss.jpg      "Gauss"
[image_canny]: ./examples/canny.jpg      "canny"
[image_roi]  : ./examples/roi.jpg        "roi"
[image_hough_lines_avg_extrapolate]: ./examples/hough_line.jpg "Hough Lines"
[image_output]  : ./examples/output.jpg     "Output Image"

---

### Program
Please check [code](CarND-LaneLines-P1/P1.ipynb)

---

### Written Report - Reflection


##1. Steps/Pipeline
For each Image:
e.g.
![alt text][image_input]
a. Converted the image to Grayscale image using function: grayscale(img)
![alt text][image_gray]
b. Applied Gaussian noise kernel on Grayscale image using function: gaussian_blur(img, kernel_size)
![alt text][image_gauss]
c. Then, used Canny Transform to find edges. Function used:canny(img, low_threshold, high_threshold))
![alt text][image_canny]
d. After that, found Region of Interest(ROI) using function:region_of_interest(canny_img, vertices) 
   - Depending on size of image, defined vertices:
     540 x 960  : (339,336),(164,540),(550,321),(917,540)
     720 x 1280 : (572,463),(180,720),(730,438),(1220,720)
![alt text][image_roi]
e. For eadges present in ROI, drawn Hough lines on blank image using function :hough_lines(img, rho, theta, threshold, min_line_len, max_line_gap)
f. Averaged out left lane & right line hough lines to get single line for each lane & extend them using function:draw_lines(img, lines, color=[0, 255, 0], thickness=10) 
![alt text][image_hough_lines_avg_extrapolate]
g. To get expected ouput, Combined blank image(having line drawn) with original image using function:weighted_img(img, initial_img, α=0.8, β=1., λ=0.)
![alt text][image_output]


##2. Identify potential shortcomings with your current pipeline
Current pipeline is working fine on videos solidWhiteRight.mp4 & solidYellowLeft.mp4

Getting following issue when run on challenge.mp4
a. Solution is not working well for brightslopt (3sec - 5sec) in the video
   Lines are not drawn stright one th video


##3. Suggest possible improvements to your pipeline
a. Fine tuning with different functions parameter to check if accuraccy can be increased
b. For Brighspot issue mentioned above, implement % deviation which can be allowed.
   If deviation is below certain threshold:
   	then only redraw new line
   else:
   	Don't redraw. Use previously drawn line
