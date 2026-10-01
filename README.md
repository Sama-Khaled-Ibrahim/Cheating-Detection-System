# AI-Powered Online Exam Proctoring System

A real-time, multi-modal exam proctoring platform that combines computer vision, audio analysis, and system-level monitoring to detect cheating during online exams — with automated, evidence-backed risk reports for instructors.

Built as a graduation project: a full Flask web application (exam creation, student/admin/doctor roles, exam delivery) wired to a live AI monitoring pipeline that watches the webcam feed, microphone, and browser activity while a student takes an exam.

---

## ✨ Key Features

### Identity Verification
- **Face verification (ArcFace / InsightFace):** confirms the student taking the exam matches their enrolled profile photo, both at login and continuously during the exam.
- **ID card verification:** a custom fine-tuned YOLOv8 model detects and validates national ID card fields (face, name, ID number) to cross-check student identity.
- **Two-factor authentication** and single-device session enforcement (a login on a new device invalidates the old session).

### Real-Time Webcam Monitoring
- **Forbidden object detection** — a YOLOv8 model fine-tuned to detect phones, books, smartwatches, headphones/earbuds, sunglasses, face masks, calculators, and laptops in frame.
- **Gaze & eye-tracking** (MediaPipe iris landmarks) — flags sustained off-screen gaze and abnormal blink patterns (EAR — Eye Aspect Ratio).
- **Head pose / leaning detection** — flags a student leaning out of frame or turning away for extended periods.
- **Extra-person & out-of-frame detection.**
- **Hand-sign recognition** — a trained MLP classifier flags suspicious hand gestures (e.g., signaling to someone off-camera).

### Audio Monitoring
- **Speaker verification** (SpeechBrain ECAPA embeddings) — flags voices that don't match the enrolled student.
- **Voice activity & overlap detection** (Silero VAD + Pyannote) — flags simultaneous speech from multiple people.
- **Talk-time anomaly detection** — flags excessive speech from the same person during a silent exam.

### System-Level Monitoring
- Browser tab switches, keyboard shortcuts, and window-focus loss are logged client-side and streamed to the server in real time.

### Automated Risk Scoring & Reporting
- A weighted **cheating-detection engine** aggregates webcam, audio, and system events into a single, normalized risk score per student.
- Every flagged event is saved with **evidence** (annotated screenshots + timestamped CSV logs), viewable per-student in the admin dashboard.
- Instructors get exam-level and student-level reports, plus a global "all reports" view.

### Exam Platform
- Role-based access (student / doctor / admin) with department- and level-scoped exam visibility.
- Exam creation with question banks, reusable question sets, and scheduling.
- Live exam-taking UI with integrated webcam/mic capture and pre-exam identity checks.

---

## 🏗️ Architecture

```
Browser (exam UI)
   │  webcam frames / audio chunks / tab & focus events
   ▼
Flask app (app.py)
   ├─ core/image_utils.py    → ArcFace face verification (InsightFace)
   ├─ core/audio_utils.py    → speaker embeddings, overlap/VAD analysis
   ├─ core/train_yolo_for_ID.py → ID-card object detection
   ├─ CV/ (ObjectDetection, EyeTracking, PoseEstimation) → per-frame YOLOv8 + MediaPipe analysis
   ├─ core/db.py              → SQLite persistence (users, exams, sessions)
   └─ cheating_engin/cheating_engin.py → weighted risk scoring over logged events
   ▼
static/{suspicious_events, system_events, faces, voices}/*.csv, evidence_<student>_<exam>/*
   ▼
Admin dashboard & per-student report views (templates/*.html)
```

---

## 🧠 Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python, Flask, Flask-Limiter, SQLite |
| Computer vision | YOLOv8 (Ultralytics, custom fine-tuned), MediaPipe, OpenCV, InsightFace (ArcFace) |
| Audio | SpeechBrain (ECAPA-TDNN speaker embeddings), Silero VAD, Pyannote (overlap detection), Librosa |
| ML/DL runtime | PyTorch, TensorFlow, ONNX Runtime, scikit-learn |
| Frontend | HTML/CSS/JS (Bootstrap, jQuery, Slick) |

