# FIRE-EVSim: Synthetic Fire Dataset for Electric Vehicle Charging Stations
## FIRE-EVSim: A BIM and 3D Simulation-based Framework for Enhanced Early Fire Detection in Electric Vehicle Charging Infrastructure
Minh-Truyen Do, Pa Pa Win Aung, Khoa Tran Dang Vo, Minsoo Park*, Seunghee Park∗,
\* : Coressponding Authors

Developments in the Built Environment

Vol. 26, pp. 100889, DOI: https://doi.org/10.1016/j.dibe.2026.100889

This repository provides the implementation of the **FIRE-EVSim** framework, a comprehensive and technically viable paradigm for generating high-fidelity synthetic fire data for EV charging stations using **Building Information Modeling (BIM)** and **Unreal Engine 5 (UE5)**.  
It establishes a reproducible data pipeline optimized for **deep learning-based fire detection** using **YOLO11**.

## Abstract
The expansion of electric vehicle (EV) charging infrastructure introduces unique fire safety challenges, particularly those associated with lithium-ion battery thermal runaway and high-voltage electrical faults, which differ from conventional fire scenarios. Current fire detection systems are limited by the scarcity of domain-specific training data in EV charging environments. To address this limitation, a simulation-driven data augmentation framework, Fire Incident Response Enhancement via Electric Vehicle Simulation (FIRE-EVSim), is proposed. This platform integrates real-world EV charging station configurations with advanced simulation technologies to generate large-scale synthetic fire datasets. Realistic 3D models are developed using Autodesk Revit for Building Information Modeling (BIM) and rendered in Unreal Engine 5 (UE5) to simulate diverse fire scenarios across varying intensities, environmental conditions, and temporal settings. The system produces 100 distinct fire scenarios and 10,000 annotated images capturing a wide range of fire behaviors and contextual factors. A YOLO11 deep learning model is trained using a hybrid dataset that combines the synthetic data with real fire imagery, demonstrating significantly improved detection performance. Evaluation results show a precision of 0.918, recall of 0.884, mAP@50 of 0.917, and F1-score of 0.901. The proposed vision-based approach leverages existing CCTV infrastructure to provide a low-cost and scalable platform for enhancing EV charging infrastructure safety through smart fire detection systems.

