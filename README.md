# WorkShop 1
# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look

### Developed by: Deepak K R
### Reg No: 212225040057

Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features:
- Detects the face in an image.
- Places a stylish sunglass overlay perfectly on the face.
- Works seamlessly with individual passport-size photos.
- Customizable for different sunglasses styles or photo types.

## Technologies Used:
- Python
- OpenCV for image processing
- Numpy for array manipulations

## How to Use:
1. Clone this repository.
2. Add your passport-sized photo to the `images` folder.
3. Run the script to see your "cool" transformation!

## Applications:
- Learning basic image processing techniques.
- Adding flair to your photos for fun.
- Practicing computer vision workflows.

## Program: 

```python

# Import libraries
import cv2
import numpy as np
import matplotlib.pyplot as plt

# 1. Load the Face Image
faceImage = cv2.imread('25008695.jpg')
plt.imshow(faceImage[:,:,::-1]); plt.title("Face")
plt.show()

print("Face Image Dimension =", faceImage.shape)

# 2. Load the Sunglass image (JPEG with black background)
glass_img = cv2.imread('glass.png')
plt.imshow(glass_img[:,:,::-1]); plt.title("glass.png")
plt.show()

# Create a mask from the black background since it's a JPEG without an alpha channel
glass_gray = cv2.cvtColor(glass_img, cv2.COLOR_BGR2GRAY)
_, glassMask1_full = cv2.threshold(glass_gray, 10, 255, cv2.THRESH_BINARY)

# 3. Define the Region of Interest (ROI) for the boy's eyes
# Tweak these coordinates if the glasses don't align perfectly on your screen
roi_y1, roi_y2 = 880, 1230   # Y-coordinates (Height = 80)
roi_x1, roi_x2 = 1200, 2050   # X-coordinates (Width = 300)

glass_w = roi_x2 - roi_x1
glass_h = roi_y2 - roi_y1

# Resize the sunglasses and the newly created mask to fit over the eye region
glassBGR = cv2.resize(glass_img, (glass_w, glass_h))
glassMask1 = cv2.resize(glassMask1_full, (glass_w, glass_h))
print("Resized Glass Dimension =", glassBGR.shape)

# Display the images for clarity
plt.figure(figsize=[15,15])
plt.subplot(121); plt.imshow(glassBGR[:,:,::-1]); plt.title('Sunglass Color channels')
plt.subplot(122); plt.imshow(glassMask1, cmap='gray'); plt.title('Sunglass Alpha channel (Generated)')
plt.show()

# 4. Naive Replacement
faceWithGlassesNaive = faceImage.copy()

# Replace the eye region directly with the sunglass image (will leave a black box around the frames)
faceWithGlassesNaive[roi_y1:roi_y2, roi_x1:roi_x2] = glassBGR

plt.imshow(faceWithGlassesNaive[...,::-1]); plt.title("Naive Replacement")
plt.show()

# 5. Arithmetic Masking for Seamless Blending
# Make the dimensions of the mask same as the input image
glassMask = cv2.merge((glassMask1, glassMask1, glassMask1))

# Make the values [0,1] since we are using arithmetic operations
glassMask = np.uint8(glassMask/255)

# Make a copy of original for arithmetic blending
faceWithGlassesArithmetic = faceImage.copy()

# Get the eye region from the face image
eyeROI = faceWithGlassesArithmetic[roi_y1:roi_y2, roi_x1:roi_x2]

# Use the mask to create the masked eye region (black out the area where glasses will go)
maskedEye = cv2.multiply(eyeROI, (1 - glassMask))

# Use the mask to create the masked sunglass region (black out the background of the glasses)
maskedGlass = cv2.multiply(glassBGR, glassMask)

# Combine the Sunglass in the Eye Region to get the augmented image
eyeRoiFinal = cv2.add(maskedEye, maskedGlass)

# Display the intermediate results
plt.figure(figsize=[20,20])
plt.subplot(131); plt.imshow(maskedEye[...,::-1]); plt.title("Masked Eye Region")
plt.subplot(132); plt.imshow(maskedGlass[...,::-1]); plt.title("Masked Sunglass Region")
plt.subplot(133); plt.imshow(eyeRoiFinal[...,::-1]); plt.title("Augmented Eye and Sunglass")
plt.show()

# Replace the eye ROI with the output from the previous section
faceWithGlassesArithmetic[roi_y1:roi_y2, roi_x1:roi_x2] = eyeRoiFinal

# Display the final result
plt.figure(figsize=[20,20])
plt.subplot(121); plt.imshow(faceImage[:,:,::-1]); plt.title("Original Image")
plt.subplot(122); plt.imshow(faceWithGlassesArithmetic[:,:,::-1]); plt.title("With Sunglasses")
plt.show()


```

## Output:

<img width="475" height="512" alt="Screenshot 2026-08-08 212310" src="https://github.com/user-attachments/assets/e86f830d-fa4e-4930-8f70-fb40d945008f" />

<img width="680" height="346" alt="Screenshot 2026-08-08 212320" src="https://github.com/user-attachments/assets/98d306f7-3e0f-4e02-9207-83439c28c265" />

<img width="1282" height="282" alt="Screenshot 2026-08-08 212331" src="https://github.com/user-attachments/assets/18fe4e96-8858-42d7-8aaf-2975b88e7292" />

<img width="450" height="491" alt="Screenshot 2026-08-08 212338" src="https://github.com/user-attachments/assets/dd61174d-1f81-4434-9e20-a0e32d4e86a6" />

<img width="1235" height="195" alt="Screenshot 2026-08-08 212348" src="https://github.com/user-attachments/assets/316007b1-048c-45c3-98a4-ae5ebe9e4eb6" />

<img width="1092" height="680" alt="Screenshot 2026-08-08 212400" src="https://github.com/user-attachments/assets/2f932134-734f-44e4-a56f-655f78a969bc" />




## Result:
The sunglasses were successfully placed over the eye region of the given image using image masking and alpha blending. The final processed image was displayed successfully in the Jupyter Notebook.
