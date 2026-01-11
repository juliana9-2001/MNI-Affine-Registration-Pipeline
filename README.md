# 🧠 MNI-Affine-Registration-Pipeline

**Automated spatial normalization of multi-modal brain MRI (DWI / FLAIR / SWI) to the MNI152 template using linear affine registration (SimpleITK).**

This pipeline standardizes head position, orientation, and scale across subjects, providing a robust initialization step for downstream tasks such as lesion segmentation, atlas mapping, and deep learning feature extraction.

# 📌 Overview

This repository provides a reproducible Python pipeline for registering clinical neuroimaging data into the standard MNI152 coordinate space using 12-DOF affine registration with Mattes Mutual Information.
The pipeline is designed for multi-modal alignment, making it suitable for registering contrast-diverse scans (e.g., DWI, ADC, FLAIR, SWI) to a common anatomical template.
This preprocessing step is critical for:
- Standardizing spatial geometry across cohorts
- Enabling voxel-wise analysis and atlas-based feature extraction
- Improving consistency for downstream machine learning pipelines

# ⚠️ Prerequisite — Skull Stripping (Critical)

Input images must be skull-stripped (brain-extracted).
Registering whole-head images (with skull, eyes, and neck) to a brain-only template will lead to severe misalignment and unstable optimization.

# ✅ Recommended Tool: SynthStrip

For robust skull stripping across heterogeneous MRI contrasts and pathological brains, SynthStrip (from FreeSurfer / SynthSeg) is strongly recommended.

Why SynthStrip?

- Deep learning–based and contrast-agnostic
- Works reliably across DWI, FLAIR, T1, pathological brains (stroke, tumor)
- Outperforms traditional heuristic tools (e.g., FSL BET) in clinical data

# Example (Docker):
```bash
# Example command to skull-strip using SynthStrip container
docker run --rm -v $(pwd):/data freesurfer/synthstrip:latest \
    -i /data/input_scan.nii.gz \
    -o /data/input_scan_brain.nii.gz

# 🛠️ Installation
Clone this repository:
```bash
git clone https://github.com/IqraWaheed/MNI-Affine-Registration-Pipeline.git
cd MNI-Affine-Registration-Pipeline

# Install dependencies:
```bash
pip install -r requirements.txt

Required Libraries: SimpleITK, numpy, nilearn

# 🚀 Usage
To run the registration pipeline on a batch of images:
```bash
python src/register_to_mni.py --input_dir ./data/skullstripped --output_dir ./data/registered

# Limitations
This pipeline performs affine normalization only. It does not account for nonlinear anatomical deformation and is intended as a fast, robust initialization step rather than final morphometric alignment.
