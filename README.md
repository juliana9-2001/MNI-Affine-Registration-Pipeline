# MNI-Affine-Registration-Pipeline

Performs linear spatial normalization (Affine, 12-DOF) to the MNI152 template. This pipeline standardizes head position and size, serving as a robust initialization step before creating separate pipelines for lesion segmentation.

# MNI-Affine-Registration-Pipeline
Automated spatial normalization of multi-modal brain MRI (DWI/FLAIR/SWI) to the MNI152 template using Linear Affine Registration (SimpleITK).

## 📌 Overview
This repository contains a robust Python pipeline for registering clinical neuroimaging data to the standard **MNI152 coordinate space**. It uses **Linear Affine Registration (12 Degrees of Freedom)** with **Mattes Mutual Information**, making it suitable for multi-modal registration (e.g., aligning DWI/ADC images to a T1-weighted template).

This preprocessing step is critical for standardizing head position and size before lesion segmentation or deep learning feature extraction.

## ⚠️ Prerequisites: Skull Stripping
**CRITICAL:** This pipeline requires **skull-stripped (brain extracted)** images as input. 
Attempting to register whole-head images (with skull/eyes) to a brain-only template will result in severe misalignment.

### Recommended Tool: SynthStrip
For robust skull stripping across different modalities (DWI, FLAIR, T1), I recommend using **SynthStrip** (part of FreeSurfer/SynthSeg).

**Why SynthStrip?** Unlike traditional tools (e.g., FSL BET), SynthStrip uses a deep learning model that is agnostic to image contrast, making it highly effective for pathological brains (stroke/tumor).

**How to run via Docker:**
```bash
# Example command to skull-strip using SynthStrip container
docker run --rm -v $(pwd):/data freesurfer/synthstrip:latest \
    -i /data/input_scan.nii.gz \
    -o /data/input_scan_brain.nii.gz

### 🛠️ Installation
Clone this repository:
Bash
git clone [https://github.com/IqraWaheed/MNI-Affine-Registration-Pipeline.git](https://github.com/IqraWaheed/MNI-Affine-Registration-Pipeline.git)
cd MNI-Affine-Registration-Pipeline

# Install dependencies:
Bash
pip install -r requirements.txt
Required Libraries: SimpleITK, numpy, nilearn

### 🚀 Usage
To run the registration pipeline on a batch of images:
Bash
python src/register_to_mni.py --input_dir ./data/skullstripped --output_dir ./data/registered