## 🔧 Frameworks and Modules  
* **3D Simulation:** Unreal Engine 5  
* **Deep Learning Framework:** PyTorch 2.2 (with CUDA 11.7)  
* **Object Detection Model:** YOLOv11 [[alif2024yolov11]](https://github.com/ultralytics/ultralytics)  
* **Data Augmentation:** Mosaic, Random Scaling, Horizontal Flipping, Brightness Adjustment  
* **Optimization Algorithm:** Stochastic Gradient Descent (SGD)  
* **Learning Scheduler:** Cosine Annealing with Warm-up  
* **Programming Language:** Python 3.10  
* **Image Processing:** OpenCV, NumPy  


## System Requirements
All experiments were conducted under the following hardware and software configuration:

## 🖥️ System Requirements  

### **Hardware Environment**  
* GPU: **NVIDIA Tesla V100** (32 GB GDDR6)  
* CUDA Version: **11.7**  
* Driver: **515.48.07**  
* Training Epochs: **300**  
* Batch Size: **16**  
* Input Resolution: **640 × 640 px**  

### **Software Environment**  
* Python: 3.10
* TensorFlow: 2.15 (with CUDA 11.7)
* scikit-learn: 1.4
* PyTorch: 2.2  
* OpenCV: 4.8 
* NumPy: 1.26 


---

## 1. Preparation

### Clone Repository
Clone or download this repository and set your working directory:
```python
MainFolder = "/your/custom/path"
```

---

## 2. Processing Steps

| Phase | Description | Tool / Software | Output |
|--------|--------------|-----------------|---------|
| **1. Data Collection** | Use real EV fire CCTV footage to design realistic 3D EV charging station models | Autodesk Revit | `.rvt` (Revit BIM model) |
| **2. 3D Simulation-based** | Import BIM models into UE5 via Datasmith; simulate fire dynamics with Niagara VFX under different lighting/weather | Unreal Engine 5 | `.mp4` / `.png` sequences |
| **3. Dataset Augmentation** | Generate annotated image sequences with bounding boxes (Fire class); apply geometric & photometric augmentations | Python (Albumentations, OpenCV) | `.json` / `.txt` / `.jpg` |
| **4. Deep Learning-based** | Train YOLO11 on synthetic + real data; evaluate generalization on DeepFire, FireNet, PublicFire | YOLOv11 | `.pt` (trained weights), `.csv` (metrics) |

---

## 3. Required Tools and Repositories

| # | Module | GitHub | Version / Commit | License |
|------|------|--------|-------------|----------|
| 1 | **Unreal Engine 5** – Simulation and rendering engine | [EpicGames/UnrealEngine](https://www.unrealengine.com/en-US/unreal-engine-5) | 5.3+ | EULA |
| 2 | **Autodesk Revit** – 3D modeling for EV station architecture | [Autodesk Revit](https://www.autodesk.com/products/revit/overview) | 2023+ | Proprietary |
| 3 | **YOLOv11** – Object Detection Framework | [ultralytics/YOLOv11](https://github.com/ultralytics/ultralytics) | v11.0 | GPL-3.0 |
| 4 | **Datasmith Exporter** – Revit to UE5 converter | [Datasmith](https://www.unrealengine.com/en-US/datasmith) | latest | EULA |
| 5 | **Albumentations** – Data augmentation | [albumentations-team/albumentations](https://github.com/albumentations-team/albumentations) | 1.4+ | MIT |

---

## 4. Directory Structure

```
FIRE-EVSim/
│
├── BIM_Revit/                # 3D EV station models (.rvt)
├── UE5_Simulation/           # Unreal Engine project (.uproject)
│   ├── Niagara_Fire/         # Fire & smoke VFX systems
│   └── Rendered_Frames/      # Rendered image sequences
│
├── Dataset/
│   ├── Images/               # Final dataset images
│   ├── Labels/               # Bounding box annotations
│   └── Metadata.csv          # Dataset summary
│
├── YOLO11_Training/
│   ├── train.py
│   ├── data.yaml
│   ├── weights/
│   └── results.csv
│
└── Docs/
    └── README.md
```

---

## 5. Example Usage

### (1) Export BIM Model
Export `.rvt` model via **Datasmith Exporter** into `.udatasmith` for import into Unreal Engine.

### (2) Simulate in Unreal Engine
Run the Niagara fire particle system and render frames using:
```
Sequencer → Render Movie → Output to /UE5_Simulation/Rendered_Frames/
```

### (3) Dataset Preparation
```python
python generate_annotations.py --input ./UE5_Simulation/Rendered_Frames/ --output ./Dataset/Labels/
```

### (4) YOLO11 Training
```bash
yolo train model=yolov11.pt data=data.yaml epochs=300 imgsz=640
```

---

## 6. Dataset Schema

| Field | Description | Example |
|--------|--------------|----------|
| `image_path` | Path to dataset image | `Dataset/Images/frame_001.png` |
| `class_id` | Class label | `0` (Fire) |
| `x1, y1, x2, y2` | Bounding box coordinates | `102, 64, 278, 198` |
| `source` | Data origin | `UE5_Simulation` / `Real_Fire` |

---

## 7. Outputs

| Type | Description | Example |
|------|--------------|----------|
| **Trained Model** | YOLOv11 weights | `best.pt` |
| **Evaluation Report** | Accuracy, mAP, Precision/Recall | `results.csv` |
| **Rendered Fire Dataset** | Multi-angle synthetic images | `/Dataset/Images/*.jpg` |

---

## 8. Citation

If you use this dataset or framework, please cite:

```bibtex
@article{do2026fire,
  title={FIRE-EVSim: A BIM and 3D simulation-based framework for enhanced early fire detection in electric vehicle charging infrastructure},
  author={Do, Minh-Truyen and Aung, Pa Pa Win and Vo, Khoa Tran Dang and Park, Minsoo and Park, Seunghee},
  journal={Developments in the Built Environment},
  pages={100889},
  year={2026},
  publisher={Elsevier}
}
```

---
## 9. Dataset Download

Due to GitHub storage limitations, the full **FIRE-EVSim synthetic fire dataset** is hosted externally.

The dataset contains **10,000 images generated from 100 simulated EV fire scenarios** created using BIM-based modeling and Unreal Engine 5 rendering.

### Download Link
Dataset is available via **Microsoft OneDrive**:

[FIRE-EVSim Dataset (OneDrive)](https://1drv.ms/f/c/c5a8c49867f477fc/IgB-e5tbHZnOTqkkgmnwE6MjAbpVkq-8ii7bA1TD-Lu6gSo?e=ReyfaK)

## Contributors
<p>
  <strong>Minh-Truyen Do∗</strong>
  <a href="https://sites.google.com/view/skkuscit" target="_blank">
    <img src="https://github.com/pms5343/Tension_aware_Wire_Tracker/raw/main/logo/skku.svg" height="20" alt="SKKU Logo"/>
  </a>
  <a href="https://scholar.google.com/citations?user=cFsaJ1OkYAsC&hl=en" target="_blank">
    <img src="https://img.shields.io/badge/-4285F4?style=flat&logo=googlescholar&logoColor=white" alt="Google Scholar"/>
  </a>
  <a href="https://github.com/TruyenDo" target="_blank">
    <img src="https://img.shields.io/badge/-000000?style=flat&logo=github&logoColor=white" alt="GitHub"/>
  </a>
</p>

<p>
  <strong>Pa Pa Win Aung</strong>
  <a href="https://www.ppwa.info/" target="_blank">
  <img src="https://github.com/pms5343/Tension_aware_Wire_Tracker/raw/main/logo/skku.svg" height="20" alt="SKKU Logo"/>
  <a href="https://scholar.google.com/citations?user=p9MXvbsAAAAJ&hl=en">
    <img src="https://img.shields.io/badge/-4285F4?style=flat&logo=googlescholar&logoColor=white" alt="Google Scholar"/>
  </a>
  <a href="https://github.com/ppglassy">
    <img src="https://img.shields.io/badge/-000000?style=flat&logo=github&logoColor=white" alt="GitHub"/>
  </a>
</p>

<p>
  <strong>Khoa Vo Dang Tran</strong>
  <a href="https://sites.google.com/view/skkuscit" target="_blank">
    <img src="https://github.com/pms5343/Tension_aware_Wire_Tracker/raw/main/logo/skku.svg" height="20" alt="SKKU Logo"/>
  </a>
</p>
    
<p>
  <strong>Minsoo Park∗</strong>
  <a href="https://sites.google.com/view/iisc-lab" target="_blank">
    <img src="https://github.com/pms5343/Tension_aware_Wire_Tracker/raw/main/logo/GWNU.svg" height="20" alt="GWNU Logo"/>
  </a>
  <a href="https://scholar.google.com/citations?user=6dCUM5oAAAAJ&hl=En">
    <img src="https://img.shields.io/badge/-4285F4?style=flat&logo=googlescholar&logoColor=white" alt="Google Scholar"/>
  </a>
  <a href="https://github.com/pms5343">
    <img src="https://img.shields.io/badge/-000000?style=flat&logo=github&logoColor=white" alt="GitHub"/>
  </a>
</p>   
<p>
  <strong>Seunghee Park∗</strong>
  <a href="https://sites.google.com/view/skkuscit" target="_blank">
    <img src="https://github.com/pms5343/Tension_aware_Wire_Tracker/raw/main/logo/skku.svg" height="20" alt="SKKU Logo"/>
  </a>
  <a href="https://scholar.google.com/citations?user=_CUQYq8AAAAJ&hl=en" target="_blank">
    <img src="https://img.shields.io/badge/-4285F4?style=flat&logo=googlescholar&logoColor=white" alt="Google Scholar"/>
  </a>
</p>
