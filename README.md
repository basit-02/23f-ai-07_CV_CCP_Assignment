```markdown
# Human Pose Estimation & Activity Classification

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![MediaPipe](https://img.shields.io/badge/MediaPipe-Vision-orange)
![OpenCV](https://img.shields.io/badge/OpenCV-4.x-green)

A lightweight, end-to-end Computer Vision pipeline designed to detect human poses, analyze joint geometry, and classify distinct physical activities (e.g., Standing vs. Squatting). This project utilizes modern pose estimation models and signal processing techniques to ensure accurate, jitter-free tracking suitable for real-world deployment.

## Features

* **Advanced Pose Detection:** Utilizes the `MediaPipe PoseLandmarker` Vision Task API for robust 33-point skeletal landmark extraction.
* **Signal Smoothing:** Implements **Savitzky-Golay** and **Moving Average** filters via `SciPy` to reduce high-frequency noise and jitter in the coordinate time-series data.
* **Dynamic Joint Geometry:** Calculates 2D joint angles (Knee, Hip, Elbow) dynamically per frame, correcting for video aspect ratios.
* **Rule-Based Classification:** Classifies frame-by-frame activity states based on geometric thresholds.
* **Comprehensive Evaluation:** Automatically generates performance metrics (Accuracy, Precision, Recall, F1-Score) and plots a Confusion Matrix against manual ground truth data.

## Tech Stack

* **Language:** Python
* **Computer Vision:** OpenCV, MediaPipe
* **Data Processing:** NumPy, Pandas, SciPy
* **Visualization:** Matplotlib

## Project Structure

```text
├── main.py                     # Main execution script for the pipeline
├── squat.mp4                   # Input video (max 2 mins, clearly showing activities)
├── pose_landmarker_full.task   # MediaPipe pre-trained model weights
├── output_skeleton.mp4         # Output video with skeletal overlay and tracking info
├── joint_angles_tracking.png   # Generated plot of raw vs. smoothed joint angles
├── confusion_matrix.png        # Generated evaluation confusion matrix
└── pose_metrics_log.csv        # Frame-by-frame log of coordinates, angles, and predictions

```

## Installation & Setup

1. **Clone the repository:**
```bash
git clone [https://github.com/your-username/pose-estimation-classifier.git](https://github.com/your-username/pose-estimation-classifier.git)
cd pose-estimation-classifier

```


2. **Install the required dependencies:**
```bash
pip install mediapipe opencv-python pandas matplotlib scipy

```


3. **Download the Model Weights:**
The script is designed to automatically download the `pose_landmarker_full.task` file on its first run.

## Usage

1. Place your target video file in the root directory and name it `squat.mp4` (or update the `INPUT_VIDEO` constant in the script).
2. Run the main processing script:
```bash
python main.py

```


3. The script will process the video frame-by-frame and generate the visualizations, CSV logs, and the final annotated `.mp4` file in the same directory.

## 📊 Results & Visualization

### Skeletal Tracking & Smoothing
The pipeline aggressively smooths the raw coordinate data using a Savitzky-Golay filter to prevent classification flickering. A dual-threshold hysteresis state machine (110° for Squatting, 150° for Standing) is utilized to govern state transitions.

<img width="4200" height="3600" alt="joint_angles_tracking" src="https://github.com/user-attachments/assets/f68c26d0-13e6-4d8f-ae62-94ca68be3e8a" />

### Activity Classification Evaluation
Evaluated against manually labeled ground truth data for a 1223-frame sequence, the baseline rule-based classifier achieved the following metrics:

| Metric | Score |
| :--- | :--- |
| **Total Frames Analyzed** | 1223 |
| **True Positives (Squat)** | 36 |
| **True Negatives (Stand)** | 718 |
| **False Positives** | 403 |
| **False Negatives** | 66 |
| **Overall Accuracy** | **61.65%** |
| **Precision** | 8.20% |
| **Recall** | 35.29% |
| **F1-Score** | 13.31% |

**Engineering Note:** The high baseline False Positive rate indicates sensitivity to the specific camera angle, where the subject's natural resting extension registered below the strict 150° standing threshold. Future roadmap features include auto-calibrating resting thresholds based on the initial frames of the video to improve precision across diverse camera setups.

## Author

**Muhammad Basit Memon** B.S. Artificial Intelligence

* [LinkedIn](https://www.linkedin.com/in/muhammad-basit-memon-a1a24921a/)

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](https://www.google.com/search?q=LICENSE) file for details.

```

```
