# Awesome Video Copy Localization

A curated list of papers, datasets, benchmarks, and resources for **Video Copy Localization (VCL)**, **Partial Video Copy Detection (PVCD)**, **Video Copy Detection (VCD)**, and related video deduplication tasks.

## Overview

```text
Video Pair
   ↓
Frame Sampling
   ↓
Feature Extraction
   ↓
Frame-to-Frame Matching
   ↓
Similarity Matrix
   ↓
Temporal Alignment / Detection
   ↓
Boundary Refinement
   ↓
Copied Segments
```

---

# Datasets & Benchmarks

## VCSL — CVPR 2022

**A Large-Scale Comprehensive Dataset and Copy-Overlap Aware Evaluation Protocol for Segment-Level Video Copy Detection**

- Large-scale segment-level video copy benchmark.
- Provides copied segment annotations.
- Introduces copy-overlap-aware evaluation.

## FiGVCL

**Fine-Grained Video Copy Localization**

- Fine-grained temporal correspondence.
- Realistic transformations and editing.
- More detailed temporal correspondence evaluation.

---

# Core Video Copy Localization

## VSAL

```text
Frame Features
   ↓
Spatial Similarity Map
   ↓
[Modified] Mask Map + Step Map
   ↓
Partial Alignment
   ↓
Copied Segment
```

**Modified:** Temporal alignment.

---

## SPD / SSAN

```text
Video
   ↓
[Modified] Self-Supervised Keyframe Extraction
   ↓
Frame Features
   ↓
Similarity Matrix
   ↓
[Modified] Similarity Pattern Detector
   ↓
Copied Segment
```

**Modified:** Sampling + temporal localization.

---

## TransVCL — AAAI 2023

```text
Frame Features
   ↓
[Modified] Self-Attention / Cross-Attention
   ↓
Enhanced Features
   ↓
[Modified] Correlation Map
   ↓
Temporal Alignment
   ↓
Copied Segment
```

**Modified:** Feature enhancement + similarity construction.

---

## RTR — ECCV 2024

```text
Frames
   ↓
ViT
   ↓
Global Token + [Modified] Regional Tokens
   ↓
Similarity Matrix
   ↓
Temporal Detector
   ↓
Copied Segment
```

**Modified:** Regional representation + self-supervised training.

---

## iESTA

```text
Frames
   ↓
Global Features
   +
[Modified] Instance Features
   ↓
Instance Relation Graph
   ↓
Temporal Transformer
   ↓
Feature Fusion
   ↓
Alignment
```

**Modified:** Instance-level spatial-temporal representation.

---

## FiGVCL

```text
Frames
   ↓
[Modified] Fine-Grained Local Representation
   ↓
[Modified] Fine-Grained Spatio-Temporal Matching
   ↓
Temporal Correspondence
   ↓
Copied Segment
```

**Modified:** Local representation + fine-grained matching.

---

## VCDT

```text
Similarity Representation
   ↓
[Modified] Transformer Encoder-Decoder
   ↓
Segment Queries
   ↓
Boundary + Confidence
```

**Modified:** Temporal localization.

---

# Efficient Video Copy Localization & Detection

## Fast Partial Video Copy Detection — WACV 2022

```text
Reference Frames
   ↓
CNN Features
   ↓
[Modified] Global KNN / ANN Index
   ↓
Query Search
   ↓
Candidate Videos
   ↓
Temporal Matching
   ↓
Copied Segment
```

**Modified:** Candidate retrieval.

---

## Fast Video Deduplication and Localization with Temporal Consistence Re-Ranking — TCSVT 2024

### Offline

```text
Database Videos
   ↓
Fisher / VGG / Thumbnail
   ↓
PCA
   ↓
[Modified] Independent k-d Trees
```

### Online

