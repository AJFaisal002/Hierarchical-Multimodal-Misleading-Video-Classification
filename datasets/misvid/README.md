# MisVid: Cross-Modal Misleading Video Dataset

**A multimodal benchmark for hierarchical misleading-video classification**

[![Dataset](https://img.shields.io/badge/Dataset-MisVid-blue)](https://drive.google.com/drive/folders/12mL6AxYjsrdg9nKE36M9eLnTlKg48qyt?usp=drive_link)
![Videos](https://img.shields.io/badge/Videos-2%2C400-green)
![Modalities](https://img.shields.io/badge/Modalities-Vision%20%7C%20Audio%20%7C%20Text-purple)
![Task](https://img.shields.io/badge/Task-Hierarchical%20Classification-orange)

---

## Overview

**MisVid (Cross-Modal Misleading Video Dataset)** is a collection of 2,400 short-form videos developed to support research on multimodal misleading-video classification.

The dataset provides balanced binary labels and four fine-grained misleading-content categories, enabling evaluation of both binary and hierarchical classification frameworks.

The accompanying research investigates visual, audio, and textual evidence for distinguishing safe videos from misleading content and identifying the corresponding misleading category.

## Dataset Statistics

| Property | Value |
|---|---|
| Dataset name | MisVid |
| Total videos | 2,400 |
| Safe videos | 1,200 |
| Misleading videos | 1,200 |
| Fine-grained misleading classes | 4 |
| Modalities | Vision, Audio, Text |
| Primary task | Hierarchical misleading-video classification |

## Hierarchical Classification

**Stage 1 — Binary Classification**

Each video is classified as either **Safe** or **Misleading**.

**Stage 2 — Fine-Grained Classification**

Videos classified as misleading are assigned to one of four categories.

| Label | Category | Videos |
|---|---|---:|
| IF | Identity Fabrication | 300 |
| PM | Perception Manipulation | 300 |
| SUS | Scientifically Unrealistic Scene | 300 |
| SC | Surreal Content | 300 |
| **Total** | **Misleading videos** | **1,200** |

### Label Hierarchy

```text
Input Video
    |
    +-- Safe
    |
    +-- Misleading
           |
           +-- Identity Fabrication (IF)
           +-- Perception Manipulation (PM)
           +-- Scientifically Unrealistic Scene (SUS)
           +-- Surreal Content (SC)
```

## Multimodal Information

The research framework processes three complementary sources of information:

- **Vision:** Video frames representing visual content and temporal information.
- **Audio:** Speech and acoustic signals extracted from video.
- **Text:** Textual evidence obtained through automatic speech recognition (ASR) and optical character recognition (OCR).

These modalities support the analysis of complementary and potentially conflicting evidence.

## Dataset Access

The MisVid dataset is hosted externally to avoid storing large video files directly in the GitHub repository.

**[Access the MisVid Dataset on Google Drive](https://drive.google.com/drive/folders/12mL6AxYjsrdg9nKE36M9eLnTlKg48qyt?usp=drive_link)**

Please consult the dataset's access conditions and applicable third-party content rights before redistribution or reuse.

## Research Applications

MisVid supports research on:

- Hierarchical misleading-video classification
- Multimodal video understanding
- Cross-modal fusion
- Fine-grained content categorization
- Explainable artificial intelligence
- Multimedia misinformation analysis

## Associated Publication

**Safe or Misleading, and Why? Coarse-to-Fine Multimodal Classification of Short-Form Videos with Evidence-Based Explanations**

Citation details will be updated when the associated manuscript is published.

## Responsible Use

MisVid is provided for research and academic investigation. Users are responsible for respecting applicable copyright, privacy, platform, and ethical requirements.

## Contact

For dataset-related questions, please open an issue in the main GitHub repository.
