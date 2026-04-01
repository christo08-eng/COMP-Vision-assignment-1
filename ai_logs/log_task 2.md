# AI Interaction Log — Task 2

## Entry 001 — 2026-03-29

**Tool:** Claude (claude.ai)
**Task:** Task 2 — Image Deblurring

---

**Prompt:**
I am using the official GoPro dataset which has a proper train/test 
split. How should I structure my evaluation to avoid data leakage 
with MPRNet which was pretrained on GoPro data?

**Output:**
Use only the official test split for evaluation and the train split 
for dataset preparation. The GoPro canonical split has 1111 test 
pairs and 2103 train pairs. MPRNet was trained on the train split 
so evaluating on the test split gives an unbiased estimate. 
Acknowledge in your report that some overlap may still exist 
since the Kaggle version does not guarantee exact correspondence 
with the original paper split.

**My modifications and decisions:**
- Used official test split (1111 pairs) for all Task 2 evaluation
- Used official train split (2103 pairs) only for Task 4 training
- Added limitation note in report about potential partial overlap
- Verified no file name overlap between train and test splits

---

**Prompt:**
I want to classify my test images by blur intensity and motion type 
to show robustness across different conditions. What is the best 
approach for a GoPro dataset?

**Output:**
Use Laplacian variance for blur intensity stratification — low 
variance means heavy blur, high variance means mild blur. For motion 
type, frequency domain analysis using FFT can reveal dominant blur 
directions. However GoPro footage tends to have predominantly 
horizontal motion due to the nature of action camera use, so 
motion type classification may not yield diverse results on this 
specific dataset.

**My modifications and decisions:**
- Computed Laplacian variance for all 300 test images
- Confirmed FFT analysis showed predominantly horizontal motion
- Manually selected representative images for each motion category
- Used visual inspection to assign horizontal, complex and diagonal labels
- Documented GoPro motion bias as a dataset characteristic in report

---

**Prompt:**
My classical filter results are worse than the blurred input across 
all motion types and blur intensity tiers. How should I present 
this negative result?

**Output:**
Negative results are valid and important findings. Present them 
honestly with a clear explanation — classical methods assume 
spatially uniform blur which does not hold for real camera motion. 
This finding motivates and justifies the use of deep learning. 
Show the results in a table and bar chart, then use them as 
evidence that deep learning is necessary rather than optional.

**My modifications and decision