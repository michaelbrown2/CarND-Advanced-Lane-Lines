## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./examples/checker_distorted.png "Distorted"
[image2]: ./examples/checker_undistorted.png "Undistorted"
[image3]: ./examples/2.png "Binary Example"
[image4]: ./examples/with_lines.png "Warp Example"
[image5]: ./examples/lines_on_combined.png "Fit Visual"
[image6]: ./examples/complete.png "Output"
[image7]: ./examples/testimage_distorted.png "Distorted"
[image8]: ./examples/testimage_undistorted.png "Undistorted"
[video1]: ./project_video_output.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the labeled code cell of the IPython notebook located in "./advancedlanefinding.ipynb"  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]
![alt text][image2]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

Distorted:
![alt text][image7]
Undistorted:
![alt text][image8]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used all the functions provided in the class as well as added L channel from LUV color space and B channel from Lab color space. After processing a warped image, it looks like this:  
![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `perspective_transform()`, which appears the labelled section in my journal.  The `warper()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I used hardcoded values as follows:

```python
src = np.float32([[img_size[0] * 0.5 - 60.0, img_size[1] * 0.5 + 102.0],
                    [img_size[0] * 0.17 + 35.0, img_size[1]],
                    [img_size[0] * 0.83  + 75.0, img_size[1]],
                    [img_size[0] * 0.5 + 70.0, img_size[1] * 0.5 + 102.0]])
                    
dst = np.float32([[(img_size[0] * 0.25), 0],
                    [(img_size[0] * 0.25), img_size[1]],
                    [(img_size[0] * 0.75), img_size[1]],
                    [(img_size[0] * 0.75), 0]])
```


I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?
See section in notebook labeled Find Lane Lines
I have a function named sliding_window which is essentially the function given in the course that I use to find the lines, identify their points, and then i fit them to a polynomial. I hope to integrate the 'memory' function into this so it doesn't start from scratch on each image and instead would remember previous positions.
![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

See the applicable section in the journal labelled Pipe Line, essentially just a trimmed version of the function given to us by the class


#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

See the applicable section in the journal labelled Pipe Line  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).


Here's a [link to my video result](./project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I had a huge problem with the perspective transformation.  Apparently my brain does not function in that 3D space and I had a very hard time getting my numbers right to get a decent transformation.  Once past that, my biggest issues were just my lack of experience in the Python programming language.  I would like to add the additional STAND OUT features but will have to do so at a later date due to time constraints.