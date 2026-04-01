# AI Interaction Log — Task 4

## Entry 001 — 2026-03-29

**Tool:** Claude (claude.ai)
**Task:** Task 4 — Dataset Preparation and Training

---

**Prompt:**
I am using the official GoPro train split with 2103 images for 
dataset preparation. How do I ensure there is no overlap with the 
test split I used in Tasks 2 and 3?

**Output:**
The official GoPro split guarantees no overlap between train and 
test by design — they are separate sequences captured at different 
times. Simply using the train folder for annotation and the test 
folder for evaluation is sufficient. Document this in your report 
as it directly addresses the data leakage concern that exists 
when using flat datasets without preserved splits.

**My modifications and decisions:**
- Confirmed train and test folders are completely separate
- Used train split exclusively for all annotation and training
- Used test split exclusively for all evaluation
- Added explicit no-overlap verification print statement in code

---

**Prompt:**
I selected 600 images from 2103 training images. Is this enough 
to fine-tune YOLOv8 effectively and how should I justify this choice?

**Output:**
600 images with augmentation is sufficient for fine-tuning a 
pretrained model. The key justification is that you are not 
training from scratch — YOLOv8m already has strong feature 
extraction from COCO training. After brightness and flip 
augmentation your effective training set triples to around 1800 
images. The stratified selection across blur tiers ensures diversity 
which matters more than raw quantity for this task.

**My modifications and decisions:**
- Selected 600 images with 200 per blur tier for diversity
- Applied brightness jitter and horizontal flip augmentation
- Achieved 1191 effective training images after augmentation
- Justified selection in report based on fine-tuning literature

---

**Prompt:**
My training mAP50 improved consistently from 0.099 at epoch 1 to 
0.590 at epoch 19. All losses decreased steadily. Does this indicate 
healthy training?

**Output:**
Yes this indicates healthy training. Consistent mAP improvement 
with decreasing losses across all 20 epochs suggests the model 
is genuinely learning from the deblurred dataset without 
overfitting. The fact that improvement continued until epoch 19 
suggests you could potentially benefit from a few more epochs, 
but the gains would likely be marginal at this point.

**My modifications and decisions:**
- Confirmed all three loss curves decreased consistently
- Noted mAP improvement continued through epoch 19
- Saved best checkpoint at epoch 19 automatically
- Reported training convergence as evidence of healthy fine-tuning

---

**Prompt:**
The test set mAP50 is 0.536 while validation mAP50 was 0.590. 
Is this gap concerning or expected?

**Output:**
A gap of around 5 percentage points between validation and test 
is within normal range and not concerning. It reflects natural 
variation between the two splits and confirms the model generalises 
beyond the validation set. A larger gap would suggest overfitting. 
Report both numbers and note that the test set result is the 
unbiased estimate of real-world performance.

**My modifications and decisions:**
- Reported both validation and test mAP50 in results
- Noted the gap is within acceptable range in report discussion
- Used test set result as primary performance metric
- Did not tune any hyperparameters based on test set results

---

**Review notes:**
- Train/test split separation confirmed before any annotation
- Augmented label correctness verified on sample images
- Training convergence confirmed through loss and mAP curves
- Test set evaluation performed only once after training complete

**Ethical notes:**
- Auto-annotation label noise acknowledged as limitation
- Class imbalance documented — person and car dominate
- No hyperparameter tuning performed on test set
- Dataset construction fully reproducible with random seed 42


---

**Prompt:**
Should I include the blurred versions of my training images 
alongside the deblurred ones to make the model more robust 
to both conditions?

**Output:**
This is a valid approach called mixed training and can improve 
robustness. However it introduces a trade-off — the model must 
balance learning from both clean and degraded inputs which can 
reduce peak performance on either condition individually. For 
this assignment since the goal is specifically to train on 
deblurred images as stated in the task description, using only 
deblurred images is the correct approach. Training on blurred 
images would be a separate experiment worth mentioning as 
future work.

**My modifications and decisions:**
- Kept training exclusively on deblurred images as per task spec
- Retained blurred versions in separate folder for Task 5 comparison
- Noted mixed training as a concrete future improvement suggestion
- Referenced task specification as justification for design choice