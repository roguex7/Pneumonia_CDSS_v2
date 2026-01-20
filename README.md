# 🫁 Pneumonia Detection System v2.0 (CDSS)

> **A Next-Generation Clinical Decision Support System for Real-Time X-Ray Analysis.**

<img width="1920" height="2450" alt="image" src="https://github.com/user-attachments/assets/fbc526c0-6ebb-4cdf-b127-c83da022376a" />

## 📋 Executive Summary

**Pneumonia_CDSS_v2** is a production-ready AI solution designed to assist radiologists in detecting pulmonary opacities.

While **v1.0** was a successful prototype proving the concept using YOLOv5, **v2.0** is a complete architectural rewrite focusing on **clinical deployment**. By migrating to the **YOLO26 Nano** architecture and engineering a **balanced dataset**, we achieved a **98% reduction in training overhead** and a **10x improvement in inference speed**, making this system viable for low-resource edge devices and cloud servers.

---

## 🚀 V1.0 vs. V2.0: The Performance Leap

The transition from Version 1 to Version 2 was driven by the need to solve critical bottlenecks in training time and deployment stability.
<img width="766" height="628" alt="image" src="https://github.com/user-attachments/assets/bf7b96f1-2b7a-4716-9139-e6ccfbd4d66b" />

| Feature | 🛑 Version 1.0 (Legacy) | ✅ Version 2.0 (Production) | 📈 Impact / Improvement |
| :--- | :--- | :--- | :--- |
| **Training Time** | **13.4 Hours** (RTX 2050) | **5.9 Hours** (RTX 2050) | **56% Faster** convergence due to Nano arch & RAM caching. |
| **Model Architecture** | YOLOv5 | **YOLO26 Nano** | **Ultra-lightweight** architecture optimized for edge devices. |
| **Inference Speed** | ~15 ms per image | **3.9 ms per image** | **3.8x Speedup** enabling real-time video analysis. |
| **Model Size** | ~14 MB | **5.2 MB** | **63% Smaller**, instant cloud loading without Git LFS. |
| **Dataset Strategy** | Raw Imbalanced | **Balanced Subset (5k)** | **Reduced Bias**: 1:1 split prevents false positives. |

<img width="3000" height="1800" alt="v1_vs_v2_comparison" src="https://github.com/user-attachments/assets/83df47ae-1399-463f-8a77-68937f23e28b" />

---

## 🏗️ System Architecture

The v2.0 pipeline consists of three distinct stages designed for transparency and speed.

### 1. Data Engineering (The "Goldilocks" Split)

Instead of overwhelming the model with noisy data, we curated a **"Goldilocks" dataset** of **5,000 images**:

* **2,500 Positive:** Confirmed Pneumonia Opacities.
* **2,500 Negative:** Normal/No Opacity.
* **Result:** A model that learns *features*, not class probability bias.

### 2. The AI Core (YOLO26 Nano)

We utilized the **Ultralytics YOLO26 Nano** backbone.

* **Resolution:** 640x640 pixels.
* **Precision:** Automatic Mixed Precision (AMP) enabled.
* **Hardware Optimization:** Trained with `workers=0` to bypass Windows/CUDA threading conflicts on RTX GPUs.

### 3. The Clinical Presentation Layer

The inference results are not just drawn on the screen; they are structured into:

* **Visual Layer:** Bounding boxes with confidence scores.
* 
**Data Layer (Pandas):** Precise `xmin, ymin, xmax, ymax` coordinates for developers.


* **Report Layer (FPDF):** A generated PDF for doctors.

graph LR
    A[User Upload] -->|X-Ray Image| B(Preprocessing)
    
    subgraph "Preprocessing"
    B --> C{Resize 640px}
    C --> D[Normalize]
    end
    
    D -->|Tensor| E[YOLO26 Nano Model]
    
    subgraph "Inference Engine"
    E -->|Raw Detections| F[NMS Filter]
    end
    
    F -->|Bbox + Conf| G[Post-Processing]
    
    subgraph "Output Generation"
    G --> H[Pandas DataFrame]
    G --> I[FPDF Generator]
    end
    
    H --> J[CSV Export]
    I --> K[PDF Medical Report]
    
    style E fill:#f9f,stroke:#333,stroke-width:4px
    style K fill:#bbf,stroke:#333,stroke-width:2px

---

## 📊 Performance Metrics

**Training Environment:** NVIDIA GeForce RTX 2050 (4GB VRAM) | 16GB RAM | Python 3.12

<img width="666" height="302" alt="image" src="https://github.com/user-attachments/assets/26cebf8a-e565-4478-b9f5-2f5fa7adb53e" />

<img width="2400" height="1200" alt="results" src="https://github.com/user-attachments/assets/3fd6176a-3c9f-4590-b256-2207ce7f3f24" />

---

## 💻 Features & Interface

### 🖥️ Real-Time Analysis

* Drag-and-drop interface for rapid X-ray screening.
* Adjustable **Sensitivity Threshold** slider (0.0 - 1.0) to filter weak detections.

<img width="1521" height="755" alt="image" src="https://github.com/user-attachments/assets/218e3641-14aa-49d8-b070-859dc2135a29" />

### 📄 Automated Medical Reporting

* **PDF Report:** Generates a timestamped, non-editable report with "Findings" and "Clinical Impression" sections.
* **CSV Export:** Download raw analysis data for hospital audits or research.

<img width="1557" height="861" alt="image" src="https://github.com/user-attachments/assets/a0f4da3a-db21-4367-844e-769702de8f4b" />

---

## 🔧 Installation & Usage

### Prerequisites

* Python 3.10 or higher.
* (Optional) NVIDIA GPU for faster inference.

### 1. Clone the Repository

```bash
git clone https://github.com/roguex7/Pneumonia_CDSS_v2.git
cd Pneumonia_CDSS_v2

```

### 2. Install Dependencies

**Note:** We use `opencv-python-headless` to ensure compatibility with cloud environments (Linux servers).

```bash
pip install -r requirements.txt

```

### 3. Launch the Application

```bash
streamlit run app.py

```

---

## ☁️ Deployment Notes

This project is optimized for **Streamlit Cloud**.

* 
**Configuration:** The `requirements.txt` includes `opencv-python-headless` to avoid the common `libGL.so.1` error on Linux servers.


* **Model Loading:** The model file `yolo26n_v2.pt` is small enough (5MB) to be hosted directly on GitHub, eliminating the need for Git LFS.

<img width="1920" height="2450" alt="image" src="https://github.com/user-attachments/assets/0a80c9bc-d022-4201-b8b7-4343f829ef11" />

---

## 👨‍💻 Credits

* 
**Developer:** Annant R Gautam 
* **Dataset Source:** RSNA Pneumonia Detection Challenge
* **Frameworks:** Ultralytics (YOLO), Streamlit, PyTorch
