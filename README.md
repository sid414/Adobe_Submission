# Project Documentation

## Overview

This project addresses two main problems: shape detection and symmetry detection. Each problem has its own dedicated notebook or script for processing and analyzing SVG files.

### Problem 1: Shape Detection

**Notebook**: `shape_detection_from_svg.ipynb`

#### Overview

This project involves the detection of shapes from SVG files. The provided Jupyter notebook `shape_detection_from_svg.ipynb` is designed to extract geometric data, apply transformations, detect intersections, smooth points, and classify shapes based on the provided data.

#### Input

- **SVG File**: The primary input is an SVG file containing the vector data of shapes.

#### Output

- **Detected Shapes**: The output includes a set of points along with their estimated shapes, centroids, and line segments.

#### Working

#### 1. SVG Extraction and Transformation

The script processes the SVG file to extract geometric data, such as points and lines, and applies transformations like translation, scaling, rotation, and skewing.

##### Dependencies

The following Python libraries are required:
- `svgpathtools`: For parsing SVG path data.
- `numpy`: For numerical operations.
- `svgwrite`: For creating SVG files (optional).

Install the dependencies using:
```bash
pip install svgpathtools numpy svgwrite
```

##### Key Functions

1. **`apply_transformations(points, transform)`**: Applies transformations (translate, scale, rotate, skew) to 2D points.
   - **Parameters**:
     - `points` (numpy array): The points to transform.
     - `transform` (str): Transformation string from SVG.
   - **Returns**: Transformed points as a numpy array.

2. **`extract_and_store_svg(input_file)`**: Extracts geometric data from an SVG file and applies any specified transformations.
   - **Parameters**:
     - `input_file` (str): Path to the SVG file.
   - **Returns**: A list of transformed geometric data.

#### 2. Intersection Detection and Smoothing

The script detects and analyzes intersections between line segments and applies smoothing to the points along these segments.

##### Key Functions

1. **`line_equation(p1, p2)`**: Calculates the slope (m) and y-intercept (b) of the line passing through two points.
   - **Returns**: Slope (m) and intercept (b) of the line, or `None` and the x-coordinate if the line is vertical.

2. **`find_intersection(line1, line2)`**: Finds the intersection point of two lines, if it exists.
   - **Returns**: Coordinates of the intersection point, or `None` if the lines are parallel.

3. **`segment_intersects(p1, p2, p3, p4)`**: Determines whether two line segments intersect.
   - **Returns**: Intersection point if it exists, otherwise `None`.

4. **`smooth_points(points, factor=0.5)`**: Applies a basic smoothing algorithm to a list of points.
   - **Returns**: Smoothed points as a list.

5. **`analyze_intersections_in_set(segment_set)`**: Analyzes and processes intersections within a set of segments, applying smoothing where intersections occur.
   - **Returns**: Processed segments with intersections analyzed and smoothed.

#### Example Usage
```python
input_svg_file = r"path_to_svg_file.svg"
stored_data = extract_and_store_svg(input_svg_file)
finalres = []
for idx, segment_set in enumerate(stored_data):
    print(f"Processing set {idx + 1}/{len(stored_data)}")
    resulting_segments = analyze_intersections_in_set(segment_set)
    finalres.extend(resulting_segments)
```

#### 3. Visualization of Point Cycles

The script visualizes cycles of points in a 2D plane, with each cycle represented as a closed loop of points.

##### Function: `plot_cycle(cycle_points, title="Cycle Plot")`
- **Purpose**: Plots a cycle, which is a sequence of 2D points forming a closed loop.
- **Parameters**:
  - `cycle_points`: A list of tuples or lists, where each tuple/list contains the x and y coordinates of a point in the cycle.
  - `title`: (Optional) A string for the plot title. Default is `"Cycle Plot"`.
- **Behavior**: The function plots the cycle, connecting the last point back to the first to close the loop.

#### 4. Shape Detection and Classification

The script processes the points to classify shapes based on their geometric properties, such as distances, angles, and centroid locations.

##### Key Functions

1. **Distance and Angle Calculations**:
   - `euclidean_distance(p, q)`: Computes the Euclidean distance between two points.
   - `calculate_angle(A, B, C)`: Calculates the angle between three points.

2. **Point Preprocessing**:
   - `remove_duplicate_points(points, distance_threshold=2)`: Removes duplicate points that are too close to each other.
   - `calculate_centroid(points)`: Finds the centroid of a set of points.
   - `calculate_side_lengths(points)`: Computes the lengths of the sides of the polygon formed by the points.
   - `calculate_interior_angles(points)`: Calculates the interior angles of the polygon.

3. **Shape Checking**:
   - `check_circle(points, centroid, tolerance=0.1)`: Determines if the points approximately form a circle.
   - `is_ellipse(points, tolerance=2.0)`: Fits an ellipse to the points and checks the fit quality.

4. **Line Segment Fitting**:
   - `fit_line_least_squares(points)`: Fits a line to the points using least squares.
   - `calculate_residuals(points, m, c)`: Calculates the residuals of the points relative to the fitted line.

