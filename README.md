# COMP6001 Assignment 1 — Image Restoration and Object Detection

## Project Overview
A multimodal pipeline integrating motion deblurring and object detection
using the official GoPro dataset with proper train/test split to avoid
data leakage. The pipeline combines classical filtering, deep learning
restoration (MPRNet) and YOLOv8 object detection across blurred,
deblurred and sharp image conditions.

## Setup
```bash
pip install -r requirements.txt
```

## Dataset
Official GoPro dataset with canonical train/test split:
- Test split: 1111 image pairs (used for Tasks 2, 3, 5)
- Train split: 2103 image pairs (used for Task 4)

Download from:
https://www.kaggle.com/datasets/jishnuparayilshibu/a-curated-list-of-image-deblurring-datasets

## Repository Structure
```
comp6001-assignment1/
├── ai_logs/          # AI interaction logs for all tasks
├── data/             # Dataset splits
├── models/           # Model checkpoints
├── notebooks/        # project.ipynb
├── report/           # report.pdf
└── README.md
```

## Key Results
| Method              | PSNR (dB) | SSIM   |
|---------------------|-----------|--------|
| Blurred (input)     | 25.11     | 0.7717 |
| Wiener Filter       | 20.31     | 0.7149 |
| Richardson-Lucy     | 19.96     | 0.7071 |
| MPRNet (DL)         | 29.38     | 0.9173 |

Detection improvement after deblurring: +26.7%
Fine-tuned model mAP50: 0.590 (val) / 0.581 (test)
Statistical significance: p=0.039

## Tasks
- Task 1: Version control and AI-assisted coding
- Task 2: Image deblurring (Wiener, Richardson-Lucy, MPRNet)
- Task 3: Object detection and analysis (YOLOv8)
- Task 4: Dataset preparation and YOLOv8 fine-tuning
- Task 5: Performance comparison and critical analysis

## Running the Notebook
1. Open project.ipynb on Kaggle
2. Add dataset: jishnuparayilshibu/a-curated-list-of-image-deblurring-datasets
3. Enable GPU accelerator
4. Run all cells in order

## AI Tools Used
All AI interactions are logged in the ai_logs/ folder with prompts,
outputs, review notes and ethical considerations for Tasks 1-5.

## Notes
- Random seed 42 used throughout for reproducibility
- No data leakage — train and test splits are completely separate
- Auto-annotation using YOLOv8m at confidence threshold 0.25