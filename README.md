# jigsaw-solver
## What ?
This project involves **developing a program that can automatically solve a jigsaw puzzle** given an image of the disassembled jigsaw. It uses both shape properties and color information of each puzzle piece as the matching criteria.
## Why ?
Solving jigsaw puzzles is an attractive problem because **it requires a solid understanding of computer vision techniques**, including image segmentation, shape description, partial boundary matching, and feature extraction. **The project can have applications in several areas**, such as connecting satellite images to maps, and art restoration.
## Technical Challenges
1. Locks searcher
    * Detecting inner/outer lock from a piece of jigsaw
2. Puzzle Aligner
    * Aligning Puzzles in the Upright Position
3. Puzzle Joiner
    * Using algorithm to calculate score for candidate piece
        * Matching algorithm
        * Colour Descriptor
## Related Works
- Using Computer Vision to Solve Jigsaw Puzzles
    * https://web.stanford.edu/class/cs231a/prev_projects_2016/computer-vision-solve__1_.pdf
- Computer Vision Powers Automatic Jigsaw Puzzle Solver :
    * https://www.abtosoftware.com/blog/computer-vision-powers-automatic-jigsaw-puzzle-solver
- Faster Jigsaw Solving :
    * https://www.i-programmer.info/news/181-algorithms/4380-faster-jigsaw-solving.html
- fastdtw :
    * https://pypi.org/project/fastdtw/
- Comparing the Performance of L*A*B* and HSV Color Spaces
with Respect to Color Image Segmentation :
    * https://arxiv.org/ftp/arxiv/papers/1506/1506.01472.pdf
    
## Method and Results
### Pre-processing and removing background
1. Changed the image's color space to HSV
2. Computed histogram to determine dominant color
3. After obtaining the mask, use median blur to reduce noise.
4. Image subtraction using a mask 
![download](https://github.com/bxntn/jigsaw-solver/assets/52697656/8c174678-9e32-4340-8c6a-5607ff7e08ae)
### Finding contour
* Using findContours from opencv library<br>
![download](https://github.com/bxntn/jigsaw-solver/assets/52697656/7f9a40be-5b3e-40c3-a453-f3925bf748ae)
### Creating JigsawLoader
> for calling function

* **find_contour**:
Find contours using function cv::findContours from mask obtained from image segmentation
* **crop_img**:
Crop the image according to the bounding boxes obtained from finding contours with 30 pixels margin.
* **find_corner**:
Finds the convex hull of contour of each puzzle and then approximates a polygon using the Douglas-Peucker algorithm. Go through every 4-point combination in the polygon, the combination with smallest standard division is the four corners.
* **find_center**:
Find the center of the puzzle piece using 4 corners.
* **find_locks**:
Identify type of the lock -- flat, inner, and outer using convex hull and convexity defects.
* **aligner**:
Rotate the puzzle piece so that the x and y axes are aligned with the image's axes.
### Creating Jigsaw object
Object that contained
- label
- x (center)
- y (center)
- corners
- image
- contour

### Matching score algorithm
- with shape<br>
1. Extend the border to make two puzzle pieces the same size
2. Crop a puzzle and extend the image for combined two images with XOR
3. Fill the unnecessary inner hole with white color<br> 
![download](https://github.com/bxntn/jigsaw-solver/assets/52697656/514b6827-c904-4d31-9aaa-2916f1b756a7)<br>
4. Change color space to gray-scale then apply XOR
5. Calculate the score using black color and image size proportion<br> 
![download](https://github.com/bxntn/jigsaw-solver/assets/52697656/b05279e1-a679-4f97-932e-33ff3647ab0a)
- with colour
1. Get RGB value along the edge contour.
![download](https://github.com/bxntn/jigsaw-solver/assets/52697656/44cc8907-cceb-4004-a799-9a691b22b611)<br>
![download](https://github.com/bxntn/jigsaw-solver/assets/52697656/5aeee50f-220c-4fae-949f-16c84c72f976)<br>
2. Compared its using DTW algorithm
### Joiner Algorithm
1. Start with corner piece<br>
![step1](https://github.com/bxntn/jigsaw-solver/assets/52697656/99083ab6-ecc8-4fda-a39d-a78af67cd189)
2. Find candidates pieces from remaining pieces<br>
![step2](https://github.com/bxntn/jigsaw-solver/assets/52697656/3a1e54df-37c7-448a-a333-1ddb19de22e2)
3. Calculate matching score and choose the best one<br>
![step3](https://github.com/bxntn/jigsaw-solver/assets/52697656/84931d8b-855e-4078-b3f4-c575811c7896)
4. User confirm or choose from the candidate pieces<br>
![step4](https://github.com/bxntn/jigsaw-solver/assets/52697656/2948724e-edec-4173-88b9-51b99cebb3ed)
5. Place the jigsaw and move to next position<br>
![step5](https://github.com/bxntn/jigsaw-solver/assets/52697656/b91a6995-9ea0-4a80-94eb-c3cf65c98608)
6. Repeat steps 2 – 5 until complete<br>
![step6](https://github.com/bxntn/jigsaw-solver/assets/52697656/943c2d48-3e9d-4d67-b25f-1334d6432c54)
### Result
![result](https://github.com/bxntn/jigsaw-solver/assets/52697656/57c67e01-6a84-49e2-81b6-7460eb0e92fe)
## Discussion and Future Work
### limitations
1. The solver must be confirmed whether or not the recommended puzzle is the proper piece.
2. Corner detection is still inaccurate, affecting other modules such as the aligner module.
### Possible Improvement
1. Visualize
    * Joiner animation
2. Algorithm approach
    * Segmentation algorithm
    * Corner finder algorithm
    * Aligner algorithm
    * Matching algorithm
