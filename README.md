# Simba's Surveillance

A wildlife surveillance system that uses computer vision to detect animals/people near a camera and logs captures to a web dashboard. Also has an interactive 3D museum showcasing endangered species built with Three.js.

Built for our CS class project.

---

## What it does

- **Live detection feed** — OpenCV HOG detector watches a webcam and uploads a screenshot + timestamp to the Flask backend whenever something is detected
- **Web dashboard** — React frontend polls the backend and displays all detections in a table with the captured image and time
- **Interactive museum** — a separate Three.js page where you can explore 3D models of endangered animals (elephants, rhinos, antelopes, etc.)

---

## Project structure

```
simbassurveillance/
├── frontend/          # React app (dashboard + museum)
│   ├── public/
│   │   ├── museum.html / museum.js / museum.css   # Three.js museum page
│   │   ├── about.html / about.js / about.css      # About page
│   │   └── assets/    # Images (sky, logo, simba)
│   └── src/
│       └── App.js     # Main dashboard component
├── backend/
│   ├── python/        # Flask API + OpenCV detector
│   │   ├── app.py         # Flask server (receives + serves detections)
│   │   ├── detector.py    # HOG person detector, sends screenshots to Flask
│   │   ├── detectorV4.py  # Updated detector version
│   │   ├── video_stream.py
│   │   └── requirements.txt
│   ├── node/          # Node.js backend (alternative/supplementary)
│   ├── backendServer.c
│   ├── cameraCode.c
│   └── concurrentTCPServer.c
```

---

## Setup

### Prerequisites

- Node.js + npm
- Python 3.x
- A webcam

### Frontend

```bash
cd frontend
npm install
npm start
```

Runs on `http://localhost:3000`

### Backend (Python/Flask)

```bash
cd backend/python
pip install -r requirements.txt
python app.py
```

Runs on `http://localhost:8080`

### Detector

In a separate terminal:

```bash
cd backend/python
python detector.py
```

This opens the webcam and starts detecting. When something is detected, it sends a screenshot to the Flask server. The webapp will update automatically.

---

## How it works

1. `detector.py` reads frames from the webcam using OpenCV
2. It runs the HOG people detector on each frame
3. If a detection confidence exceeds the threshold (default 0.80) and the cooldown period (10s) has passed, it saves a screenshot and POSTs it to `http://localhost:8080/upload-screenshot`
4. `app.py` receives the image, base64-encodes it, and stores it in memory
5. The React frontend polls `/get-detections` every second and renders the table

---

## Museum

Navigate to `/museum.html` from the dashboard nav bar. Uses Three.js + GLTF models to render an interactive 3D scene with endangered species.

---

## Tech stack

| Layer | Tech |
|---|---|
| Frontend | React, Three.js, Framer Motion, Tailwind |
| Backend | Python, Flask, Flask-CORS |
| Computer Vision | OpenCV (HOG descriptor) |
| Other | C TCP server (experimental) |
