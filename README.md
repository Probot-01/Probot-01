<h1 align="center">Mohammad Saad Inamdar</h1>

<p align="center">
  Computer vision · Medical imaging · Model evaluation<br/>
  B.Tech CSE (Honours by Research) · Sardar Patel Institute of Technology, Mumbai
</p>

<p align="center">
  <a href="mailto:saad.inamdar24@spit.ac.in"><img src="https://img.shields.io/badge/Email-saad.inamdar24%40spit.ac.in-24292F?style=flat-square&logo=gmail&logoColor=white" alt="Email"/></a>
  <img src="https://img.shields.io/badge/Based_in-Mumbai%2C_India-24292F?style=flat-square" alt="Mumbai, India"/>
</p>

---

### About

I'm a third-year Computer Science undergraduate on the research track at SPIT. I build computer vision systems, and then I spend a lot of time checking whether I can trust them. That means reproducing a model's outputs from raw pixels, testing whether its confidence scores are calibrated, and looking for inputs that make it fail without any warning.

My recent work covers two areas: real-time head-pose estimation for screen privacy, and a diabetic retinopathy screening pipeline for rural primary health centres.

### Current focus

- **Medical image analysis:** fundus segmentation and grading, and deciding which cases need a human to review them
- **Calibration and uncertainty:** temperature scaling, split conformal prediction, and triage built on calibrated scores
- **Real-time vision on limited compute:** detection and pose pipelines that run on a CPU without a GPU
- **Vision for human-computer interaction:** systems that infer what a person intends to do, as well as whether they are present

---

### Research

