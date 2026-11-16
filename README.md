# SignSense AI — Real-Time Indian Sign Language Recognition System

<div align="center">

![SignSense AI Banner](https://img.shields.io/badge/SignSense-AI-00e5ff?style=for-the-badge&logo=openai&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-2.15-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![MediaPipe](https://img.shields.io/badge/MediaPipe-0.10.14-0077ED?style=for-the-badge&logo=google&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-4.8-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-2.3-000000?style=for-the-badge&logo=flask&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white)

**A Deep Learning powered real-time Indian Sign Language (ISL) recognition platform.**  
*Department of Computer Science & Engineering — SRM University*

</div>

---

## 📋 Overview

**SignSense AI** is a real-time Indian Sign Language (ISL) recognition system powered by deep learning and computer vision. The system captures live video from any standard webcam, extracts 258 skeletal & hand landmarks using Google MediaPipe, and classifies gestures into 49 distinct ISL vocabulary classes using a stacked 2-layer LSTM recurrent neural network.

The platform includes an intuitive, high-performance web application featuring real-time detection overlays, sliding-window voting stabilization, session analytics, and dynamic sentence aggregation.

---

## 🎯 Key Highlights

- **49 Dynamic ISL Classes**: Comprehensive vocabulary covering daily greetings, verbs, requests, questions, and common expressions.
- **High Test Accuracy**: **98.48%** overall accuracy with a **0.98 Macro F1 score**.
- **Real-Time 30 FPS Processing**: Low latency inference (~33ms on GPU / ~80ms on CPU).
- **258 Keypoint Input**: Holistically extracts 33 pose landmarks (with visibility) and 21 landmarks for each hand (x, y, z).
- **Temporal Voting Stabilization**: 20-frame majority voting buffer prevents false positives and jitter.
- **Cyber Midnight UI**: Modern dark theme with real-time HUD overlays, landmark visualizer, and Chart.js analytics.

---

## 🏗️ Architecture & Pipeline

```
  ┌──────────────┐      ┌─────────────────────────┐      ┌─────────────────────────┐
  │ Webcam Input │ ───> │  MediaPipe Holistic     │ ───> │ Normalization & EMA     │
  │   (30 FPS)   │      │  (258 3D Keypoints)     │      │ Temporal Smoothing (α)  │
  └──────────────┘      └─────────────────────────┘      └─────────────────────────┘
                                                                      │
                                                                      ▼
  ┌──────────────┐      ┌─────────────────────────┐      ┌─────────────────────────┐
  │ Live Web UI  │ <─── │ Voting Buffer (12/20)   │ <─── │ 2-Layer LSTM Network    │
  │ Sentence HUD │      │ Prediction Stabilization│      │ (128 Hidden, 49 Classes)│
  └──────────────┘      └─────────────────────────┘      └─────────────────────────┘
```

### LSTM Model Specifications
- **Input Dimensions**: 258 keypoints per frame (33 pose × 4 + 21 left hand × 3 + 21 right hand × 3)
- **Sequence Length**: 30 consecutive frames
- **Hidden Units**: 128 (2 stacked LSTM layers)
- **Regularization**: Dropout ($p=0.5$)
- **Output Classes**: 49 gesture classes with Softmax probability distribution

---

## 🛠️ Technology Stack & Requirements

| Component | Technologies & Versions |
|---|---|
| **Python Runtime** | Python 3.12 (Recommended: 3.12.x or 3.11.x) |
| **Deep Learning Framework** | PyTorch (`torch>=2.2.0`) |
| **Computer Vision & Tracking** | MediaPipe Holistic (`mediapipe==0.10.14`), OpenCV (`opencv-python==4.8.1.78`) |
| **Data Processing & ML** | NumPy (`numpy==1.26.4`), Joblib (`1.3.2`), ML-Dtypes (`0.3.2`) |
| **Backend & Web Server** | Flask (`flask==2.3.3`), Gunicorn (`21.2.0`), Werkzeug |
| **Frontend Interface** | HTML5, Tailwind CSS, Chart.js, Google Fonts (Anta, Ubuntu, JetBrains Mono) |

---

## 📁 Project Structure

```
SignSense-AI/
├── templates/
│   ├── index.html                  # Main landing page & system overview
│   ├── change-mode.html            # Mode selector (Dynamic LSTM vs Static Alphabet)
│   ├── inference.html              # Dynamic 49-gesture real-time inference interface
│   ├── analytics.html              # Dynamic session metrics & chart visualizer
│   ├── data_analytics.html         # Training dataset distribution & class metrics
│   ├── static_inference.html       # Alphabet/finger-spelling interface
│   ├── static_analytics.html       # Static session analytics
│   └── static_data_analytics.html  # Static dataset analytics
├── app.py                          # Main application entry point & CV inference engine
├── sign_lstm_best.pt               # Pre-trained PyTorch model weights (49 ISL classes)
├── requirements.txt                # Pinned library dependencies for reproduction
├── render.yaml                     # Cloud deployment configuration
└── README.md                       # Complete setup, testing, and execution guide
```

> **Main file to run:** `app.py`

---

## 🚀 Installation & Setup Guide

### 1. Prerequisites
- **Python**: Version **3.12** (or 3.11) installed on your system.
- **Hardware**: Standard webcam (built-in or USB external).
- **Git** (optional, for cloning).

---

### 2. Environment Setup

#### Step 1: Clone or Extract the Project
Extract the zip file or clone the repository:
```bash
git clone https://github.com/ArshSyed17/SignSense-AI.git
cd SignSense-AI
```

#### Step 2: Create a Virtual Environment
Using Python 3.12:

**On Windows (PowerShell / Command Prompt):**
```powershell
py -3.12 -m venv venv
.\venv\Scripts\activate
```

**On macOS / Linux:**
```bash
python3.12 -m venv venv
source venv/bin/activate
```

#### Step 3: Install Required Libraries
Install all exact dependencies using `requirements.txt`:
```bash
pip install -r requirements.txt
```

---

## 📦 Dataset & Model Placement Instructions

### Model Weights Placement
The system comes with the pre-trained deep learning model already included:
- **Model File**: `sign_lstm_best.pt`
- **Location**: Must be placed in the **root directory** of the project:
  ```
  SignSense-AI/
  ├── sign_lstm_best.pt   <-- Pre-trained weights file
  ├── app.py
  └── ...
  ```
- **Size**: ~1.29 MB (1,352,501 bytes).
- When `app.py` runs, it automatically scans for `sign_lstm_best.pt` in the working directory and loads it onto CPU/GPU.

### Dataset Information
The model was trained on the standardized Indian Sign Language (ISL) landmark dataset:
- **Total Samples**: 3,279 video sequences
- **Training Split**: 2,623 sequences (80%)
- **Testing Split**: 656 sequences (20%)
- **Classes**: 49 gesture categories (greetings, identity, action verbs, and questions).
- The dataset feature statistics, class distributions, and F1 performance per class are built directly into the web application under the **Dataset Analytics** page (`/dynamic/dataset-analytics`).

---

## 💻 Commands to Run, Test, and Evaluate

### 1. Run the Main Application
To start the live recognition web server:
```bash
python app.py
```
*(Or directly using the virtual environment: `.\venv\Scripts\python.exe app.py`)*

### 2. Access the Application
Open any modern web browser and navigate to:
```
http://localhost:5000
```
- Click **"Inference"** in the navigation bar or choose **Dynamic Mode**.
- Click **"Turn On"** to activate your webcam.
- Perform any of the 49 ISL signs in front of the camera to see real-time landmark tracking, predicted labels, confidence percentages, and sentence building.

### 3. Verify / Test Model & Dependencies
To test if all libraries and the model load properly without starting the web server, run this verification command:
```bash
python -c "import torch, mediapipe, cv2, numpy, flask; print('Libraries: OK'); model = torch.load('sign_lstm_best.pt', map_location='cpu', weights_only=False); print('Model load: SUCCESS')"
```

### 4. Health Check Endpoint
While the server is running, you can test the backend health and model status via curl or browser:
```bash
curl http://localhost:5000/api/health
```
**Expected Response:**
```json
{
  "status": "healthy",
  "dynamic_model_loaded": true,
  "mediapipe_available": true,
  "dynamic_camera_active": false
}
```

---

## 📊 Performance Metrics

| Evaluation Metric | Score |
|---|---|
| **Overall Test Accuracy** | **98.48%** |
| **Macro Precision** | **0.99** |
| **Macro Recall** | **0.98** |
| **Macro F1 Score** | **0.98** |
| **Weighted F1 Score** | **0.98** |
| **GPU Inference Speed** | ~33 ms per frame (~30 FPS) |
| **CPU Inference Speed** | ~80–100 ms per frame |

---

## 👥 Project Team

| Name | Registration Number | Department & Section | Project Role |
|---|---|---|---|
| **Arsh Syed** | `AP23110010587` | B.Tech CSE — Section D | Deep Learning & Model Architecture |
| **Surya Teja Annam** | `AP23110010525` | B.Tech CSE — Section D | Real-Time Pipeline & Full-Stack Integration |

- **Institution**: Department of Computer Science & Engineering, SRM University  
- **Course**: B.Tech CSE Deep Learning Project

---

## 📄 License & Attribution

Developed for academic demonstration and research purposes under the **B.Tech Computer Science and Engineering** curriculum at **SRM University**.

---

<div align="center">

**© 2026 SignSense AI** — Built with ❤️ for the Indian Sign Language community.

</div>
