# AI Interaction Log — Task 3

## Entry 001 — 2026-03-28

**Tool:** Claude (claude.ai)
**Task:** Task 3 — Object Detection and Analysis

---

**Prompt:**
When I run detection on blurred images the confidence scores are 
much lower than on deblurred images. Does lower confidence always 
mean worse detection or can high confidence detections still be wrong?

**Output:**
Confidence score and detection accuracy are related but not identical. 
A high confidence detection on a blurred image can still be a false 
positive — the model is certain about something that is not actually 
there. Blur can cause the model to confidently misidentify blurred 
textures as familiar object patterns. This is why you should look 
at both confidence scores and detection counts together, and ideally 
compare against ground truth annotations. In your case the train 
class disappearing after deblurring is a good example of a high 
confidence false positive caused by blur.

**My modifications and decisions:**
- Tracked both detection counts and confidence scores separately
- Identified train class as a false positive specific to blur condition
- Documented this as evidence that blur causes incorrect detections
- Used this to argue deblurring improves both recall and precision

---

**Prompt:**
I found that 14% of blurred images had complete detection failure 
but deblurring recovered all objects. What are the likely root 
causes for complete failure specifically?

**Output:**
Complete detection failure on blurred images typically happens when 
blur severity is so extreme that no recognisable features remain 
above the detection threshold. The model's feature extractors rely 
on edges and textures that are completely destroyed by heavy motion 
blur. Images with Laplacian variance below around 10 often fall 
into this category. Another cause is small objects whose features 
become completely indistinguishable from background noise when blurred.

**My modifications and decisions:**
- Checked Laplacian scores of failure cases — confirmed heavy blur
- Noted small object classes like backpack contributed to failures
- Documented 14% figure as a key quantitative finding
- Connected failure cases back to blur stratification from Task 4

---

**Prompt:**
Potted plant detections increased from 44 to 197 after deblurring — 
a 4.5x increase. Is this realistic or could something be wrong 
with the detection?

**Output:**
A 4.5x increase is large but plausible for a class like potted plant 
which has subtle visual features easily destroyed by motion blur. 
Small decorative plants in street scenes are often partially occluded 
and rely on fine texture details for detection. It is worth visually 
verifying a sample of these detections to confirm they are genuine 
rather than the deblurring process introducing false positives 
through over-sharpening artefacts.

**My modifications and decisions:**
- Visually inspected sample images with high potted plant counts
- Confirmed detections corresponded to real plants in scene
- Noted potted plant as a class highly sensitive to image quality
- Included this as a specific example in the per-class analysis

---

**Prompt:**
The inference latency is almost identical across blurred, deblurred 
and sharp images at around 27ms each. What does this tell us about 
the pipeline efficiency?

**Output:**
Identical inference latency confirms that image content quality 
does not affect detection speed — YOLO processes all images in the 
same time regardless of sharpness. This means the computational 
cost of the full pipeline is dominated by the deblurring step 
(MPRNet) not the detection step. For real-time applications the 
bottleneck is therefore the restoration model, and any efficiency 
improvements should focus there rather than on the detector.

**My modifications and decisions:**
- Measured over 20 trials per condition for stable timing estimates
- Confirmed MPRNet is the pipeline bottleneck through separate timing
- Noted implications for real-time deployment in analysis
- Recommended lightweight deblurring alternatives as future work

---

**Review notes:**
- All detection counts verified visually against displayed images
- False positive finding verified across multiple images
- Latency measurements taken in same GPU session for fair comparison
- Potted plant detections spot-checked visually before reporting

**Ethical notes:**
- COCO pretrained model may have demographic bias in person detection
- False positive finding reported transparently as a limitation
- No selective reporting — all 50 images included in averages
- AI consulted for interpretation guidance only