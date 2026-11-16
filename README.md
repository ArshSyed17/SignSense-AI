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
- **Real-Time 30 FPS Processing**: Low latency inference (~33ms GPU / ~80ms CPU).
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
- **Hidden Units**: 128 (2 stacked bidirectional/standard LSTM layers)
- **Regularization**: Dropout (p=0.5)
- **Output Classes**: 49 gesture classes + Softmax distribution

---

## 🛠️ Technology Stack

| Component | Technologies |
|---|---|
| **Deep Learning Framework** | PyTorch (`torch>=2.2.0`) |
| **Computer Vision & Tracking** | MediaPipe Holistic (`0.10.14`), OpenCV (`4.8+`) |
| **Data Processing & Smoothing** | NumPy (`1.26.4`), SciPy, Joblib |
| **Backend & Web Server** | Flask (`2.3.3`), Werkzeug, Gunicorn |
| **Frontend Interface** | HTML5, Tailwind CSS, Chart.js, JetBrains Mono, Anta |

---

## 📁 Project Structure

```
SignSense-AI/
├── templates/
│   ├── index.html                  # Landing page & project showcase
│   ├── change-mode.html            # Recognition mode selector
│   ├── inference.html              # Dynamic 49-gesture real-time inference
│   ├── analytics.html              # Dynamic session metrics & chart logs
│   ├── data_analytics.html         # Training dataset distribution & F1 scores
│   ├── static_inference.html       # Alphabet/finger-spelling interface
│   ├── static_analytics.html       # Static session analytics
│   └── static_data_analytics.html  # Static dataset analytics
├── app.py                          # Unified Flask backend & CV inference engine
├── sign_lstm_best.pt               # Trained PyTorch LSTM weights (49 ISL classes)
├── requirements.txt                # Python package dependencies
├── render.yaml                     # Cloud deployment configuration
└── README.md                       # Documentation
```

---

## 🚀 Getting Started & Installation

### Prerequisites
- Python 3.11 or Python 3.12 installed
- Standard built-in or external webcam

### Step 1: Clone the Repository
```bash
git clone https://github.com/ArshSyed17/SignSense-AI.git
cd SignSense-AI
```

### Step 2: Create and Activate Virtual Environment
**On Windows:**
```powershell
py -3.12 -m venv venv
.\venv\Scripts\activate
```

**On macOS / Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

### Step 3: Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 4: Run the Application
```bash
python app.py
```

### Step 5: Open in Browser
Open your browser and navigate to:
```
http://localhost:5000
```

---

## 📊 Dataset & Class Distribution

The LSTM model was trained on **3,279** verified ISL gesture sequences across 49 categories:

| Dataset Split | Samples | Percentage |
|---|---|---|
| **Training Set** | 2,623 sequences | 80% |
| **Validation / Test Set** | 656 sequences | 20% |
| **Total** | **3,279 sequences** | **100%** |

### Gesture Categories:
- **Greetings & Basics** (12 classes): *Hello, Bye, Thank You, Welcome, Sorry, Good, Morning, Night, etc.*
- **Identity & People** (10 classes): *I / Me, You, My, Your, Name, Who, Teacher, Student, etc.*
- **Actions & Verbs** (11 classes): *Do, Write, Read, Study, Look, Come, Help, Stand, Walk, etc.*
- **Questions & Places** (16 classes): *Home, Work, Class, Here, Now, What, How, Where, When, Why, etc.*

---

## 👥 Project Team

| Name | Registration Number | Department & Section | Project Role |
|---|---|---|---|
| **Arsh Syed** | `AP23110010587` | B.Tech CSE — Section D | Deep Learning & Model Architecture |
| **Surya Teja Annam** | `AP23110010525` | B.Tech CSE — Section D | Real-Time Pipeline & Full-Stack Integration |

**Institution**: Department of Computer Science & Engineering, SRM University  
**Academic Project**: B.Tech CSE Deep Learning Project

---

## 📄 License & Attribution

This project is developed for educational and research purposes under the **B.Tech Computer Science & Engineering** curriculum at **SRM University**.

---

<div align="center">

**© 2026 SignSense AI** — Built with ❤️ for the Indian Sign Language community.

</div>