5. **Clustering and Line Segments**:
   - `cluster_points(points, epsilon=1.0)`: Clusters points using DBSCAN to group them into potential line segments.
   - `process_clusters(clusters)`: Processes the clusters to fit line segments and classify the points.

6. **Ellipse Fitting**:
   - `ellipse_equation(x, h, k, a, b)`: Defines the ellipse equation for fitting.
   - `fit_ellipse(points)`: Fits an ellipse to a set of points.

7. **Classification**:
   - `classify_based_on_lines(line_segments, centroid, remaining_points)`: Classifies the shape based on the detected line segments and any remaining points.

8. **Processing Cycles**:
   - `apply_to_cycles(cycles)`: Applies the entire processing pipeline to each cycle of points.

#### Processed Results

After processing each cycle, the results include:
- **Line Segments**: Equations of the detected line segments.
- **Remaining Points**: Points that could not be classified into line segments.
- **Shape Classification**: The determined shape of the cycle (e.g., "Circle," "Square," "Triangle").
- **Centroid**: The centroid of the points in the cycle.

#### Example Output

```plaintext
Line Segments:
Line: y = 0.5x + 2.0
Line: y = -1.5x + 10.0
...
Remaining Points:
(3, 4)
(5, 6)
...
The shape is classified as: Square
Centroid: (4.5, 5.5)
```

### Problem 2: Symmetry Detection

**Script**: `mirror_symmetry.py`

#### Overview
This script is focused on detecting mirror symmetry in images. It takes a converted PNG image from an SVG file, identifies symmetry lines, and visualizes the symmetry by drawing a line over the image.

#### Input:
- **PNG image**: Generated from the SVG file and processed for symmetry detection.

#### Output:
- Top 10 pairs of symmetry points.
- A graph showing the midpoints of symmetric features.
- A final image with the detected line of symmetry overlaid.

#### Key Functions:
1. **`detecting_mirrorLine`**: Detects the mirror line by comparing the original image and its mirrored counterpart.
2. **`mirror_symmetry` (Class)**: Contains helper functions for symmetry detection, which are used in `detecting_mirrorLine`.

#### Helper Functions:
- **`find_matchpoints(self)`**: Matches features between the original and mirrored images.
- **`find_points_r_theta(self, matchpoints: list)`**: Calculates polar coordinates (r, θ) for the midpoints of matched keypoints.
- **`draw_matches(self, matchpoints, top=10)`**: Visualizes the best feature matches between the original and mirrored images.

### How to Access the Solution:
1. **Run the detection.ipynb Notebook**:
   - This notebook processes an SVG file and converts it into a PNG image for symmetry detection.
   
2. **Output**:
   - The output includes an image with a line of symmetry, indicating the detected symmetry axis.

### References

1. Seel-audom, C., Naiyapo, W., Chouvatut, V., & Chouvatut, V. (2017). *A search for geometric-shape objects in a vector image: Scalable Vector Graphics (SVG) file format*. In *2017 9th International Conference on Knowledge and Smart Technology (KST)*. DOI: [10.1109/KST.2017.7886098](https://doi.org/10.1109/KST.2017.7886098)

2. Shalma, H., & Selvaraj, P. (2022). *Deep-Learning Based Object Detection and Shape Recognition in Multiple Occluded Images*. In *2022 International Conference on Data Science, Agents & Artificial Intelligence (ICDSAAI)*. IEEE, 979-8-3503-3384-8/22/$31.00.

3. Brahmbhatt, S. (2013). *Practical OpenCV*. Packt Publishing. ISBN: 978-1-78216-358-0.

4. Har-Peled, S., & Varadarajan, K. R. (2001). *Approximate shape fitting via linearization*. In *Foundations of Computer Science, 2001. Proceedings. 42nd IEEE Symposium on* (pp. 167-176). IEEE. DOI: [10.1109/SFCS.2001.959881](https://doi.org/10.1109/SFCS.2001.959881)

5. Jadhav, S., Tarle, B., & Waghmare, L. M. (2007). *Polygonal Approximation of 2-D Binary Images*. In *Fourth International Conference on Information Technology: New Generations (ITNG 2007)*, 2-4 April 2007, Las Vegas, Nevada, USA. DOI: [10.1109/ITNG.2007.151](https://doi.org/10.1109/ITNG.2007.151)

6. Loy, G., & Eklundh, J.-O. (2006). *Detecting Symmetry and Symmetric Constellations of Features*. In *Computer Vision - ECCV 2006, 9th European Conference on Computer Vision*, Graz, Austria, May 7-13, 2006, Proceedings, Part II. DOI: [10.1007/11744047_39](https://doi.org/10.1007/11744047_39)

7. Xiao, Z., & Wu, J. (2007). *Analysis on Image Symmetry Detection Algorithms*. In *Fourth International Conference on Fuzzy Systems and Knowledge Discovery, FSKD 2007*, 24-27 August 2007, Haikou, Hainan, China, Proceedings, Volume 4. DOI: [10.1109/FSKD.2007.173](https://doi.org/10.1109/FSKD.2007.173)
