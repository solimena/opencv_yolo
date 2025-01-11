# Image Processing Pipeline with YOLOv8

This repository provides a Python-based image processing pipeline leveraging **YOLOv8** for person detection and **OpenCV** for image manipulation. The code processes images by detecting persons, scaling them to a target bounding box height, cropping them to a specific aspect ratio (3:4), and saving intermediate and final outputs in organized folders.

---

## Why This Repository?

This pipeline is designed for tasks requiring precise detection and manipulation of images containing people. It organizes the workflow into distinct steps and uses globally configurable parameters for flexibility.

---

## Prerequisites

### Imports and Their Purpose

1. **`os`**: Used to handle file and folder operations, such as creating directories and accessing files in input/output folders.
2. **`cv2` (OpenCV)**: A powerful library for computer vision tasks, used here for image reading, manipulation, and saving.
3. **`YOLO` from `ultralytics`**: Provides the YOLOv8 model for object detection. YOLO is a state-of-the-art, real-time object detection framework.
4. **`matplotlib.pyplot`**: Used to display images during processing for debugging or visualization purposes.

---

## Installation

Clone the repository and install the required libraries:
```bash
git clone https://github.com/yourusername/image-processing-pipeline.git
cd image-processing-pipeline

# Install required libraries
pip install opencv-python opencv-python-headless ultralytics matplotlib
```

---

## Configuration

Key parameters are defined as **global variables** in the script for easy customization:
- **`desired_box_height`**: Target height (in pixels) for the bounding box around the detected person. Default: `2600`.
- **`crop_width`**: Width of the cropped image. Default: `1920`.
- **`crop_height`**: Height of the cropped image. Default: `2700`.

The aspect ratio of the crop is fixed at **3:4**, which is common for portrait images.

---

## Folder Structure

- **`input_images`**: Folder for input images.
- **`output_images`**: Folder for final processed images.
  - **`working_images`**: Subfolder for intermediate results like bounding box annotations.

Example:
```
.
├── input_images
│   ├── image1.jpg
│   ├── image2.jpg
├── output_images
│   ├── image1.jpg
│   ├── image2.jpg
│   ├── working_images
│       ├── image1_step1_bounding_box.jpg
│       ├── image2_step2_upscaled_bounding_box.jpg
```

---

## How It Works

### Step-by-Step Process

1. **Person Detection (YOLOv8)**:
   - The function `detect_person_with_yolo()` detects persons in an image using the YOLOv8 model.
   - It returns the coordinates (`x_min`, `y_min`, `box_width`, `box_height`) of the bounding box for the detected person.

2. **Save Bounding Box (Step 1)**:
   - The function `save_image_with_bounding_box()` saves an image with the detected person's bounding box highlighted in green.
   - This helps verify the detection before further processing.

3. **Scaling the Image**:
   - The bounding box is scaled to match the global `desired_box_height`.
   - The scaling factor is calculated using `calculate_scaling_factor()`, ensuring the person is resized to the desired height while maintaining proportions.

4. **Save Upscaled Bounding Box (Step 2)**:
   - After scaling, the bounding box is recalculated, and the upscaled image is saved.

5. **Center Cropping (Step 3)**:
   - The function `crop_image_to_center()` crops the image around the detected person to match the globally defined dimensions (`crop_width` and `crop_height`).
   - This ensures all output images have a consistent aspect ratio of **3:4**.

6. **Final Output**:
   - The final cropped image is saved in the `output_images` folder.
   - Intermediate results are saved in the `working_images` subfolder for debugging or review.

---

## Code Overview

### Global Variables
- `desired_box_height`: Sets the target height for scaling the bounding box.
- `crop_width` and `crop_height`: Define the size of the cropped output image. Maintain a 3:4 aspect ratio.

### Key Functions
1. **`process_images_in_folder(input_folder, output_folder)`**:
   - Iterates through all images in the `input_folder` and processes them using `process_image()`.

2. **`process_image(input_path, output_folder, working_folder)`**:
   - Handles the full pipeline for a single image:
     - Person detection (Step 1).
     - Image scaling (Step 2).
     - Center cropping (Step 3).
     - Saves intermediate and final outputs.

3. **`detect_person_with_yolo(image)`**:
   - Detects persons in an image using the YOLOv8 model and returns bounding box coordinates.

4. **`calculate_scaling_factor(box_height)`**:
   - Computes the scaling factor to resize the image based on the global `desired_box_height`.

5. **`crop_image_to_center(image, box)`**:
   - Crops the image around the bounding box, ensuring the output has the specified width and height.

6. **`save_image_with_bounding_box(image, box, output_path, comment)`**:
   - Annotates an image with a bounding box and saves it.

---

## How to Run

1. Add input images to the `input_images` folder.
2. Run the script:
   ```bash
   python process_images.py
   ```
3. Processed images will be saved in the `output_images` folder.

---

## Run on Google Colab

This project is compatible with Google Colab for easy execution without local setup. Follow these steps:

1. Open the file **`resize_and_center_image.ipynb`** on Colab.
2. Upload your images to a folder named `input_images` in the Colab environment.
3. Run the cells in the notebook sequentially to:
   - Install dependencies.
   - Upload input images.
   - Process images using the pipeline.
   - Download the processed images.

4. Processed images will be stored in the `output_images` folder within the Colab environment, ready for download.

---

## Example Output

For an input image:
- **Step 1**: Save the image with the detected bounding box highlighted in green.
- **Step 2**: Scale the image to ensure the bounding box matches the target height.
- **Step 3**: Crop the image to a 3:4 aspect ratio.

---

## Extendability

This pipeline can be extended to:
- Include other object detection models for multi-class detection.
- Perform pose estimation or feature extraction.
- Process video frames instead of static images.

---

## License

This project is licensed under the MIT License. See the `LICENSE` file for more details.

Feel free to contribute by submitting issues or pull requests. 😊