---

## 🖥️ Prerequisites

* Python 3.11
* Git
* [ffmpeg](https://www.gyan.dev/ffmpeg/builds/) — download the essentials build and add it to your `PATH`

---

## ⚡ Installation

1. **Clone the repository**

```bash
git clone https://github.com/Sama-Khaled-Ibrahim/Cheating-Detection-System.git
cd exam-proctoring-system
```

2. **Create a virtual environment**

```bash
# Windows
py -3.11 -m venv .venv311

# Activate the environment
# PowerShell
.venv311\Scripts\Activate.ps1
# or CMD
.venv311\Scripts\activate.bat
```

3. **Install dependencies**

```bash
pip install --upgrade pip
pip install -r Newrequirements.txt --use-deprecated=legacy-resolver
```

> ⚠️ Use Python 3.11 to avoid dependency conflicts (TensorFlow/NumPy pin to `numpy<2`).

---

## 🧰 Running the Project

1. **Start the Flask server**

```bash
python app.py
```

2. **Open your browser** at:

```
http://127.0.0.1:5000
```

3. Register a student account (or use an admin/doctor account), enroll a reference photo, and start an exam to see live proctoring in action.

---

## 📝 Notes

* **Database:** the project uses `database.db` (SQLite). Delete the file and rerun the app to reset it.
* **Uploads:** `static/faces/` and `static/voices/` are git-ignored — enrolled reference photos and voice samples are stored there at runtime.
* **Evidence:** flagged events are saved under `evidence_<student_id>_<exam_id>/` with annotated screenshots and CSV logs; per-event CSVs also live under `static/suspicious_events/` and `static/system_events/`.
* **Model weights:** pretrained/fine-tuned YOLOv8 weights (`yolov8n.pt`, `trained_models/best.pt`, `CV/NewFullDataSet_FT.pt`) are loaded at startup — make sure they're present before running.

---

## 💡 Tips

* Always activate the virtual environment before running any Python scripts.
* If you hit errors related to **NumPy or TensorFlow**, confirm your virtual environment uses Python 3.11 and `numpy<2`.
* Set any Hugging Face model tokens via environment variable (e.g. `HF_TOKEN`) — do not hardcode credentials.
## My Contributions

I was part of the Computer Vision team and contributed to the design and implementation of several components of the proctoring pipeline.

### Computer Vision

- Developed the pose-estimation and student-monitoring modules using MediaPipe.
- Implemented facial landmark tracking to verify that the student remained visible and appropriately positioned within the camera frame.
- Developed and trained a YOLOv8-based person-detection model using the AdamW optimizer to detect the presence of additional individuals during an exam session.
- Implemented pose-based monitoring with MediaPipe to identify excessive leaning and abnormal body positioning.
- Developed a hand-tracking component using MediaPipe Hands to detect potentially suspicious hand gestures or signing behavior.
- Experimented with multiple computer-vision approaches and detection strategies before selecting and integrating the final models used in the system.

### Web Application and Backend

In addition to the computer-vision pipeline, I contributed to the development of the web application and backend as part of the wider team effort.

My contributions included:

- Implementing student registration and database integration.
- Developing the question-randomization functionality used during exams.
- Building and integrating several pages of the web application.
- Contributing to additional backend logic and supporting system features.
## Project Collaboration

This project was developed collaboratively as a graduation project.

This repository is a cleaned portfolio copy based on the latest team version of the project. My individual contributions are documented above, while other components and later improvements were developed collaboratively or by other members of the team.

### Project Repositories

- [Latest Team Version](https://github.com/cheating-detection-in-online-exames/FairEx)
- [Original Collaborative Repository](https://github.com/hager55m/Cheating-Detection-System)
## Team

Developed collaboratively by:

- Sama Khaled Ibrahim
- Heba Ahmed Noah
- Omnia Salah Mahmoud
- Hager Mahmoud Elshrief
- Hager Hussein Rasheed 