# Automated Barcode and QR Code Scanner


A computer vision project to detect, localize, and decode barcodes from images using OpenCV and Pyzbar. This system demonstrates a practical image processing pipeline from raw image to extracted data.


![Barcode Scanner Output](https://scanbot.io/wp-content/uploads/2024/12/opencv-pyzbar-tutorial-barcode-scanning-result.png)


---

## Table of Contents

*   [Project Overview](#-project-overview)
    *   [Goal](#-goal)
    *   [Role](#-role)
    *   [Challenges](#-challenges)
*   [Introduction: What is a Barcode?](#-introduction-what-is-a-barcode)
*   [Methodology: The Computer Vision Pipeline](#-methodology-the-computer-vision-pipeline)
*   [The Code Workflow & Libraries](#-the-code-workflow--libraries)
*   [Results](#-results)
*   [Conclusion](#-conclusion)
*   [How to Use](#-how-to-use)

## Project Overview

This section outlines the core objectives, my responsibilities, and the key difficulties encountered during the development of this barcode detection system.

### Goal
The primary goal of this project is to develop an automated system that can accurately **detect, localize, and decode** barcodes and QR codes from digital images. The system should be robust enough to handle various image conditions and serve as a foundational tool for applications in retail, logistics, or inventory management.

### Role
As the project developer, my role was to engineer the entire computer vision pipeline. This involved:
*   Researching and implementing image preprocessing techniques to enhance barcode visibility.
*   Applying gradient analysis and morphological transformations to isolate potential barcode regions.
*   Using contour detection to accurately localize the barcode within the image.
*   Integrating a third-party decoding library (`pyzbar`) to extract the final information.
*   Structuring the code to be modular, readable, and efficient.

### Challenges
*   **Image Quality:** The algorithm must be resilient to variations in lighting, shadows, blur, and low resolution, which can obscure the barcode's features.
*   **Noise and Clutter:** Differentiating the barcode from other objects, text, and patterns in a busy image is a significant challenge.
*   **Perspective and Distortion:** Barcodes are not always perfectly aligned with the camera. The system needs to handle rotations, skewing, and perspective distortions.
*   **Finding a Universal Approach:** Developing a single pipeline that reliably works for both 1D (linear) and 2D (QR code) barcodes requires a generalized feature detection approach.

## Introduction: What is a Barcode?

A barcode is a machine-readable representation of information, typically in the form of a visual pattern of parallel lines (1D barcodes) or squares, dots, or other geometric patterns (2D barcodes, like QR codes). Barcodes are widely used for data storage, identification, and inventory management in industries such as retail, logistics, and healthcare.

## Methodology: The Computer Vision Pipeline

The project follows a sequential pipeline where the output of one step serves as the input for the next.

1.  **Image Acquisition:** The process begins with capturing an image using a camera or scanner. The image might include a barcode alongside other content.

2.  **Preprocessing:** Techniques such as grayscale conversion, contrast enhancement, and noise removal are applied to improve the quality of the image. This step is crucial for reliable detection.

3.  **Region of Interest (ROI) Identification:** We use a Sobel filter to compute the image gradient, emphasizing the high-frequency transitions between the black and white bars of a barcode. By subtracting the vertical gradient from the horizontal gradient, we strongly highlight vertically-oriented patterns.

4.  **Barcode Localization:**
    *   The gradient image is blurred and thresholded to create a binary mask, highlighting regions that are likely to contain a barcode.
    *   **Morphological operations** (specifically, a "closing" operation followed by erosions and dilations) are applied to close the gaps between the individual bars of the barcode, forming a single, solid "blob" or contour.
    *   `cv2.findContours` is then used to detect the outline of this solid region.

5.  **Bounding Box Extraction:** We identify the largest contour, which is assumed to be our barcode. A bounding box is drawn around this contour to mark its exact location. The region containing the barcode is extracted for decoding.

6.  **Decoding:** The isolated barcode region is passed to the **`pyzbar`** library, which analyzes the pattern and decodes it into a text string.

## The Code Workflow & Libraries

The implementation relies on several key Python libraries to execute the computer vision pipeline.

| Library | Role in Project |
| :--- | :--- |
| **cv2 (OpenCV)** | Used for all core image processing tasks: loading images, color space conversion (`cvtColor`), blurring, gradient calculation (`Sobel`), thresholding, and morphological operations (`morphologyEx`). |
| **numpy** | Provides the fundamental data structure (the `ndarray`) for representing images and enables efficient numerical operations on them. |
| **matplotlib.pyplot** | Utilized for visualizing intermediate steps and displaying the final result in development environments like Jupyter Notebooks. |
| **pyzbar** | A powerful, dedicated library for decoding various types of barcodes and QR codes from images. It's the final step in our pipeline. |

## Results

The final output of the script is the original image with a bounding box drawn around the detected barcode. The decoded information (the barcode's data and type) is printed to the console and can be overlaid on the image.

**Console Output:**
```
Decoded Barcode Type: EAN13
Decoded Barcode Data: 9780201379624
```

**Visual Output:**
The script displays the image with the green bounding box, confirming successful localization.

## Conclusion

This project successfully demonstrates the power of a classical computer vision pipeline in solving practical problems. By combining gradient analysis, morphological transformations, and contour detection, we can create a robust system for automating barcode detection. This approach is highly relevant for building applications in industrial, retail, and healthcare environments, where efficiency and accuracy are paramount.

## How to Use

To run this project on your local machine, follow these steps.

**1. Clone the Repository**
```bash
git clone https://github.com/your-username/barcode-scanner-project.git
cd barcode-scanner-project
```

**2. Create a Virtual Environment (Recommended)**
```bash
python -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
```

**3. Install Dependencies**
This project requires the following libraries. You can install them using the `requirements.txt` file.
```bash
pip install -r requirements.txt
```
*(If you don't have a `requirements.txt` file, create one with the following content:)*
```
opencv-python
numpy
matplotlib
pyzbar
```

**4. Run the Script**
Place your barcode images in an `images/` directory. Run the script from the command line, specifying the path to your image.

```bash
python image_proccessing_Finalproject.py --image images/example1.jpg
```
