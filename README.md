# Finding Lane Lines on the Road
#### Adithya Balaji

---

**Project Goals**
* Develop a computer vision pipeline that finds lane lines on the road
* Reflect on the pipeline in a written report

[//]: # (Image References)

[image1]: ./writeup_resources/initial_image.jpg "Initial Image"
[image2]: ./writeup_resources/region_filter.jpg "Region Filtering"
[image3]: ./writeup_resources/color_filter.jpg "Color Filtering"
[image4]: ./writeup_resources/grayscale_filter.jpg "Grayscale Filter"
[image5]: ./writeup_resources/blur_filter.jpg "Blur Filter"
[image6]: ./writeup_resources/simple_hough.jpg "Simple Hough Lines"
[image7]: ./writeup_resources/complex_hough.jpg "Complex Hough Lines"
[image8]: ./writeup_resources/final_result.jpg "Pipeline Result"

---

## Reflection

### The Pipeline
#### Summary
The vision pipeline would filter and annotate the image as follows:

Initial Image              |  Pipeline Result
:-------------------------:|:-------------------------:
![][image1]                |  ![][image8]

Note: We will be using a snapshot image form the challenge image to visualize the pipeline process

The vision processing pipeline consists of three main sections:

1. Color based filtering
2. Hough lines filtering
3. Line filtering

#### Color Based Filtering
Color based filtering consists of two parts. First the image is masked to a region of interest which filters the image to nearly only the lane lines. The image is then converted to the HSV and filtered by color. This colorspace change is performed so that colors can be more easily selected. Surprisingly with a filtering based on value only, the lane lines could be isolated.

Region Filtering           |  Color Filtering
:-------------------------:|:-------------------------:
![][image2]                |  ![][image3]

#### Hough Lines filtering
Next, the color filtered image enters the Hough lines filtering portion. Here the image is first converted to grayscale. Next, a gaussian blur is applied and then canny edge detection is performed on the image. Finally, the Hough lines algorithm is applied to the canny edges result and the initial lines annotation is shown in red.

Grayscale Conversion       |  Blur Filter              |  Simple Hough Lines
:-------------------------:|:-------------------------:|:-------------------------
![][image4]                |  ![][image5]              |  ![][image6]

#### Line Filtering
Now that we have a few candidate lines segments on each side of lane, we can take the average of these lines in order to present a final annotated road. This pipeline is mostly included in the helper function section. First the lines are separated by slope into left and right lines. Next, the lines are averaged and the bottom point is used to generate the equations that are to represent the average line. Next, this average line is transformed into points to be drawn on the image using the intersection of the middle of the image and the base of the image as reference lines. Lastly, if both a left and right lane are detected the lines are shaved to their intersection.

These filtered lines are then blended into the initial image to annoate the lane line position.

Complex Hough Lines        |  Pipeline Result
:-------------------------:|:-------------------------:
![][image7]                |  ![][image8]

### Future Work
#### Shortcomings and improvments
Although, the pipeline covers a lot of edge cases there is still some potential areas of improvement:

1. The pipeline does not always detect a left and right lane
	* The pipeline could be further tuned to match these edge cases using the cv2 morphological filters to better capture the lane markings
2. The pipeline jitters quite a bit
	* In order to smoothen this jitter the pipeline could cache previous detections and only update the annotation if the lane changes past a set point
3. The pipeline needs to be tuned to road conditions
	* Jumping from the white and yellow lanes to the challenge image required additional tuning of the pipeline which in the end made the pipeline more robust but a color invariant solution could avoid this issue altogether








