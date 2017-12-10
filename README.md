# **Finding Lane Lines on the Road** 

### Pipeline Description

My lane detection pipeline is fairly minimal. The first step consists of converting the image
to grayscale since all the image processing functions require that the image be in grayscale
colorspace. The noise in the grayscale image is then suppressed by applying a Gaussian blur
with a kernel size of 3. My approach is to use edges as the primary feature for detecting
the lanes so I run Canny edge detection on the filtered image. The lanes are generally
in the lower part of the image, so a triangular region of interested is extracted from the edge
image to make the lane detection process easier. Finally, the lane lines are detected from the
edges by using Hough Transform.

I decided not to use color segmentation as part of my pipeline since the color thresholds
tuned on the input images would not generalize well. Additionally, the lane marking colors
can actually vary a lot so it would not make sense to rely on color for detecting lanes.

I modified the `draw_lines` function to make the lane detection output more stable and 
make debugging easier. Concretely, I fit a first order polynomial to the detected lanes 
and run a low-pass filter on the polynomial parameters so that the output doesn't jump 
around a lot. This also allows approximate lane identification possible for images in which
the raw detection pipeline could not find any lanes.

The visualized output consists of three types of lines:
```
Green color: Left lane
Blue color: Right lane
Red color: Raw lane detection output
```

![alt text](test_images/solidWhiteRight.jpg)
![alt text](test_images/solidWhiteRightOutput.jpg)


### Shortcomings with current pipeline

One major shortcoming with my pipeline is that I assume that the lane lines can be modelled
accurately with a first order polynomial. This may not be true for all situations and it may
be necessary to model the curves in the lane for planning / in-lane driving purposes.

### Possible improvements to the pipeline

Extracting the lanes using the Hough Transform limits the lanes to being represented as a 
straight lines only. This constrains the horizon to which the algorithm can extract the lanes 
in a given image - especially if the image contains a curve on the road. While this
works for highway driving, it may not work for urban driving that contains a more diverse
set of lane markings. I would like to find an alternative to using Hough Transform that can
do more than just detect straight lines.

Additionally, I would like to implement an algorithm that can classify the lane type - for 
example: `Solid Yellow`, `Solid White`, `Broken White` etc.
