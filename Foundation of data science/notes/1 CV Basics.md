
## Filtering
### Linear Filtering
- Linear filtering involves replacing each pixel in an image with a linear combination of its neighbours.
- This technique can be used to reduce noise, fill in missing information, and extract image features like edges and corners.
- The simplest case of linear filtering is convolution, which uses a filter kernel to multiply and sum values from a local image patch.
	- Convolution is a common linear filtering operation that can be expressed as matrix multiplication. It involves sliding the filter kernel across the image, multiplying the kernel values with the corresponding pixel values, and summing the results to get the new pixel value.
##### Applications of linear filtering
- Noise reduction
- Filling in missing values/information
- Extracting image features (edges, corners...)
### Gaussian Filtering
- Gaussian filtering is a type of linear filtering that uses a Gaussian kernel, which weights nearby pixels more than distant ones.
- This approach reflects the idea of probabilistic inference and leads to effective smoothing of the image.
- Gaussian filters are separable, meaning they can be implemented efficiently by convolving each row and column with a 1D Gaussian filter.

### Multi-Scale Image Representation: Gaussian Pyramid
- A Gaussian pyramid is a multi-scale representation of an image created by repeatedly applying Gaussian smoothing and subsampling.
- This pyramid allows for the analysis of images at different levels of detail, enabling tasks like searching for objects across scales.
- Subsampling without proper pre-smoothing can introduce aliasing artefacts, where high-frequency details are misrepresented. Using a Gaussian filter before downsampling helps prevent this by attenuating high frequencies.

### Edge Detection
- Edges represent significant changes in image intensity and can be detected using derivatives.
- Image derivatives can be computed using linear filters that approximate the first and second derivatives of the image.
	- The first derivative is useful for detecting edges as points of rapid intensity change
	- The second derivative helps locate edges as zero-crossings.
- The Canny edge detector is a popular algorithm that combines Gaussian smoothing, gradient computation, non-maximum suppression, and hysteresis thresholding for robust edge detection.
	1. Gaussian smoothing: Reduces noise before calculating derivatives
	2. Gradient computation: Calculate the magnitude and direction of the gradient using derivative filters
	3. Non-maximum suppression: Thin edges by keeping only the local maxima of the gradient magnitude along the gradient direction
	4. Hysteresis thresholding: Use two thresholds to connect strong edges and extend them to weaker ones
- The Laplacian of Gaussian operator, often approximated using the Difference of Gaussians, can also be used for edge detection by identifying zero-crossings in the second derivative.
- The choice of scale for smoothing filters (e.g., the sigma value for Gaussian) influences the types of edges detected. Smaller scales capture fine details, while larger scales focus on more prominent edges.

## Recognition
### Recognition using Line Drawings
- Line drawings, often generated from edge detection results, can serve as a simplified representation for object recognition and localisation.
- By matching model lines to extracted image lines, the 3D pose of an object can be estimated.
- Method
	1. Extract edges from the image using an edge detector
	2. Fit lines to the detected edges, representing object boundaries
	3. Match model lines (from a known object) to the extracted image lines
	4. By finding the best-fitting alignment, the 3D pose (position and orientation) of the object can be estimated.
### Object Instance Identification using Color Histograms
- Object instance identification aims to recognize specific instances of objects, distinguishing them from other instances of the same class.
- Color histograms provide a statistical representation of the color distribution in an image, enabling object identification by comparing histogram similarity.
- They are robust to translation, rotation, and partial occlusion.
- However, color histograms are sensitive to illumination changes and might not be discriminative enough for all object types.

##### Comparison Measures for Histograms:
- **Intersection:** Measures the overlap between two histograms, reflecting shared colour characteristics. Ranges from 0 (no overlap) to 1 (identical).
- **Euclidean distance:** Quantifies the overall difference between histograms. Larger distances indicate less similarity.
- **Chi-square:** A statistically motivated measure sensitive to differences in bin proportions. More discriminative than Euclidean distance but can be affected by outliers.