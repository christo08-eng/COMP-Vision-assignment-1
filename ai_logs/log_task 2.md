# AI Interaction Log — Task 2

## Entry 001 — 2026-03-28

**Tool:** Claude (claude.ai)
**Task:** Task 2 — Image Deblurring

---

**Prompt:**
I am implementing Wiener filtering for motion deblurring but my 
results are worse than the blurred input. The PSNR dropped from 
24.44 to 23.98. What could be causing this and how do I fix it?

**Output:**
The most likely cause is a mismatch between your assumed PSF and 
the actual blur kernel. Wiener filtering is very sensitive to PSF 
accuracy — if your assumed blur shape does not match the real blur 
the filter amplifies noise instead of removing it. GoPro motion blur 
is spatially varying, meaning the blur direction and intensity changes 
across the image, which a single uniform PSF cannot capture. Try 
testing multiple PSF sizes systematically to find the least bad 
result, and document this sensitivity as a finding in your report.

**My modifications and decisions:**
- Tested PSF sizes 5 to 25 systematically and recorded all results
- Confirmed sensitivity — larger PSF made results significantly worse
- Documented PSF mismatch as the root cause in analysis
- Used this finding to justify moving to Richardson-Lucy next

---

**Prompt:**
Richardson-Lucy also failed to improve over the blurred input. 
More iterations actually made results worse. Why does this happen?

**Output:**
This is a known phenomenon called noise amplification in 
Richardson-Lucy deconvolution. Each iteration attempts to sharpen 
the image further but also amplifies any noise present. Without 
an accurate blur kernel the algorithm diverges rather than converging 
to the correct solution. For real-world motion blur where the kernel 
is unknown and spatially varying, classical iterative methods 
consistently struggle. This is well documented in the deblurring 
literature and is a key motivation for deep learning approaches.

**My modifications and decisions:**
- Tested iterations 5, 10, 20 and 30 across multiple PSF sizes
- Confirmed noise amplification — PSNR decreased with more iterations
- Concluded classical methods are not suitable for this dataset type
- Used these negative results as evidence in report discussion

---

**Prompt:**
I am getting an ImportError when trying to load MPRNet. The class 
exists in the file but Python cannot find it. How do I debug this?

**Output:**
This is likely a module caching or path conflict issue. Python caches 
imported modules in sys.modules — if you imported an earlier version 
the cached version takes priority even after fixing the path. Clear 
the cache by removing MPRNet entries from sys.modules before 
re-importing. Also make sure you are pointing to the correct 
subdirectory since MPRNet has separate folders for deblurring, 
deraining and denoising — you need the Deblurring subfolder 
specifically.

**My modifications and decisions:**
- Walked the file tree to find the correct subfolder path
- Cleared sys.modules cache before reimporting
- Verified the correct class name using grep on the source file
- Added path insertion at index 0 to ensure priority over cached paths

---

**Prompt:**
MPRNet gives PSNR of 36.66 dB on one image but the average across 
all 1029 images is only 34.04 dB. Why is there such a difference 
and which number should I report?

**Output:**
The single image result is not representative — it happened to be 
a relatively easy case. The average across all images is the correct 
number to report as it accounts for variation in blur severity, 
scene complexity and motion direction. The gap between single image 
and average performance is actually a useful finding — it shows the 
model performs inconsistently across different blur conditions, which 
is worth discussing in your analysis alongside per-image variance.

**My modifications and decisions:**
- Reported full dataset average as primary result
- Noted per-image variation as a finding in the analysis
- Included both 20-image sample results and full dataset results
- Discussed implications of variance for real-world deployment

---

**Review notes:**
- All metric calculations verified against skimage documentation
- PSF sensitivity confirmed across multiple test images
- MPRNet results validated on sample before full dataset processing
- Negative classical results reported honestly without omission

**Ethical notes:**
- MPRNet is open source and publicly available for academic research
- GoPro dataset contains no personally identifiable information
- AI consulted for debugging and conceptual understanding only
- All experimental findings reported including negative results