```text
Query Frames
   ↓
k-d Tree KNN Search
   ↓
Sparse Candidate Matches
   ↓
[Modified] Video-ID Consistency
   ↓
[Modified] Temporal Consistency
   ↓
Video ID + Temporal Location
```

**Modified:** Sparse retrieval + temporal consistency.

---

## MLT-Dedup

```text
Video Repository
   ↓
[Modified] Multi-Level Representation
   ↓
Sparse Clip Embedding
   ↓
HNSW Retrieval
   ↓
Candidate Videos
   ↓
Frame-Level Features
   ↓
[Modified] Spatial-Temporal Matching
   ↓
Duplicated Segment
```

**Modified:** Hierarchical retrieval + fine matching.

---

# Efficient Video Representation

## Extremely Compact Video Representation

```text
Video
   ↓
[Modified] Inter-Frame Difference
   ↓
Keyframe Selection
   ↓
[Modified] Miniature Frame
   ↓
Lightweight Siamese Network
   ↓
Compact Descriptor
```

---

## Temporal-Attack-Aware Frame Selection

```text
Video
   ↓
[Modified] Inter-Frame Difference
   ↓
Representative Frames
   ↓
Standard VCD Pipeline
```

---

## Logic Gate Network

```text
Frame
   ↓
Miniaturization
   ↓
[Modified] Logic Gate Network
   ↓
Boolean Descriptor
   ↓
Fast Matching
```

---

# Video Similarity Representation

## ViSiL — ICCV 2019

```text
Frames
   ↓
Regional CNN Features
   ↓
Frame Similarity
   ↓
[Modified] Chamfer Similarity
   ↓
[Modified] CNN Refinement
   ↓
Video Similarity
```

---

## S²VS

```text
Unlabeled Videos
   ↓
Self-Supervised Learning
   ↓
[Modified] Video Similarity Representation
   ↓
Retrieval / Matching
```

---

## FCPL

```text
Multiple Backbones
   ↓
[Modified] Feature-Compatible Progressive Learning
   ↓
Compatible Feature Space
   ↓
Feature Ensemble
   ↓
Matching
```

---

# Weakly / Self-Supervised VCL

## VCSA

```text
Single-Frame Annotation
   ↓
[Modified] Weakly-Supervised Learning
   ↓
Copied Segment Localization
```

## RTR

Uses transitivity-based self-supervision to reduce reliance on manually annotated copied segments.

---

# Multimodal Video Copy Detection

## Audio-Visual Video Copy Detection

```text
Video
 ├─ Visual
 └─ Audio
      ↓
[Modified] Multimodal Features
      ↓
Cross-Modal Matching
      ↓
Similarity Representation
      ↓
Temporal Localization
```

---

# Competition Methods

## SAM

```text
Frame Features
   ↓
Similarity Map
   ↓
[Modified] Alignment
   ↓
[Modified] Refinement
   ↓
Copied Segment
```

## Dual-Level Detection

```text
Video
 ├─ Video-Level Detection
 └─ Frame-Level Scene Detection
          ↓
      Copy Decision
```

---

# Method Taxonomy

