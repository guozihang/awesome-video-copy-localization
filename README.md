# awesome-video-copy-localization

# Awesome Video Copy Localization

A curated list of papers, datasets, benchmarks, and resources for **Video Copy Localization (VCL)**, **Partial Video Copy Detection (PVCD)**, **Video Copy Detection (VCD)**, and related video deduplication tasks.

## Overview

A typical Video Copy Localization pipeline can be summarized as:

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

Existing work mainly improves one or more of the following components:

- Frame sampling
- Visual representation
- Local / regional matching
- Similarity-map construction
- Temporal alignment
- Boundary localization
- Candidate retrieval
- Sparse matching
- Multimodal matching
- Efficient representation

---

# Datasets & Benchmarks

## VCSL

**A Large-Scale Comprehensive Dataset and Copy-Overlap Aware Evaluation Protocol for Segment-Level Video Copy Detection**  
CVPR 2022

- Large-scale benchmark for segment-level video copy localization.
- Provides copied segment annotations.
- Introduces copy-overlap-aware evaluation.
- Widely used by subsequent VCL methods.

```text
Dataset / Benchmark
```

Repository: `alipay/VCSL`

---

## FiGVCL

**Fine-Grained Video Copy Localization**

- Focuses on fine-grained temporal correspondence.
- Designed for realistic video transformations and editing.
- Introduces more detailed temporal correspondence evaluation.

```text
Coarse Segment Annotation
        ↓
Fine-Grained Temporal Correspondence
```

---

# Core Video Copy Localization

## VSAL

**Video Similarity and Alignment Learning**

### Pipeline

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

### Main Idea

Instead of directly applying conventional alignment algorithms to a cosine-similarity matrix, VSAL learns:

- whether a location belongs to an alignment path;
- how the alignment path should move.

**Modified component:** Temporal alignment.

---

## SPD / SSAN

### Pipeline

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

### Main Idea

Treat diagonal copy patterns in the similarity matrix as a detection problem.

**Modified components:**

- Frame sampling
- Temporal localization

---

## TransVCL

**Transformer Based Video Copy Localization**  
AAAI 2023

### Pipeline

```text
Frame Features
   ↓
[Modified] Self-Attention
   ↓
[Modified] Cross-Attention
   ↓
Enhanced Features
   ↓
[Modified] Correlation / Similarity Map
   ↓
Temporal Alignment
   ↓
Copied Segment
```

### Main Idea

The two videos interact through Transformer attention before constructing the similarity representation.

**Modified components:**

- Feature enhancement
- Similarity-map construction

---

## RTR

**Regional Token Representation for Video Copy Localization**  
ECCV 2024

### Pipeline

```text
Frames
   ↓
Vision Transformer
   ↓
Global Token
   +
[Modified] Regional Tokens
   ↓
Similarity Matrix
   ↓
Temporal Detector
   ↓
Copied Segment
```

Training additionally introduces:

```text
[Modified] Transitivity-Based Self-Supervision
```

### Main Idea

A single global representation per frame may fail under:

- cropping;
- picture-in-picture;
- partial-region copying.

RTR therefore introduces regional representations.

**Modified components:**

- Frame representation
- Training strategy

---

## iESTA

**Instance-Enhanced Spatial-Temporal Alignment for Video Copy Localization**

### Pipeline

```text
Frames
   ↓
Global Features
   +
[Modified] Instance-Level Features
   ↓
Instance Relation Graph
   ↓
Temporal Transformer
   ↓
[Modified] Cross-Level Feature Fusion
   ↓
Alignment
   ↓
Copied Segment
```

### Main Idea

Combine global visual correspondence with object / instance-level correspondence.

**Modified components:**

- Representation
- Spatial matching
- Temporal matching

---

## FiGVCL

### Pipeline

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

### Main Idea

Move from coarse frame-level matching toward fine-grained spatial and temporal correspondence.

**Modified components:**

- Local representation
- Matching granularity
- Evaluation protocol

---

## VCDT

**Video Copy Detection Transformer**

### Pipeline

```text
Similarity Representation
   ↓
[Modified] Transformer Encoder
   ↓
[Modified] Transformer Decoder
   ↓
Segment Queries
   ↓
Boundary + Confidence
```

### Main Idea

Replace traditional:

```text
Similarity Matrix
   ↓
DP / DTW
```

with a DETR-like segment detector.

**Modified component:** Temporal localization.

---

# Efficient Video Copy Localization & Detection

## Fast Partial Video Copy Detection

WACV 2022

### Pipeline

```text
Reference Frames
   ↓
CNN Features
   ↓
[Modified] Global KNN / ANN Index
   ↓

Query Frames
   ↓
KNN Search
   ↓
Candidate Videos
   ↓
Temporal Matching
   ↓
Copied Segment
```

### Main Idea

Avoid comparing a query video with every reference video.

Candidate frames / videos are retrieved first, followed by temporal matching.

**Modified component:** Candidate retrieval.

---

## Fast Video Deduplication and Localization with Temporal Consistence Re-Ranking

TCSVT 2024

### Offline Pipeline

```text
Database Videos
   ↓
Frame Sampling
   ↓
Fisher Vector
VGG Feature
Thumbnail Feature
   ↓
PCA
   ↓
[Modified] Three Independent k-d Trees
```

### Online Pipeline

```text
Query Frames
   ↓
Feature Extraction
   ↓
k-d Tree KNN Search
   ↓
Sparse Candidate Frame Matches
   ↓
[Modified] Video-ID Consistency
   ↓
[Modified] Temporal Consistency Pruning
   ↓
Video ID
   +
Temporal Location
```

### Main Idea

Avoid constructing a complete frame-to-frame similarity matrix.

Instead:

```text
Dense M × N Matching
        ↓
Sparse M × K Matching
```

Temporal consistency is then used to remove isolated false matches and recover the corresponding video segment.

**Modified components:**

- Candidate retrieval
- Sparse frame matching
- Temporal consistency

---

## MLT-Dedup

### Pipeline

```text
Large Video Repository
   ↓
[Modified] Multi-Level Video Representation
   ↓
Sparse Clip Embeddings
   ↓
HNSW Retrieval
   ↓
Candidate Videos
   ↓
Load Fine-Grained Frame Features
   ↓
[Modified] Spatial-Temporal Matching
   ↓
Duplicated Segment
```

### Main Idea

Use lightweight clip-level representations for large-scale retrieval and fine-grained frame representations only for candidate videos.

**Modified components:**

- Hierarchical representation
- Candidate retrieval
- Fine-grained verification

---

# Efficient
