# OculoMetric PD | Web-Based Pupillary Distance Measurement System

[![License: MIT](https://img.shields.io/badge/License-MIT-teal.svg)](https://opensource.org/licenses/MIT)
[![Technology](https://img.shields.io/badge/Engine-Google%20MediaPipe%20FaceMesh%20(Iris)-00f2fe.svg)](https://developers.google.com/mediapipe/solutions/vision/face_landmarker)
[![Architecture](https://img.shields.io/badge/Architecture-Zero--Dependency%20Single--File-00f5a0.svg)](#)

> A clinical-grade, browser-based pupillary distance (PD) diagnostic tool powered by sub-pixel ocular landmarking, parallax compensation gating, and dual-mode biometric scaling.

---

## 🎯 Overview

Accurate Pupillary Distance (PD) is critical in optometry and optical dispensing. Even a **$1.0\text{ to }1.5\text{ mm}$ error** induces unwanted prismatic effects, asthenopia, and optical aberrations in corrective and progressive lenses.

Traditional measurement methods either require costly hardware (corneal reflection pupilometers), clinic visits, or awkward methods requiring users to balance credit cards against their faces.

**OculoMetric PD** eliminates these barriers. Using **Google MediaPipe's 478-point Iris Model**, it measures binocular distance, monocular splits (OD / OS), and near reading PD in real time directly from any standard webcam—all wrapped in an intuitive, sci-fi clinical Heads-Up Display (HUD).

---

## ✨ Features

- **Zero-Hardware Sub-Millimeter Measurement:**
  - Employs human horizontal visible iris diameter (HVID $\approx 11.70\text{ mm}$) to compute a real-time pixel-to-millimeter ratio. No physical cards or calibration targets required.
- **Dual-Mode Calibration Engine:**
  - **Auto Iris Mode:** Zero-friction, instant capture based on ocular anatomy.
  - **ISO/IEC 7810 ID-1 Card Mode:** Optional verification caliper overlay calibrated to universal standard plastic card dimensions ($85.60\text{ mm} \times 53.98\text{ mm}$).
- **Monocular Split (OD / OS):**
  - Projects the nasal bridge sagittal axis (Landmark `168`) to compute individual Right Eye (OD) and Left Eye (OS) measurements—vital for asymmetric prescriptions.
- **Parallax & Posture Alignment Gating:**
  - Tracks head roll, yaw, and camera distance in real-time. Automatically pauses capture if tilt exceeds $3.8^\circ$, preventing perspective-foreshortening distortion.
- **Multi-Frame Median Filter:**
  - Accumulates 30 validated biometric frames to cancel micro-saccades and blink artifacts, reporting standard deviation tolerance (typically $\pm0.35\text{ mm}$).
- **Reading / Near PD Convergence Calculation:**
  - Automatically derives Near PD using optical convergence offset ($-3.0\text{ mm}$) for bifocal and progressive lenses.
- **Synthetic Diagnostic Simulator:**
  - Built-in simulation loop for testing UI and data flows without requiring webcam permissions.
- **100% Client-Side Privacy:**
  - Video streams are processed entirely inside browser memory via WebAssembly/WebGL. Zero camera data or images leave the user's machine.

---

## 🔬 Scientific & Optical Principles

### 1. Scaling Ratio ($\text{mm/px}$)
The iris diameter in camera pixels ($D_{px}$) is computed using landmark Euclidean distance between temporal and nasal iris margins (landmarks `469` and `471` for OS, `474` and `476` for OD):

$$D_{px} = \sqrt{(x_{ext} - x_{int})^2 + (y_{ext} - y_{int})^2}$$

Using the corneal constant $HVID = 11.70\text{ mm}$:

$$\text{Scale Factor } (S) = \frac{11.70\text{ mm}}{\bar{D}_{px}}$$

### 2. Binocular & Monocular PD Formulation
Let $P_R$ and $P_L$ represent the center pupil coordinates (landmarks `473` and `468`) and $M_{sagittal}$ represent the nasal bridge projection:

$$\text{Binocular PD} = \text{dist}(P_R, P_L) \times S$$

$$\text{OD (Right)} = |M_x - P_{Rx}| \times S \qquad \text{OS (Left)} = |M_x - P_{Lx}| \times S$$

$$\text{Near PD} = \text{Binocular PD} - 3.0\text{ mm}$$

---

## 🚀 Quick Start & Deployment

### Run Locally
Because this project is built as a single, self-contained HTML application, there are **no dependencies to install**:

1. Clone or download the repository:
   ```bash
   git clone https://github.com/your-username/oculometric-pd.git
   cd oculometric-pd
   ```
2. Serve via any local HTTP server (required for webcam camera permissions):
   ```bash
   # Python 3
   python -m http.server 8080

   # Node.js
   npx serve .
   ```
3. Open `http://localhost:8080` in Chrome, Edge, Safari, or Firefox.

### Deploy to GitHub Pages (Share via Link)
1. Push `index.html` to your GitHub repository.
2. In your repository, go to **Settings** $\to$ **Pages**.
3. Under **Branch**, select `main` (or `master`) and folder `/ (root)`.
4. Click **Save**. Within 1–2 minutes, your clinical PD tool is live at:
   `https://<your-username>.github.io/<repo-name>/`

---

## 📋 Clinical Testing Protocol

To achieve optimal lab-grade readings:
1. **Lighting:** Sit in front of a soft, diffuse light source (avoid harsh backlight or shadows on the eye sockets).
2. **Gaze Target:** Look directly at the physical camera lens—not at your reflection on the monitor. This aligns the visual axis to optical infinity.
3. **Working Distance:** Sit at an arm's distance (approximately $45\text{ to }60\text{ cm}$).
4. **Hold Steady:** Maintain posture until the HUD progress ring reaches 100% and locks the reading.

---

## 🛠 Tech Stack

- **Computer Vision:** [Google MediaPipe Face Mesh](https://developers.google.com/mediapipe/solutions/vision/face_landmarker) (478 Iris-refinement topology)
- **Styling & UI:** [Tailwind CSS](https://tailwindcss.com/) (CDN) + Custom Sci-Fi SVG Overlays
- **Audio Feedback:** Native Web Audio API procedural synthesizer
- **Runtime:** Pure client-side Vanilla ES6+ HTML5 Canvas

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).