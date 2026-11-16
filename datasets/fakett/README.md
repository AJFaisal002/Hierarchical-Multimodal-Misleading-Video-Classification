# FakeTT: External Benchmark Dataset

**Cross-dataset evaluation for misleading-video classification**

![Dataset](https://img.shields.io/badge/Dataset-FakeTT-blue)
![Evaluation](https://img.shields.io/badge/Evaluation-External%20Benchmark-green)
![Task](https://img.shields.io/badge/Task-Binary%20Classification-orange)

---

## Overview

FakeTT is an external short-video benchmark used to evaluate the generalization capability of the proposed multimodal misleading-video classification framework.

Unlike MisVid, FakeTT is an independently sourced dataset and is not introduced as a new dataset by this research.

## Dataset Information

| Property | Description |
|---|---|
| Dataset | FakeTT |
| Dataset role | External evaluation |
| Evaluation samples | 1,992 |
| Classification task | Binary |
| Classes | Real and Fake |
| Evaluation purpose | Cross-dataset generalization |

## Evaluation Protocol

FakeTT is used to assess the Stage 1 binary classification component of the hierarchical framework.

The evaluation investigates whether the learned multimodal representations generalize beyond the primary MisVid dataset.

FakeTT is not used to evaluate MisVid's four-category misleading-content taxonomy.

## Dataset Access

FakeTT is publicly available from its original dataset providers.

Users should obtain the dataset from its official source and follow the creators' licensing and usage conditions.

The original dataset files are not redistributed through this repository.

## Associated Research

**Safe or Misleading, and Why? Coarse-to-Fine Multimodal Classification of Short-Form Videos with Evidence-Based Explanations**

## Citation

Researchers using FakeTT should cite the original FakeTT dataset publication in addition to any associated research using this benchmark.

## Contact

For questions about this repository's FakeTT evaluation, please open an issue in the main GitHub repository.