```text
Video
 │
 ▼
① Frame Sampling
 │
 ├── Uniform Sampling
 │     ├── VCSL baselines
 │     ├── TransVCL
 │     └── RTR
 │
 ├── Keyframe Selection
 │     ├── SPD / SSAN
 │     └── Extremely Compact Video Representation
 │
 └── Inter-Frame Difference
       ├── Extremely Compact Video Representation
       └── Temporal-Attack-Aware Frame Selection
 │
 ▼
② Feature Extraction / Representation
 │
 ├── Global CNN Features
 │     ├── ViSiL
 │     ├── Fast Partial Video Copy Detection
 │     └── Fast Video Deduplication
 │
 ├── Transformer / ViT Features
 │     ├── TransVCL
 │     └── RTR
 │
 ├── Regional Features
 │     ├── ViSiL
 │     ├── RTR
 │     └── FiGVCL
 │
 ├── Instance-Level Features
 │     └── iESTA
 │
 ├── Self-Supervised Features
 │     ├── S²VS
 │     ├── SPD / SSAN
 │     └── RTR
 │
 ├── Multi-Backbone Features
 │     └── FCPL
 │
 ├── Compact Features
 │     ├── Extremely Compact Video Representation
 │     └── Logic Gate Network
 │
 └── Multimodal Features
       └── Audio-Visual Video Copy Detection
 │
 ▼
③ Candidate Retrieval
 │
 ├── KNN
 │     └── Fast Partial Video Copy Detection
 │
 ├── k-d Tree
 │     └── Fast Video Deduplication and Localization
 │
 ├── FAISS / ANN
 │     └── Fast Partial Video Copy Detection
 │
 └── HNSW
       └── MLT-Dedup
 │
 ▼
④ Frame / Region Matching
 │
 ├── Cosine Similarity
 │     ├── VCSL baselines
 │     ├── TransVCL baseline pipeline
 │     └── Fast retrieval methods
 │
 ├── Chamfer Similarity
 │     └── ViSiL
 │
 ├── Learned Correlation
 │     ├── TransVCL
 │     └── VSAL
 │
 ├── Regional Matching
 │     ├── RTR
 │     └── FiGVCL
 │
 ├── Instance-Level Matching
 │     └── iESTA
 │
 └── Multimodal Matching
       └── Audio-Visual Video Copy Detection
 │
 ▼
⑤ Similarity Representation
 │
 ├── Dense Similarity Matrix
 │     ├── VSAL
 │     ├── SPD
 │     ├── TransVCL
 │     ├── RTR
 │     ├── iESTA
 │     └── FiGVCL
 │
 ├── Refined Similarity Matrix
 │     ├── ViSiL
 │     └── TransVCL
 │
 └── Sparse Correspondences
       ├── Fast Partial Video Copy Detection
       ├── Fast Video Deduplication
       └── MLT-Dedup
 │
 ▼
⑥ Temporal Alignment / Detection
 │
 ├── Dynamic Programming
 │     └── Traditional VCL / VCSL baselines
 │
 ├── DTW
 │     └── Traditional VCL baselines
 │
 ├── SPD / Pattern Detection
 │     └── SPD / SSAN
 │
 ├── Mask + Step Prediction
 │     └── VSAL
 │
 ├── Transformer Alignment
 │     ├── TransVCL
 │     └── iESTA
 │
 ├── Transformer Segment Detection
 │     └── VCDT
 │
 ├── Align → Refine
 │     └── SAM
 │
 ├── Temporal Consistency
 │     ├── Fast Video Deduplication and Localization
 │     └── MLT-Dedup
 │
 └── Fine-Grained Temporal Correspondence
       └── FiGVCL
 │
 ▼
⑦ Boundary Localization / Refinement
 │
 ├── DP / Alignment Boundary
 │     └── Traditional VCL
 │
 ├── Detection-Based Boundary
 │     ├── SPD
 │     └── VCDT
 │
 ├── Alignment Refinement
 │     ├── SAM
 │     └── TransVCL
 │
 ├── Temporal-Chain Boundary
 │     └── Fast Video Deduplication and Localization
 │
 └── Fine-Grained Boundary
       └── FiGVCL
 │
 ▼
Copied Segment
```

---

# Related Tasks

## Video Deduplication

```text
Query Video
   ↓
Large Video Repository
   ↓
Duplicate Retrieval
   ↓
Duplicate / Non-Duplicate
```

## Video Copy Localization

```text
Video A + Video B
        ↓
Copy Matching
        ↓
[startA, endA]
        ↕
[startB, endB]
```

# Topics

`video-copy-localization` · `video-copy-detection` · `partial-video-copy-detection` · `video-similarity` · `video-deduplication` · `temporal-alignment` · `near-duplicate-video-retrieval`
