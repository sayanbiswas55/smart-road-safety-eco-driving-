# Automatic-Number-Plate-Recognition-System
This project is an end-to-end computer vision pipeline that detects and reads license plates from a video feed in real-time. It uses YOLOv8 for object detection and EasyOCR for text extraction, along with advanced OpenCV techniques for pre-processing and text stabilization.

### 🎥 Demo
<img width="1173" height="664" alt="Screenshot 2025-11-17 at 7 49 03 PM" src="https://github.com/user-attachments/assets/706650c8-45e9-4410-bd7e-41a0fc436ef3" />


## 📋 Table of Contents

* [Problem Statement](#-problem-statement)
* [Solution Pipeline](#-solution-pipeline)
* [Technology Stack](#-technology-stack)
* [How to Use](#-how-to-use)
* [Conclusion](#-conclusion)
* [Future Scope](#-future-scope)

## 🎯 Problem Statement

The primary objective was to build an automated system to accurately detect and read license plate numbers from a video. The system needed to be robust enough to handle vehicles in motion, variable lighting, and diverse, non-Indian plate formats (e.g., European plates).
Key challenges included:
* Filtering "junk" text from non-plate elements (like country stickers or borders).
* Correcting common OCR ambiguities (like 'Z' vs. '2', or 'B' vs. '8').
* Stabilizing the final text to prevent flickering between frames.

## 💡 Solution Pipeline

Our method uses a multi-stage pipeline to process the video frame by frame:

1.  **Model Training:** We fine-tuned a **YOLOv8-Nano** model on a Roboflow license plate dataset to quickly detect plate locations.
2.  **Image Pre-processing:** For each detected plate, we applied a series of OpenCV filters (**Grayscaling**, **Median Blur**, and **Adaptive Thresholding**) to remove noise and handle poor lighting.
3.  **OCR and "Junk" Filtering:**
    * **EasyOCR** (with `paragraph=False`) reads text in blocks (e.g., `['SK']`, `['XZ8X']`, `['8473']`).
    * A custom filter **trims "junk" blocks** (like the 'SK' band) from the start and end of the list.
    * The remaining valid blocks are joined (`XZ8X8473`) and formatted with a space (`XZ8X 8473`).
4.  **Temporal Stabilization:** To prevent flickering, we store the last 10 readings for each plate and use the **majority vote** as the final, stable text.
5.  **Final Output:** The stable text and bounding box are drawn onto the frame, and all processed frames are saved into the final output video.

## 🛠️ Technology Stack

* **Programming Language:** Python 3
* **Object Detection:** Ultralytics YOLOv8 (trained with PyTorch)
* **OCR:** EasyOCR
* **Data & Training:** Roboflow (Dataset) https://universe.roboflow.com/roboflow-universe-projects/license-plate-recognition-rxg4e/dataset/11#
* **Computer Vision:** OpenCV (for video I/O, pre-processing, and annotation)
* **Data Processing:** NumPy, Regular Expressions (re)
* **Environment:** Google Colab (with T4 GPU)


## 🔮 Future Scope

* Train the model on a larger, more diverse dataset (including Indian plates) to create a universal plate reader.
* Optimize the pipeline for real-time (e.g., 30 FPS) performance using batch inference or model quantization (TensorRT).
* Integrate the output with a database to check against a watchlist for security or toll applications.
