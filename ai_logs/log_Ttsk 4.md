# AI Interaction Log — Task 4

## Entry 001 — 2026-03-28

**Tool:** Claude (claude.ai)
**Task:** Task 4 — Dataset Preparation and Training

---

**Prompt:**
I want to make sure my training dataset covers different blur 
severities rather than being dominated by one type. What is the 
best way to measure and stratify blur level across 1029 images?

**Output:**
Laplacian variance is the standard approach for measuring image 
sharpness. It computes the second spatial derivative of pixel 
intensities — sharp edges produce high variance, blurry regions 
produce low variance. Sort all images by this score and divide 
into equal thirds to create heavy, medium and mild blur tiers. 
Randomly sample from each tier equally to guarantee representation 
across the full blur spectrum in your training set.

**My modifications and decisions:**
- Applied to all 1029 images before any selection
- Visualised one representative sample from each tier to verify
- Selected 500 images with equal representation per tier
- Used random seed 42 for full reproducibility

---

**Prompt:**
I am using YOLOv8 to auto-annotate my deblurred images. Some images 
produce zero detections and I am unsure whether to include them 
as negative samples or skip them entirely.

**Output:**
For object detection training, images with zero detections are 
ambiguous — they could be genuinely empty scenes or missed detections 
due to model limitations. Including them as negatives can help reduce 
false positives but risks teaching the model to suppress real objects 
if the annotations are wrong. For a dataset of this size and with 
auto-annotation, skipping zero-detection images is the safer choice 
as it avoids introducing incorrect negative labels into training.

**My modifications and decisions:**
- Chose to skip zero-detection images as recommended
- Tracked how many were skipped per split for transparency
- Noted this decision and its rationale in dataset documentation
- Monitored for class imbalance after skipping

---

**Prompt:**
My training mAP50 peaks at epoch 2 at 0.783 then fluctuates between 
0.67 and 0.74 for the remaining epochs. Should I be concerned and 
does this mean my model is not learning?

**Output:**
This is not unusual for fine-tuning on a small dataset with auto-generated 
labels. The early peak often occurs because the pretrained weights 
already generalise well and the model only needs minor adjustment. 
The subsequent fluctuation is likely due to the label noise from 
auto-annotation making the loss surface noisy. The best checkpoint 
is automatically saved so the epoch 2 result is preserved regardless. 
You could interpret the plateau as the model reaching the performance 
ceiling given the label quality available.

**My modifications and decisions:**
- Confirmed best.pt checkpoint was saved correctly at epoch 2
- Reviewed per-epoch results to confirm no catastrophic forgetting
- Interpreted plateau as evidence of label noise ceiling in report
- Evaluated final model on held-out test set to confirm generalisation

---

**Prompt:**
After augmentation my training set tripled from 331 to 993 images. 
How do I verify the augmented labels are correct, especially for 
the horizontally flipped images?

**Output:**
For horizontal flip the only coordinate that changes is the centre x 
value which becomes 1 minus the original cx. The cy, width and height 
stay identical. To verify, load a few flipped images alongside their 
label files and draw the bounding boxes manually to check alignment. 
Pay particular attention to asymmetric objects like people facing 
left or right where an incorrect flip would be obvious.

**My modifications and decisions:**
- Spot checked flipped coordinates on 5 sample images visually
- Confirmed bounding boxes aligned correctly after flipping
- Verified brightness augmented labels were identical to originals
- Documented augmentation pipeline for reproducibility

---

**Review notes:**
- Stratification confirmed by visual inspection of tier representatives
- Auto-annotation quality spot-checked on random sample images
- Augmented label correctness verified visually before training
- Test set kept completely separate from all training decisions

**Ethical notes:**
- Auto-annotation label noise acknowledged as key limitation
- Class imbalance documented — person and potted plant dominate
- Dataset construction pipeline fully documented for reproducibility
- AI consulted for methodological guidance, decisions made independently