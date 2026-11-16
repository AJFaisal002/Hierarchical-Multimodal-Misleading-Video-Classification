# 🎬 Safe or Misleading, and Why?

<div align="center">

### Coarse-to-Fine Multimodal Classification of Short-Form Videos with Evidence-Based Explanations

**An Explainable Hierarchical Multimodal Framework for Misleading Video Classification**

[![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/Framework-PyTorch-EE4C2C?logo=pytorch&logoColor=white)](https://pytorch.org/)
[![Research](https://img.shields.io/badge/Research-Multimodal%20AI-7952B3)](#overview)
[![Task](https://img.shields.io/badge/Task-Hierarchical%20Classification-00897B)](#hierarchical-classification-framework)
[![Dataset](https://img.shields.io/badge/Dataset-MisVid-1976D2)](datasets/misvid/README.md)

**[📊 Datasets](datasets/README.md) · [🧠 Model Checkpoints](checkpoints/) · [💻 Notebooks](notebooks/) · [📈 Results](#experimental-results) · [📖 Citation](#citation)**

</div>

---

## 📌 Overview

Short-form videos may contain misleading information through fabricated identities, manipulated perceptions, or unrealistic scenes. Identifying such content is challenging because important evidence can be distributed across visual scenes, speech, and textual information.

Existing multimodal approaches often focus on binary classification, providing limited information about the specific type of misleading content.

This repository accompanies our research on **hierarchical multimodal misleading-video classification**, which addresses this limitation through a two-stage framework:

1. **Stage 1:** Distinguishes Safe from Misleading videos.
2. **Stage 2:** Classifies misleading videos into four fine-grained categories.

The framework integrates **vision, audio, and text**, evaluates multiple multimodal fusion strategies, and incorporates explainability techniques to provide evidence supporting its predictions.

The research is evaluated using the **MisVid dataset** and the independent **FakeTT benchmark**.

---

## ✨ Key Contributions

| Contribution | Description |
|---|---|
| 🌳 **Hierarchical Classification** | A two-stage framework that distinguishes safe from misleading videos and subsequently identifies four misleading-content categories. |
| 🎥 **Trimodal Learning** | Integration of visual, audio, and textual representations for short-form video understanding. |
| 🔀 **Fusion Evaluation** | Systematic comparison of nine multimodal fusion strategies, including ambiguity-aware fusion. |
| 📐 **Theoretical Analysis** | Analysis of hierarchical error propagation and fusion-related computational properties. |
| 🌐 **Cross-Dataset Evaluation** | Evaluation on MisVid and FakeTT, supported by cross-validation and comparative experiments. |
| 🔍 **Explainable Predictions** | Visual, textual, and modality-level explanations using Grad-CAM, Score-CAM, LIME, and modality ablation. |
| 💬 **Evidence-Based Explanations** | LLM-based synthesis of extracted evidence into readable explanations without allowing the LLM to determine the predicted class. |

---

## 🌳 Hierarchical Classification Framework

The proposed framework follows a **coarse-to-fine classification strategy**.

Instead of directly assigning every video to one of five classes, the framework first determines whether the video is misleading. Only videos predicted as misleading proceed to the second classification stage.

### Classification Pipeline

```text
                    ┌─────────────────────┐
                    │     Input Video     │
                    └──────────┬──────────┘
                               │
                 ┌─────────────┴─────────────┐
                 │   Multimodal Processing   │
                 │                           │
                 │  Vision | Audio | Text    │
                 └─────────────┬─────────────┘
                               │
                    ┌──────────▼──────────┐
                    │  Multimodal Fusion  │
                    └──────────┬──────────┘
                               │
                    ┌──────────▼──────────┐
                    │      Stage 1        │
                    │ Binary Classification│
                    └──────────┬──────────┘
                               │
                  ┌────────────┴────────────┐
                  │                         │
               SAFE                    MISLEADING
                  │                         │
             Final Output          ┌────────▼────────┐
                                   │     Stage 2     │
                                   │  Fine-Grained   │
                                   │ Classification  │
                                   └────────┬────────┘
                                            │
                         ┌──────────┬───────┴──────┬──────────┐
                         │          │              │          │
                         IF         PM            SUS         SC
```

### Stage 1 — Binary Classification

The first stage classifies each video into one of two categories:

- **Safe:** Content not identified as misleading under the dataset taxonomy.
- **Misleading:** Content containing misleading information or representations.

### Stage 2 — Fine-Grained Classification

Videos predicted as misleading are further classified into four categories.

| Label | Category | Description |
|---|---|---|
| **IF** | Identity Fabrication | Content involving fabricated or falsely represented identities. |
| **PM** | Perception Manipulation | Content that manipulates how viewers interpret events or visual information. |
| **SUS** | Scientifically Unrealistic Scene | Content depicting scenes that conflict with established scientific or physical principles. |
| **SC** | Surreal Content | Content containing surreal or implausible visual situations. |

### Why Hierarchical Classification?

The hierarchical design separates two related but distinct tasks:

- Identifying whether a video is misleading.
- Determining the specific nature of misleading content.

This organization enables fine-grained analysis while retaining a binary decision suitable for general content-screening applications.

Because Stage 2 receives only videos predicted as misleading, errors in Stage 1 can affect the final five-class prediction. The accompanying paper analyzes this error-propagation behavior.

---

## 🧠 Multimodal Learning

The framework integrates three complementary modalities.

| Modality | Representation | Purpose |
|---|---|---|
| 👁️ Vision | Video-frame representations | Capture visual scenes and temporal information. |
| 🔊 Audio | Speech and acoustic representations | Capture spoken and nonverbal audio evidence. |
| 📝 Text | ASR, OCR, and textual representations | Capture spoken words and visible textual information. |

### Feature Extraction

The research investigates pretrained multimodal representations, including:

- **CLIP ViT-L/14** for visual feature extraction.
- **Wav2Vec2** for audio representation learning.
- **XLM-RoBERTa** for textual representation learning.
- **Qwen-based text representations** in the evaluated configurations.

These representations provide complementary evidence for multimodal classification.

### Fusion Strategies

Nine fusion mechanisms are evaluated:

| Category | Fusion Strategy |
|---|---|
| Conventional | Concatenation |
| Conventional | Gated Fusion |
| Conventional | Attention-Based Fusion |
| Conventional | Cross-Attention |
| Hybrid | Combined CA–GF–AMW |
| Comparative | QuaFusion |
| Comparative | CCIN-SA |
| Comparative | AtCAF |
| Ambiguity-Aware | Ambiguity-Aware Fusion |

The experiments show that fusion performance varies across classification tasks.

Cross-attention achieves the strongest Stage 1 result, whereas ambiguity-aware fusion performs best on Stage 2 and FakeTT.

The framework uses ambiguity-aware fusion in its final evaluated configuration.

---

## 📊 Datasets

Two datasets are used to evaluate the framework.

| Dataset | Videos | Classification Task | Purpose |
|---|---:|---|---|
| **MisVid** | 2,400 | Binary and Fine-Grained | Primary hierarchical evaluation |
| **FakeTT** | 1,992 evaluated | Binary | External generalization evaluation |

### 1. MisVid — Primary Dataset

**MisVid (Cross-Modal Misleading Video Dataset)** contains 2,400 short-form videos, balanced between Safe and Misleading content.

| Category | Videos |
|---|---:|
| Safe | 1,200 |
| Identity Fabrication | 300 |
| Perception Manipulation | 300 |
| Scientifically Unrealistic Scene | 300 |
| Surreal Content | 300 |
| **Total** | **2,400** |

**[📥 Access MisVid Dataset](https://drive.google.com/drive/folders/12mL6AxYjsrdg9nKE36M9eLnTlKg48qyt?usp=drive_link)**

**[📖 MisVid Documentation](datasets/misvid/README.md)**

### 2. FakeTT — External Benchmark

FakeTT is used to evaluate the generalization of the Stage 1 binary classifier on an independent real-world dataset.

Because FakeTT provides binary labels, it is not used to evaluate the four-category MisVid taxonomy.

**[📖 FakeTT Documentation](datasets/fakett/README.md)**

> **Data availability:** Large video files are hosted externally. The repository provides dataset documentation and access information. Users should follow the applicable dataset and third-party content usage conditions.

---

## 📈 Experimental Results

The framework is evaluated through independent stage-level experiments, complete hierarchical evaluation, cross-dataset testing, fusion comparisons, and explainability analysis.

### Main Classification Performance

| Evaluation | Accuracy | Macro F1 |
|---|---:|---:|
| **MisVid — Stage 1 (Binary)** | **85.75%** | — |
| **MisVid — Stage 2 (Fine-Grained)** | **84.00%** | — |
| **MisVid — Complete Hierarchical Cascade** | **78.75%** | **74.83%** |
| **FakeTT — Binary Classification** | **85.71%** | **85.08%** |

*Results are reported for the corresponding evaluation settings in the associated manuscript. Stage-level and complete-cascade results measure different classification tasks.*

### Five-Fold Cross-Validation

| Task | Accuracy (%) | Macro F1 (%) | MCC |
|---|---:|---:|---:|
| Stage 1 | 86.88 ± 0.44 | 86.87 ± 0.45 | 0.738 ± 0.009 |
| Stage 2 | 82.50 ± 0.71 | 82.67 ± 0.72 | 0.770 ± 0.010 |
| FakeTT | 85.62 ± 0.38 | 85.15 ± 0.36 | 0.703 ± 0.007 |

Cross-validation was conducted within the training data while maintaining the held-out test set.

### Statistical Significance

The research compares cross-attention and ambiguity-aware fusion across ten independent random seeds.

| Task | Paired t-test | Wilcoxon Test |
|---|---:|---:|
| Stage 1 | 0.067 | 0.063 |
| Stage 2 | <0.001 | 0.002 |
| FakeTT | <0.001 | 0.002 |

The differences favoring ambiguity-aware fusion are statistically significant for Stage 2 and FakeTT, while the Stage 1 difference is not statistically significant at the 0.05 level.

---

## 🔍 Explainable Artificial Intelligence

The framework incorporates multiple complementary explanation methods to investigate the evidence underlying its predictions.

### Visual Explainability

**Grad-CAM and Score-CAM** highlight image regions associated with model predictions.

These methods help examine which visual regions contribute to the classification decision.

### Textual Explainability

**LIME** identifies influential words and textual features associated with the predicted class.

### Modality Contribution Analysis

Modality-ablation experiments investigate the relative importance of vision, audio, and text.

The analysis indicates that:

- Text is particularly influential for Stage 1 binary classification.
- Vision is particularly influential for Stage 2 fine-grained classification.

### LLM-Synthesized Explanations

The framework further uses a large language model to combine extracted visual, textual, and modality-level evidence into readable explanations.

**Important:** The LLM does not independently determine the classification label. It summarizes evidence associated with the model's prediction.

---

## 📂 Repository Structure

```text
CrossModal-Misleading-Video-Detection/
│
├── README.md
│
├── checkpoints/
│   ├── README.md
│   ├── stage1_binary_model.pth
│   └── stage2_misleading_model.pth
│
├── datasets/
│   ├── README.md
│   ├── misvid/
│   │   └── README.md
│   └── fakett/
│       └── README.md
│
└── notebooks/
    ├── fusion_strategies/
    ├── hierarchical/
    ├── ablation_studies/
    ├── explainability/
    ├── cross_dataset/
    ├── baseline_comparison/
    └── exploratory/
```

### Directory Guide

| Directory | Contents |
|---|---|
| [`checkpoints/`](checkpoints/) | Saved Stage 1 and Stage 2 model checkpoints |
| [`datasets/`](datasets/) | Dataset descriptions and access information |
| [`notebooks/fusion_strategies/`](notebooks/fusion_strategies/) | Multimodal fusion experiments |
| [`notebooks/hierarchical/`](notebooks/hierarchical/) | Hierarchical classification experiments |
| [`notebooks/ablation_studies/`](notebooks/ablation_studies/) | Modality and feature ablation studies |
| [`notebooks/explainability/`](notebooks/explainability/) | Explainability and LLM-based explanation experiments |
| [`notebooks/cross_dataset/`](notebooks/cross_dataset/) | FakeTT evaluation experiments |
| [`notebooks/baseline_comparison/`](notebooks/baseline_comparison/) | Comparative experiments |
| [`notebooks/exploratory/`](notebooks/exploratory/) | Earlier experimental and development notebooks |

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/AJFaisal002/CrossModal-Misleading-Video-Detection.git

cd CrossModal-Misleading-Video-Detection
```

### 2. Prepare the Environment

The experimental implementations are provided as Jupyter notebooks.

A Python environment with PyTorch, Jupyter, and the dependencies used by the relevant notebook is required.

```bash
python -m venv .venv
```

Activate the environment:

**Windows**

```bash
.venv\Scripts\activate
```

**Linux / macOS**

```bash
source .venv/bin/activate
```

Install Jupyter:

```bash
python -m pip install jupyterlab
```

Additional dependencies, pretrained models, and configuration requirements depend on the selected experiment.

### 3. Download the Dataset

Access the primary dataset:

**[MisVid — Google Drive](https://drive.google.com/drive/folders/12mL6AxYjsrdg9nKE36M9eLnTlKg48qyt?usp=drive_link)**

For external evaluation, obtain FakeTT from its original provider.

Refer to [`datasets/`](datasets/) for additional information.

### 4. Explore the Experiments

Launch JupyterLab:

```bash
jupyter lab
```

Open the relevant notebook from the `notebooks/` directory.

The notebooks cover hierarchical classification, multimodal fusion, ablation studies, cross-dataset evaluation, and explainability.

> **Reproducibility note:** The repository currently organizes the original research notebooks and saved model checkpoints. A consolidated installation specification and fully automated end-to-end reproduction workflow are not yet provided. Some notebooks may require adjustments to local paths, dependencies, or runtime settings.

---

## 🧪 Research Reproducibility

The experimental evaluation includes:

- Independent Stage 1 and Stage 2 classification.
- End-to-end hierarchical cascade evaluation.
- Comparison of nine multimodal fusion strategies.
- Modality-ablation experiments.
- Five-fold cross-validation.
- Multi-seed statistical significance testing.
- External evaluation on FakeTT.
- Visual, textual, and modality-level explainability.

Researchers should use the corresponding dataset splits, preprocessing procedures, model configurations, and evaluation protocols described in the manuscript when reproducing the results.

---

## 📖 Associated Publication

**Safe or Misleading, and Why? Coarse-to-Fine Multimodal Classification of Short-Form Videos with Evidence-Based Explanations**

**Authors**

Adnan Faisal · Md. Mynul Hasan · Aish Albladi · Mohammad Azad

**Affiliations**

- Department of Computer Science and Engineering, Chittagong University of Engineering & Technology (CUET), Bangladesh.
- Department of Computer Science, College of Computer & Information Sciences, Jouf University, Saudi Arabia.

Publication details will be updated when available.

---

## 📝 Citation

If this repository, its implementation, or the MisVid dataset contributes to your research, please cite the associated paper once its publication information becomes available.

The official bibliographic citation will be added following publication.

---

## ⚖️ Usage and Responsible Research

This repository is intended to support academic research on hierarchical classification, multimodal learning, misleading-video analysis, and explainable artificial intelligence.

The source code, pretrained models, and external datasets may be subject to different licenses or usage conditions.

Users should respect applicable copyright, privacy, platform, and research-ethics requirements.

The framework is a research system and should not be treated as an independently verified fact-checking authority.

---

## 📬 Contact

For questions, implementation issues, or research-related discussions, please use the repository's [GitHub Issues](https://github.com/AJFaisal002/CrossModal-Misleading-Video-Detection/issues).

---

<div align="center">

### 🌳 Beyond Binary Detection: Understanding What Makes a Video Misleading

**Hierarchical Classification · Multimodal Learning · Evidence-Based Explainability**

[⬆ Back to Top](#-safe-or-misleading-and-why)

</div>
