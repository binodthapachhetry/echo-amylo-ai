# EchoNet-Dynamic Toolkit 🏥🫀🎥  
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.8%20|%203.9%20|%203.10-blue.svg)](#)
[![PyPI - Status](https://img.shields.io/badge/status-research--grade-yellow)](#)

> End-to-end deep-learning workflows for echocardiography video analysis –  
> ejection-fraction regression & left-ventricular segmentation – built on PyTorch/TorchVision.

## Why this repo?
Cardiac ultrasound (A4C view) remains the clinician’s work-horse for assessing heart function,
yet expert frame selection and manual tracing limit scalability.  
This toolkit reproduces and extends the **NeurIPS 2019 ML4H EchoNet-Dynamic** benchmarks:

* **EF Prediction** – 3-D CNNs (e.g. R(2+1)D-18) trained on 32-frame clips  
* **LV Segmentation** – 2-D FCNs (e.g. DeepLabV3-ResNet50) for systole / diastole frames  
* Publication-quality plots, boot-strapped CIs, and rendered MP4 overlays

## Table of Contents
1. [Quick Start](#quick-start)  
2. [Dataset Setup](#dataset-setup)  
3. [Training Recipes](#training-recipes)  
4. [Results & Artifacts](#results--artifacts)  
5. [Citation](#citation)  
6. [License](#license)

## Quick Start
```bash
# 1️⃣ Create & activate environment
python -m venv venv_echoai && source venv_echoai/bin/activate
pip install -r requirements.txt   # ▸ PyTorch ≥1.12, TorchVision ≥0.13

# 2️⃣ Point to dataset root (must contain FileList.csv and Videos/)
export ECHONET_DATA_DIR=/path/to/a4c-video-dir

# 3️⃣ Train EF predictor
echonet video --frames 32 --period 2 --model_name r2plus1d_18 --pretrained

# 4️⃣ Train LV segmentation (+ optional rendered videos)
echonet segmentation --model_name deeplabv3_resnet50 --save_video
```

## Dataset Setup
EchoNet-Dynamic public release – https://echonet.github.io/dynamic/  
Directory structure expected by this repo:
```
<DATA_DIR>/
├── FileList.csv
├── VolumeTracings.csv
└── Videos/
    ├── 0X1234ABCD.mp4
    └── ...
```
Define the root via `--data_dir` CLI flag **or** the `$ECHONET_DATA_DIR` environment variable.

## Training Recipes
* EF regression (baseline):  
  `echonet video --model_name r2plus1d_18 --frames 32 --period 2 --pretrained`
* LV segmentation:  
  `echonet segmentation --model_name deeplabv3_resnet50 --pretrained`
* Inference only with saved weights:  
  `echonet video --weights /path/to/best.pt --run_test`
* Ablation (100 patients):  
  `echonet video --num_train_patients 100`
* Resume training automatically (checkpoint detection built-in).

## Results & Artifacts
All runs write to `output/{video|segmentation}/<spec>/`

| Task | Metric | Val | Test | Paper |
|------|--------|-----|------|-------|
| EF regression (R2+1D-18) | MAE (%) | 4.21 | 4.35 | 4.06 |
| LV segmentation (DLV3-R50) | Dice | 0.89 | 0.88 | 0.89 |

> Pre-trained weights and demo MP4s are available in the repository “Releases” tab.

## Citation
If you use this code, please cite:
```bibtex
@inproceedings{ouyang2020echonet,
  title={Video-based AI for beat-to-beat assessment of cardiac function},
  author={Ouyang, Diane and He, Benjamin and et al.},
  booktitle={Nature},
  year={2020}
}
```

## License
MIT – see [LICENSE](LICENSE) for details.
