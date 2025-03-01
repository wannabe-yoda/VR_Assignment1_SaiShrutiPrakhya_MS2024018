# VR_Assignment1

# Question 2: Panorama Stitching

Keypoints of 3 overlapping images were identified using SIFT and they were stitched together using OpenCV's stitcher class. The following section shows the final satisfactory output. The next section will show the outputs that were not satisfactory and then the analysis of the entire process. 

## Optimal Results
### Input Images

The following three images of a living room were used as input:

![Input Image 1](Input_Images/Pan_Living_Room1.jpg)
![Input Image 2](Input_Images/Pan_Living_Room2.jpg)
![Input Image 3](Input_Images/Pan_Living_Room3.jpg)

### Process

1. **Image Loading**: 
   - Three overlapping images were loaded using OpenCV
   - Images were converted from BGR to RGB color space for visualization

2. **Feature Detection**:
   - SIFT (Scale-Invariant Feature Transform) algorithm was used for feature detection
   - SIFT parameters were tuned with contrastThreshold=0.04 for better feature detection
   - Keypoints were detected and visualized on each image:

![Keypoints](Results/Pan_Living_Room_Keypoints.png)

3. **Image Stitching**:
   - OpenCV's Stitcher class was used in PANORAMA mode
   - Images were automatically aligned and blended
   - The final panorama was generated:

![Final Panorama](Results/Pan_Living_Room_Final.png)

### Implementation

The implementation uses the following key components:
- OpenCV for image processing and stitching
- SIFT algorithm for feature detection
- Matplotlib for visualization
- NumPy for array operations

## Alternative Attempts and Analysis

1. **Using a custom stitching function for creating the panorama**
   - The distortion in the panorama can be attributed to several factors in this implementation.
   - The FLANN-based matching with a ratio test threshold of 0.8 could be too lenient, potentially including incorrect matches that affect the homography calculation.
   - The homography estimation using RANSAC with a reprojection threshold of 5.0 might not be optimal for these specific images, causing perspective distortions.
   - The blending approach using a Gaussian mask, while attempting to create smooth transitions, might have introduced ghosting effects when the alignment wasn't perfect.
   - The sequential stitching approach (stitching images one by one) can accumulate errors, as any misalignment in earlier steps propagates and amplifies through subsequent stitching operations. This explains why the final panorama appears distorted compared to the OpenCV Stitcher class implementation, which uses a more sophisticated global alignment and blending strategy.
![Distorted Panorama](Results/Pan_Living_Room_distorted.png)

2. **Using other sets of images**
   - There was a huge variation in panorama quality between the two indoor scenes - living room(Refer to the Optimal Results section) and the hostel common room (elaborated below).
   -  The living room scene had more favorable characteristics for stitching: consistent depth of field, well-distributed features across the frame, and uniform lighting conditions.
   -  In contrast, the hostel room scene presented more challenging conditions. The presence of a sofa in close proximity to the camera created significant parallax error.
   -  This parallax effect violates the fundamental assumption in panoramic stitching that the scene can be approximated as a planar surface viewed from different angles.
   -  While the transformation to LAB color space and subsequent image enhancement operations (smoothing and edge enhancement) helped improve feature detection by reducing noise and emphasizing structural elements, they couldn't fully compensate for the inherent geometric challenges posed by the varying depth planes.
   -  The LAB color space transformation was particularly helpful because it separated luminance from chrominance, allowing for better feature detection in areas with varying lighting conditions, but the fundamental issue of parallax distortion from the close-up sofa remained a limiting factor in achieving seamless stitching.
   -  Note that in the results below "Original Image" refers to the RGB image itself and the "Enhanced Image" refered to the image in the LAB color space.
   -  Additionally, the presence of windows in the hostel room scene introduced significant exposure variations, with bright light sources creating high contrast areas and uneven illumination across the capturing plane. These extreme lighting differences from the windows caused inconsistent feature detection between overlapping images and created challenges in blending areas where bright window light met darker indoor shadows, further complicating the stitching process.
   ![Hostel Common Room Keypoints](Results/Hostel_Common_Room_Keypoints.png)
   ![Hostel Common Room Panorama](Results/Pan_Hostel_Common_Room.png)

