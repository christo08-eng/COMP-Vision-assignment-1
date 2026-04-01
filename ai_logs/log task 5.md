# AI Interaction Log — Task 5

## Entry 001 — 2026-03-29

**Tool:** Claude (claude.ai)
**Task:** Task 5 — Performance Comparison and Critical Analysis

---

**Prompt:**
My finetuned model has higher confidence scores but fewer detections 
than the baseline across all conditions. How should I interpret 
and present this trade-off?

**Output:**
This is a precision-recall trade-off. The finetuned model has 
learned to be more selective — it fires with higher confidence 
on objects it is certain about but suppresses uncertain detections 
that the baseline would include. This is not necessarily worse 
performance — higher confidence detections are more reliable in 
practice. Present both metrics separately and discuss the trade-off 
explicitly rather than declaring one model better overall.

**My modifications and decisions:**
- Presented detection count and confidence as separate metrics
- Discussed precision-recall trade-off in critical analysis section
- Noted finetuned model is more suitable for high-precision applications
- Used statistical test to confirm the difference is significant

---

**Prompt:**
How do I perform a paired t-test to confirm that the difference 
between blurred and deblurred detection counts is statistically 
significant?

**Output:**
Use scipy.stats.ttest_rel which performs a paired t-test on two 
related samples. Pass the detection count lists for blurred and 
deblurred images — paired because each index corresponds to the 
same scene. A p-value below 0.05 confirms statistical significance. 
Also run the test for baseline versus finetuned to confirm that 
fine-tuning made a genuine difference beyond random variation.

**My modifications and decisions:**
- Applied paired t-test for both comparisons
- Blurred vs Deblurred: p=0.000 — highly significant
- Baseline vs Finetuned: p=0.039 — statistically significant
- Reported both p-values in results with interpretation

---

**Prompt:**
The finetuned model gets zero detections for bus and umbrella 
classes on deblurred images while the baseline detects them. 
How do I discuss this honestly without undermining my results?

**Output:**
Acknowledge this directly as a class imbalance effect from 
training data. The finetuned model optimised for the dominant 
classes in your dataset — person and car — at the expense of 
rare classes. This is a well-known limitation of fine-tuning 
on small imbalanced datasets. Frame it as a specific finding 
that points to a concrete future improvement — using class-balanced 
sampling during dataset construction.

**My modifications and decisions:**
- Documented bus and umbrella as zero-detection classes
- Traced root cause to class imbalance in training annotations
- Added class-balanced sampling as a specific future improvement
- Reported finding transparently rather than omitting it

---

**Prompt:**
How should I structure the critical analysis section to read 
like a concise research report as required by the assignment?

**Output:**
Structure it with clear subsections: key findings, trade-offs, 
limitations and future work. Each finding should be grounded in 
specific numbers from your experiments rather than general claims. 
Trade-offs should discuss both sides — for example fine-tuning 
improves confidence but reduces detection count. Limitations 
should be specific and honest — dataset scope, label noise, 
class imbalance. Future work should be concrete and directly 
motivated by observed limitations.

**My modifications and decisions:**
- Structured analysis around experimental findings not theory
- Grounded every claim in specific numbers from results
- Connected each limitation to a concrete future improvement
- Used statistical evidence to support all major claims

---

**Review notes:**
- Statistical tests performed using scipy paired t-test
- All 6 model-condition combinations evaluated on same image set
- Class imbalance finding cross-checked against training labels
- Critical analysis written independently based on experimental results

**Ethical notes:**
- All results reported including where finetuned underperformed
- Class imbalance and label noise acknowledged as bias sources
- GoPro-specific findings noted as limitation for generalisation
- Statistical significance used to avoid overclaiming results

---

**Prompt:**
Looking at my full results, what is the single most important 
finding from this assignment and how should I frame it in 
the conclusion?

**Output:**
The most important finding is that deblurring as a preprocessing 
step produces statistically significant improvements in object 
detection — confirmed by p=0.000 for blurred versus deblurred. 
This validates the core hypothesis of the assignment. Frame it 
by stating the finding, the evidence, and the practical 
implication — that image restoration should be considered as 
a standard preprocessing step in detection pipelines operating 
in motion-blur prone environments such as action cameras, 
dashcams and sports broadcasting.

**My modifications and decisions:**
- Identified deblurring improvement as primary finding
- Supported claim with statistical significance p=0.000
- Added practical deployment context to conclusion
- Connected finding back to real-world applications in report