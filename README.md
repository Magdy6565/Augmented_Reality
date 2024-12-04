# Augmented Reality Application: Book and Video Integration



## **1.1 Getting Correspondences**

To establish correspondences between the image of the book cover (Figure 2.a) and the first frame of the video, the **SIFT descriptor** from OpenCV was used to detect keypoints and descriptors in both images. A brute-force matcher was applied with the **KNN matching method** (size=2) to find the two best matches for each descriptor. To filter out good matches, a **ratio test** was applied, retaining only matches where the distance of the closest match is significantly less than the distance of the second-closest match.

### Implementation Steps:

1. Extract keypoints and descriptors using the SIFT algorithm.
2. Match descriptors using the KNN method.
3. Apply a ratio test to filter good correspondences.



## **1.2 Compute the Homography Parameters**

To align points between the book cover image and the video frame, a **homography matrix** \(H\) was computed. This 3×3 matrix transforms points from one view into their corresponding points in another view.

### Implementation Steps:

1. Select a minimum of 4 pairs of corresponding points between the two views.
2. Construct a system of linear equations \(Ax = b\), where \(A\) is derived from the correspondences, \(b\) represents the point coordinates, and \(x\) contains the homography parameters.
3. Solve for \(x\) using least squares, setting \(H_{3,3} = 1\) to normalize the matrix.
4. Verify the homography matrix by mapping points from the book cover image to the video frame.

---

## **1.3 Calculate Book Coordinates**

Using the computed homography matrix, the four corners of the book in the video frame were identified. The four corners of the book cover image were mapped onto the video frame, defining the region of interest (ROI) for the AR overlay.

### Implementation Steps:

1. Define the four corners of the book cover image in pixel coordinates.
2. Transform these points into the video frame using the homography matrix.

---

## **1.4 Crop AR Video Frames**

To fit the video frames onto the book cover, each frame of the movie video was cropped to match the aspect ratio of the book cover. Only the central region of the video frame was retained to ensure optimal fitting.

### Implementation Steps:

1. Calculate the aspect ratio of the book cover.
2. Crop the movie video frames to match the aspect ratio.
3. Resize the cropped frame to match the dimensions of the book cover.

---

## **1.5 Overlay the First Frame of the Two Videos**

The first cropped frame of the movie video was overlaid onto the first frame of the video, replacing the book cover.

### Implementation Steps:

1. Use the homography matrix to align the movie frame with the book cover.
2. Blend or replace the region corresponding to the book cover in the video frame with the cropped movie frame.

---

## **1.6 Creating the AR Application**

The final AR application replaces the book cover in every frame of the video with the corresponding cropped frame of the movie video.

### Implementation Steps:

1. For each video frame:
   - Compute the homography matrix between the current frame and the book cover or the previous frame.
   - Calculate the new location of the book in the video frame.
   - Crop and resize the movie frame to fit the detected book region.
2. Overlay the cropped movie frame onto the detected book region in each video frame.

---

