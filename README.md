# Exam Proctoring System

This project is a **real-time exam proctoring system** using audio and face verification. It allows recording voices and verifying faces during exams to prevent cheating.

---

## Prerequisites

* Python 3.11
* Git
* [ffmpeg](https://www.gyan.dev/ffmpeg/builds/) install (ffmpeg-git-essentials.7z) and add to PATH 

---

## Installation

1. **Clone the repository**

```bash
[git clone https://github.com/Sama-Khaled-Ibrahim/Cheating-Detection-System.git]
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
pip install -r requirements.txt --use-deprecated=legacy-resolver
```

>  Make sure you have the correct version of Python (3.11) to avoid dependency conflicts.

---

##  Running the Project

1. **Start the Flask server**

```bash
python app.py
```

2. **Open your browser** at:

```
http://127.0.0.1:5000
```

3. **Test the functionality**


---

##  Notes

* **Database**: The project uses `database.db` (SQLite). If you want to reset, delete the file and rerun the app.
* **Uploads**: `static/faces/` and `static/voices/` are ignored in Git.

---

##  Tips

* Always activate the virtual environment before running any Python scripts.
* If you encounter errors related to **NumPy or TensorFlow**, ensure your virtual environment has Python 3.11 and `numpy<2`.
## My Contributions

As a member of the Computer Vision team, I contributed to the design and implementation of the proctoring system’s visual monitoring pipeline.

### Computer Vision

- Developed the pose-estimation and student-monitoring modules using MediaPipe.
- Implemented facial landmark tracking to verify that the student remained properly positioned and visible within the camera frame.
- Developed a YOLOv8-based person-detection model, trained with the AdamW optimizer, to detect the presence of additional individuals during an exam session.
- Implemented pose-based analysis using MediaPipe to identify excessive leaning and abnormal body positioning.
- Developed a hand-tracking component using MediaPipe Hands to detect potentially suspicious hand gestures or signing behavior.
- Evaluated and experimented with multiple computer-vision approaches before selecting and integrating the final models used in the system.

### Web Application and Backend

In addition to the computer-vision pipeline, I contributed to the development of the web application and backend as part of the wider team effort.

My contributions included:

- Implementing student registration and database integration.
- Developing the question-randomization functionality used during exams.
- Building and integrating several pages of the web application.
- Contributing to additional backend logic and system features required for the final application.
---