**Real-Time Intent-Based Shoulder Surfing Detection Using Webcam-Driven Head Pose Estimation and Temporal Threat Reasoning**<br/>
M. S. Inamdar, N. Jain, V. Hole: *manuscript in preparation*, 2026 · [code](https://github.com/Probot-01/Shoulder-Surfer-Detection)

### Experience

| Role | Organisation | Period |
| :-- | :-- | :-- |
| System Administrator (Fellowship) | GPU-SPIT, Sardar Patel Institute of Technology | Aug 2026 – present |
| Undergraduate Researcher (Supervisor: Prof. Varsha Hole) | Dept. of CSE, Sardar Patel Institute of Technology | Apr 2026 – May 2026 |
| Vice Chairperson (Finance) | Enactus SPIT | Jul 2026 – present |

At GPU-SPIT, I help run the institute's Slurm-based GPU cluster, which has more than 100 users. The work covers LDAP accounts and role-based access, CUDA modules and PyTorch environments, and job scheduling and GPU allocation.

---

### Featured projects

#### 🚧 DR✦AI: Diabetic retinopathy screening for rural health centres
[`Diabetic-Retinopathy-Screening`](https://github.com/Probot-01/Diabetic-Retinopathy-Screening) (mirror of the team repo [`krrishgadekar/SIH_2026`](https://github.com/krrishgadekar/SIH_2026)) · Smart India Hackathon 2026 · team project · **work in progress**

A screening platform that connects rural primary health centres to ophthalmologists who review cases remotely. I work on the **backend and the ML pipeline**.

- **Dual-branch grading:** an EfficientNet-B0 classifier runs alongside a rule engine based on the ICDR grading scale. The rule engine is driven by four U-Net segmentation models (vessels, optic disc/fovea, bright lesions, red lesions) that the team trained. When the two branches disagree, the case goes to human review. Those flagged cases contained classifier errors at 1.37× the rate of a random sample of the same size.
- **Reproducibility audit:** I reproduced the published outputs of all five trained models from raw images. The segmentation masks matched pixel for pixel, 77 of 78 landmarks matched, and Dice scores matched to four decimal places. The audit also exposed silent preprocessing bugs. A resize-interpolation mismatch alone had cut agreement from 98.7% to 28.2%.
- **Calibration and triage:** temperature scaling reduced expected calibration error from 0.102 to 0.055. Split conformal prediction then sorts cases into three triage tiers.
- **Failure analysis:** on a separate fundus test set, the image-quality gate silently rejected heavily compressed images. After I re-tuned it, the gate kept 88.5% of the good images it had been discarding.

`Python` `PyTorch` `OpenCV` `MATLAB` `Node.js / Express` `PostgreSQL`

#### Shoulder-surfing detection with head-pose estimation
[`Shoulder-Surfer-Detection`](https://github.com/Probot-01/Shoulder-Surfer-Detection) · research project · manuscript in preparation

A webcam system that detects when someone is actually **reading your screen**, not just standing nearby.

- **Pipeline:** YOLOv8n detects people in the frame, the system separates the user from observers, MediaPipe extracts each face, and a MobileNetV2 classifier estimates head pose.
- **Why head pose:** at 1–2 m, an iris covers only about 4–8 px in a 640×480 frame, so iris-based gaze tracking is unreliable at webcam resolution.
- **Head-pose classifier:** a small classifier head on a frozen, ImageNet-pretrained MobileNetV2, trained on the BIWI head-pose dataset. It reached 91.99% accuracy on a balanced validation set.
- **Fewer false alarms:** asymmetric confidence thresholds filter out ambiguous head poses. A hysteresis state machine raises an alert only after several consecutive confident frames from the same observer. An ablation study confirmed the improvement.
- **End-to-end results:** frame-level accuracy of 96.50%, F1 of 91.36%, precision of 90.24% and recall of 92.50%, measured across more than 50 real-world scenarios with one to ten people in frame. Profiling and frame-skip caching raised throughput from 30–40 FPS to 55–65 FPS on a CPU with no GPU.

`Python` `PyTorch` `OpenCV` `YOLOv8` `MediaPipe` `MobileNetV2`

#### Voice-based Type 2 diabetes risk prediction
[`Diabetes-Detection`](https://github.com/Probot-01/Diabetes-Detection) · research prototype

Tests whether acoustic features of the voice carry a signal for Type 2 diabetes.

- **Features:** 267 acoustic features, including MFCCs and their deltas, spectral features, LPC, jitter and shimmer, extracted with librosa and Parselmouth.
- **Models:** XGBoost, with SMOTE applied only to the training split, plus gender-stratified variants and variants that add BMI. A Gradio app runs inference on uploaded audio.
- **What the evaluation showed:** voice features alone were weak, with a held-out AUC of 0.59. Stratifying by gender raised the scores sharply. The README documents this as possibly reflecting demographic confounding rather than acoustic signal, and notes that there was no external validation.

`Python` `XGBoost` `scikit-learn` `librosa` `Parselmouth` `Gradio`

#### Slate: handwriting-to-structure on an infinite canvas
[`Slate`](https://github.com/Probot-01/Slate)

Turns handwritten ink on a tldraw canvas into editable Markdown using a vision-language model, and measures where the latency comes from.

- **Region of interest:** when the user switches to a new thought, the system detects the spatial jump and clusters strokes with union-find. It then crops only the cluster containing the most recent stroke, rather than sending the whole canvas.
- **Instrumentation:** a Fastify server streams responses over SSE. Every request is traced in stages: capture, dispatch, time to first byte, streaming and render.
- **Latency experiment:** a 135-request experiment across three image resolutions showed that the model provider's time to first byte dominates latency. Client-side work was under 2% of median end-to-end time.

`TypeScript` `React` `tldraw` `Fastify` `Zod` `Gemini API` `Python`

#### ReconAI: payment reconciliation engine
[`Payment-Reconciliation-System`](https://github.com/Probot-01/Payment-Reconciliation-System)

Matches incoming payment transactions to expected orders and flags discrepancies.

- **Matching engine:** seven deterministic passes that detect duplicates, exact, partial and delayed matches, and unmatched records. The rules use configurable amount tolerances and time windows, and amounts are stored in integer paise.
- **Confidence scoring:** payments with no ID link are matched by how close the amount and timing are. Each match gets a confidence score, with configurable thresholds for automatic matching versus manual review.
- **Application:** CSV import of transactions, JWT authentication, and a React dashboard for reconciliation status.

`TypeScript` `Express` `Prisma` `SQLite` `React` `Vite`

<details>
<summary><b>Other work</b></summary>
<br/>

- [`Civic-Connect`](https://github.com/Probot-01/Civic-Connect): a team hackathon project for reporting civic issues. It has three React + TypeScript portals (citizen, municipal admin, field supervisor) with map-based reporting and ward-level analytics.
- [`Hospital_Employee_Management`](https://github.com/Probot-01/Hospital_Employee_Management): a coursework project with a REST API built on Node.js, Express and MySQL, and a plain HTML/CSS/JS front end.

</details>

---

### Tools

<p>
  <img src="https://skillicons.dev/icons?i=python,pytorch,opencv,matlab,cpp,ts,nodejs,postgres,linux,git" alt="Python, PyTorch, OpenCV, MATLAB, C++, TypeScript, Node.js, PostgreSQL, Linux, Git"/>
</p>

| Area | Tools |
| :-- | :-- |
| **Languages** | Python, C++, Java, TypeScript, JavaScript, SQL, MATLAB |
| **Computer vision and ML** | PyTorch, OpenCV, MediaPipe, YOLOv8, U-Net, EfficientNet, MobileNet, XGBoost, scikit-learn |
| **Evaluation and uncertainty** | Temperature scaling, split conformal prediction, Grad-CAM, ablation studies |
| **Audio** | librosa, Parselmouth (Praat) |
| **Backend and data** | Node.js, Express, Fastify, Prisma, PostgreSQL, MySQL, SQLite |
| **Frontend** | React, Next.js, Vite, Tailwind CSS |
| **Systems** | Linux, Slurm, CUDA, LDAP, Git |

---

### Recognition

- **Smart India Hackathon 2026:** placed 4th of more than 150 teams in the institute round and was selected for the national round (DR✦AI)

### Education

**B.Tech, Computer Science and Engineering**, Sardar Patel Institute of Technology (University of Mumbai), 2024 – 2028 (expected)<br/>
Honours by Research track (from May 2026) · Minor in FinTech Engineering and Digital Financial Systems

---

<p align="center">
  Open to research internships in computer vision and medical imaging.<br/>
  <a href="mailto:saad.inamdar24@spit.ac.in">saad.inamdar24@spit.ac.in</a>
</p>
