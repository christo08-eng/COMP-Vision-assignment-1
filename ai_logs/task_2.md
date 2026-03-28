# AI Interaction Log — Task 2

## Entry 001 — 2026-03-28

**Tool:** Claude (claude.ai)
**Task:** Task 2 — Image Deblurring Implementation

**Prompt:**
Help me implement image deblurring methods for the GoPro dataset including
classical filters and deep learning approaches. Evaluate using PSNR and SSIM.

**Output:**

### Cell 1 — Imports and setup
pip install scikit-image opencv-python-headless matplotlib numpy
Imported cv2, numpy, matplotlib, skimage restoration and metrics modules
Set up paths to blur and sharp image directories

### Cell 2 — Load and visualise images
Loaded 1029 blur/sharp image pairs from GoPro dataset
Displayed side by side comparison of blurred vs sharp ground truth

### Cell 3 — Wiener Filter
Implemented Wiener filter using skimage.restoration.wiener
Required PSF (Point Spread Function) argument — used uniform box PSF
Tested PSF sizes 5, 10, 15, 20, 25
Best result at PSF=5:
- PSNR: 23.98 dB (blurred input was 24.44 dB)
- SSIM: 0.7736 (blurred input was 0.8031)

### Cell 4 — Richardson-Lucy Deconvolution
Implemented Richardson-Lucy using skimage.restoration.richardson_lucy
Tested PSF sizes 3, 5, 7 with iterations 5, 10, 20, 30
Best result at PSF=3, Iterations=5:
- PSNR: 23.67 dB
- SSIM: 0.7906

### Cell 5-6 — MPRNet Deep Learning Setup
Cloned MPRNet repository from GitHub
Downloaded pretrained GoPro weights (80.5MB)
Fixed import path issue — correct path was /kaggle/working/MPRNet/Deblurring/
Fixed module caching issue using sys.modules cleanup

### Cell 7 — MPRNet Inference
Loaded pretrained MPRNet model on CUDA GPU
Implemented deblur_image() function with:
- Tensor conversion and padding to multiple of 8
- GPU inference with torch.no_grad()
- Output clipping and format conversion
Single image result:
- PSNR: 36.66 dB (vs blurred 24.44 dB)
- SSIM: 0.9722

### Cell 8 — Full Dataset Evaluation
Ran MPRNet on all 1029 images
Saved all deblurred outputs to /kaggle/working/deblurred_images/
Final averaged results across full dataset:
- Blurred input:     PSNR 26.85 dB, SSIM 0.8653
- Wiener filter:     PSNR 23.98 dB, SSIM 0.7736
- Richardson-Lucy:   PSNR 23.67 dB, SSIM 0.7906
- MPRNet (DL):       PSNR 34.04 dB, SSIM 0.9412

**Issues encountered and fixes:**
1. wiener() missing PSF argument — fixed by adding uniform box PSF
2. ImportError for MPRNet — fixed by using correct path
   /kaggle/working/MPRNet/Deblurring/ instead of root
3. Module caching conflict — fixed by clearing sys.modules before import
4. Dataset path wrong — found correct path by walking /kaggle/input directory
5. Axes attribute error in matplotlib — fixed typo in imshow call

**Review notes:**
- All code reviewed line by line before running
- PSF size tuning done systematically across multiple values
- MPRNet pretrained on GoPro dataset specifically — appropriate choice
- Results validated against 20 image sample before full dataset run
- Classical methods underperformed due to spatially varying motion blur

**Ethical notes:**
- MPRNet is open source under MIT license — free to use academically
- Pretrained weights are publicly available from original authors
- No privacy concerns — GoPro dataset contains no personal data
- AI generated code was reviewed and modified before use
- Bias consideration: model trained on GoPro data, may not generalise
  to other camera types or blur patterns