Edge Detection:
- Scanline: one row of pixels in an image
	- Compute B[m+1] - B[m]
	- Take the first derivative of a scanline, and it becomes nonzero when an edge (pixels change values) is encountered
	- since derivative of scanline is f(x) - f(c) / 1, this becomes:
		- I[m] = 1 times B[m+1] + -1B[m]
		- This is really just a dot product of the vector [-1 1] repeated each pixel in the resulting image
		- I[m] = [B[m] B[m+1] dot [-1 1]]
		- [-1 1] is a mask
- Convolution: 
	- Mathematical transformation that consists in multiplying two signals
$$
	- Discrete: (f∗g)[n]=∑f[n−k]g[k]
$$
	- Convention for computer vision is to use cross-correlation
$$
	∑∀mx[n]y[n+m]
$$
	- Note: https://dsp.stackexchange.com/questions/27451/the-difference-between-convolution-and-cross-correlation-from-a-signal-analysis
	- https://www.fieldbox.ai/app/uploads/2023/03/CONV_5.gif
	- The operation of moving a mask across an image is called convolution
	- A multiplication in the Laplace domain
	- y(t) integral x(τ) h(τ) dτ
-  Convolution can be used for smoothing, averaging, blurring
- Edge detection, object detection, selecting a region
- Why is it useful:
	- Edge Detection:
		- Convolution is a way to approximate spatial gradients, can detect edges (strong variations in colors between close pixels)
		- Since a convolution is a sum of differences and products, it is a good tool to approximate many mathematical operators
	- Learning a generic representation:
		- Convolutional Neural Networks (CNNs)
- Gaussian Masks:
	- Used to smooth images and for noise reduction
	- Use before edge detection to avoid false edges
- Edge Detection:
	- Many convolution kernals can be used for edge detection (Laplace, Sobel, Canny)
	- How would we find 'interesting' parts of an image?
		- Let's say corners are interesting
		- Convolve with Gaussians, take difference
			- Wide masks lead to uncertainty about location of edge
			- Can detect more gradual edges
		- Scale Stability
			- Interest points should be stable to scaling
			- Meaning, the feature detection algorithm should be work across different sizes and resolutions of an image
			- Can calculate DoG (difference of gaussians) at many scales, find strong edges in space and scale
			- You use this by taking the difference of one gaussian with another by subtracting a Gaussian kernel with a different width
			- https://en.wikipedia.org/wiki/Difference_of_Gaussians
		- Scale Space is a multi-scale representation of an image, where the image is progressively blurred using a Gaussian filter with increasing width
			- Typically represented as a pyramid or series of images where each level is a different scale or blur 
		- Keypoints:
			- Specific points on an image that are considered informative
			- Used as compact representation of an images content
			- Distinct, repeatable, local, invariant
			- Harris Corner
			- Scale-Invarient Feature Transform (SIFT)
				- Detects keypoints using DoG
				- Speeded Up Robust Features (faster SIFT)
		- Keypoint Orientation:
			- Keypoint orientation is a property associated with a keypoint
			- Represents the dominant direction or orientation of the local image patch surrounding the keypoint
			- Important to achieve rotation invariance 
			- Common approach to determining keypoint orientation is to compute the gradient magnitude and orientation in a local area around the keypoint
				- Then, the gradient orientations are weighted by their magnitudes and aggregated into a histogram
				- The peak of the histogram indicates the dominant orientation, which is assigned as the keypoint orientation
			- Keypoint orientations are used to align the feature descriptors to allow consistent identification of features even under rotation
				