# Awesome Video Copy Localization

A curated list of papers, datasets, benchmarks, methods, and related resources for **Video Copy Localization (VCL)**, **Partial Video Copy Detection (PVCD)**, **Video Copy Detection (VCD)**, **video similarity**, and **video deduplication**.

> **Scope.** The main focus is segment-level video copy localization: given a pair of videos, identify which temporal segment in one video corresponds to which temporal segment in the other. Closely related VCD, PVCD, video similarity, retrieval, and deduplication works are included when they contribute important components such as representation learning, candidate retrieval, sparse matching, or temporal alignment.

---

## Contents

- [Task Definition](#task-definition)
- [Standard VCL Pipeline](#standard-vcl-pipeline)
- [Datasets and Benchmarks](#datasets-and-benchmarks)
- [Core Video Copy Localization Methods](#core-video-copy-localization-methods)
- [Efficient Video Copy Localization and Detection](#efficient-video-copy-localization-and-detection)
- [Video Similarity and Representation Learning](#video-similarity-and-representation-learning)
- [Weakly and Self-Supervised VCL](#weakly-and-self-supervised-vcl)
- [Multimodal Video Copy Detection](#multimodal-video-copy-detection)
- [Competition and Challenge Methods](#competition-and-challenge-methods)
- [Method Taxonomy](#method-taxonomy)
- [Challenges](#challenges)
- [Publication Timeline](#publication-timeline)
- [Related Tasks](#related-tasks)

---

# Task Definition

## Video Copy Localization

Given two videos, the goal is to identify the copied temporal segments and their correspondence:

```text
Video A
───────[===========]────────
       startA     endA
             ↕
Video B
──────────[===========]─────
          startB     endB
```

The output is typically:

```text
(startA, endA) ↔ (startB, endB)
```

The task is harder than video-level copy detection because the model must not only determine whether copied content exists, but also precisely localize its temporal boundaries.

---

# Standard VCL Pipeline

A typical VCL pipeline can be summarized as:

```text
Video A / Video B
        ↓
① Frame Sampling
        ↓
② Feature Extraction
        ↓
③ Frame-to-Frame / Region Matching
        ↓
④ Similarity Representation
   usually an M × N similarity matrix
        ↓
⑤ Temporal Alignment / Detection
        ↓
⑥ Boundary Localization / Refinement
        ↓
Copied Segments
```

A simple version is:

```text
Frame features
    ↓
Cosine similarity
    ↓
Frame-to-frame similarity matrix
    ↓
DP / DTW / SPD / temporal detector
    ↓
Segment boundaries
```

Most VCL papers modify one or more stages of this pipeline rather than replacing the entire framework.

---

# Datasets and Benchmarks

## VCSL — CVPR 2022

**A Large-Scale Comprehensive Dataset and Copy-Overlap Aware Evaluation Protocol for Segment-Level Video Copy Detection**

- **Venue:** CVPR 2022
- **Type:** Dataset / benchmark
- Large-scale benchmark for segment-level video copy detection and localization.
- Provides copied segment annotations.
- Includes realistic copy transformations.
- Introduces copy-overlap-aware evaluation.

Representative transformations include:

```text
crop
resize
picture-in-picture
text / logo overlay
filtering
background change
camcording
deepfake / editing
temporal modifications
```

Repository: `alipay/VCSL`

---

## FiGVCL — IEEE TPAMI 2025

**FiGVCL: Fine-Grained Benchmark and Method for Video Copy Localization**

- **Venue:** IEEE TPAMI, 2025
- **Type:** Benchmark + method
- Focuses on fine-grained temporal correspondence.
- Contains challenging real-world editing operations.
- Moves evaluation beyond coarse segment overlap toward finer temporal correspondence.

```text
Coarse copied segment annotation
             ↓
Fine-grained temporal correspondence
```

---

# Core Video Copy Localization Methods

## ViSiL — ICCV 2019

**ViSiL: Fine-Grained Spatio-Temporal Video Similarity Learning**

Although ViSiL is primarily a video similarity method rather than a complete VCL framework, it strongly influenced later fine-grained matching methods.

### Pipeline

```text
Frames
   ↓
Regional CNN Features
   ↓
Frame-to-Frame Regional Similarity
   ↓
[Modified] Chamfer Similarity
   ↓
[Modified] CNN Similarity Refinement
   ↓
Video Similarity Score
```

### Main contribution

Moves beyond a single global video descriptor and performs fine-grained regional comparison.

**Modified stages:** representation + similarity modeling.

---

## VSAL — ACM MM 2021

**Video Similarity and Alignment Learning on Partial Video Copy Detection**

### Pipeline

```text
Frame Features
   ↓
Spatial Similarity Map
   ↓
[Modified] Mask Map
   +
[Modified] Step Map
   ↓
Partial Temporal Alignment
   ↓
Copied Segment
```

### Main contribution

Instead of applying a fixed alignment rule directly on a cosine-similarity matrix, VSAL learns:

- whether a location belongs to an alignment path;
- how the alignment path should move.

This helps reduce the bias of purely spatial similarity and introduces explicit temporal alignment learning.

**Modified stage:** temporal alignment.

---

## SSAN / SPD — ACM MM 2021

**Learning Segment Similarity and Alignment in Large-Scale Content Based Video Retrieval**

Important components include:

- **SKE:** Self-Supervised Keyframe Extraction
- **SPD:** Similarity Pattern Detection

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
[Modified] Similarity Pattern Detection
   ↓
Copied Segment
```

### Main contribution

Treats copied regions in the similarity map as structured visual patterns rather than relying only on handcrafted alignment rules.

**Modified stages:** frame sampling + temporal localization.

---

## TransVCL — AAAI 2023

**TransVCL: Attention-Enhanced Video Copy Localization Network with Flexible Supervision**

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
[Modified] Learned Correlation / Similarity Map
   ↓
Temporal Alignment
   ↓
Copied Segment
```

### Main contribution

Allows the two videos to interact before similarity construction, producing a more discriminative similarity representation.

**Modified stages:** feature enhancement + similarity-map construction.

---

## VCDT — ICANN 2023

**Learning Video Localization on Segment-Level Video Copy Detection with Transformer**

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

### Main contribution

Replaces classical alignment-based localization with a DETR-like transformer segment detector.

**Modified stage:** temporal localization.

---

## RTR — ECCV 2024

**Self-Supervised Video Copy Localization with Regional Token Representation**

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

### Training

```text
[Modified] Transitivity-Based
Self-Supervised Data Generation
```

### Main contribution

A single global frame representation can fail under local editing operations such as:

```text
crop
picture-in-picture
partial-region copy
overlay
```

RTR therefore introduces regional tokens and self-supervised training based on transitivity.

**Modified stages:** regional representation + training strategy.

---

## iESTA — IEEE TCSVT 2025

**iESTA: Instance-Enhanced Spatial-Temporal Alignment for Video Copy Localization**

- **Early access:** 2024
- **Formal volume:** IEEE TCSVT, 2025

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
Spatial-Temporal Alignment
   ↓
Copied Segment
```

### Main contribution

Introduces instance-level information in addition to global frame correspondence.

**Modified stages:** representation + spatial matching + temporal matching.

---

## FiGVCL — IEEE TPAMI 2025

### Pipeline

```text
Frames
   ↓
[Modified] Fine-Grained Local Representation
   ↓
[Modified] Fine-Grained Spatio-Temporal Matching
   ↓
Fine-Grained Temporal Correspondence
   ↓
Copied Segment
```

### Main contribution

Moves from coarse frame-level correspondence toward fine-grained local and temporal correspondence.

**Modified stages:** local representation + matching granularity + evaluation.

---

# Efficient Video Copy Localization and Detection

## Fast Partial Video Copy Detection — WACV 2022

**A Fast Partial Video Copy Detection Using KNN and Global Feature Database**

### Standard approach

```text
Query Video
    ×
Every Reference Video
    ↓
Dense Pairwise Matching
```

### Proposed pipeline

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

### Main contribution

Transforms exhaustive pairwise search into retrieval-first matching.

**Modified stage:** candidate retrieval.

---

## Fast Video Deduplication and Localization with Temporal Consistence Re-Ranking — IEEE TCSVT 2024

### Offline pipeline

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
[Modified] Independent k-d Trees
```

The three feature families are reduced separately and indexed separately.

### Online pipeline

```text
Query Clip
   ↓
Sparse Query Frames
   ↓
FV / VGG / Thumbnail Features
   ↓
PCA
   ↓
Three k-d Tree KNN Searches
   ↓
Merge Sparse Candidate Frame Matches
   ↓
[Modified] Video-ID Consistency Pruning
   ↓
[Modified] Temporal Consistency Pruning
   ↓
Common Video ID
   ↓
Recover Temporal Chain
   ↓
Video ID + Temporal Location
```

### Key idea

Avoid constructing a full frame-to-frame similarity matrix:

```text
Dense matching
M × N
   ↓
Sparse matching
M × K
```

where `K << N`.

Temporal consistency is used to remove visually similar but temporally inconsistent matches.

**Modified stages:** sparse retrieval + temporal consistency.

---

## MLT-Dedup — ACM SIGKDD 2026

**MLT-Dedup: Efficient Large-Scale Online Video Deduplication via Multi-Level Representations and Spatial-Temporal Matching**

### Pipeline

```text
Large Video Repository
   ↓
[Modified] Multi-Level Representation
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

### Main contribution

Uses cheap clip-level representations for large-scale retrieval and more expensive frame-level representations only for candidate videos.

```text
Cheap retrieval
      ↓
Candidate selection
      ↓
Fine-grained verification
```

**Modified stages:** hierarchical representation + candidate retrieval + fine matching.

---

## Extremely Compact Video Representation — Pattern Recognition 2025

**Extremely Compact Video Representation for Efficient Near-Duplicates Detection**

### Pipeline

```text
Video
   ↓
[Modified] Inter-Frame Difference
   ↓
Keyframe Selection
   ↓
[Modified] Miniature Frames
   ↓
Lightweight Siamese CNN
   ↓
Compact Descriptor
```

### Main contribution

Reduces:

- number of processed frames;
- image resolution;
- feature size;
- storage cost;
- matching cost.

**Modified stages:** sampling + compact representation.

---

## Counteracting Temporal Attacks — ACIIDS 2025

**Counteracting Temporal Attacks in Video Copy Detection**

### Pipeline

```text
Video
   ↓
[Modified] Local Maximum
of Inter-Frame Difference
   ↓
Representative Frames
   ↓
Standard Video Copy Detection
```

### Main contribution

Improves robustness to temporal attacks while reducing redundant frame representations.

**Modified stage:** frame sampling.

---

## Efficient Logic Gate Networks for Video Copy Detection — ACIIDS 2026

### Pipeline

```text
Frame
   ↓
Miniaturization
   ↓
Binary Preprocessing
   ↓
[Modified] Logic Gate Network
   ↓
Boolean / Binary Descriptor
   ↓
Fast Matching
```

### Main contribution

Replaces expensive floating-point neural descriptors with extremely lightweight Boolean computation.

**Modified stage:** feature extraction.

---

# Video Similarity and Representation Learning

## S²VS — CVPR Workshops 2023

**Self-Supervised Video Similarity Learning**

### Pipeline

```text
Unlabeled Videos
   ↓
Self-Supervised Representation Learning
   ↓
[Modified] Video Similarity Features
   ↓
Retrieval / Matching
```

### Main contribution

Learns general-purpose video similarity representations from unlabeled data.

**Modified stage:** representation learning.

---

## FCPL — arXiv 2023 / VSC22 Challenge

**Feature-Compatible Progressive Learning for Video Copy Detection**

### Pipeline

```text
Multiple Visual Backbones
   ↓
[Modified] Feature-Compatible Progressive Learning
   ↓
Compatible Embedding Space
   ↓
Feature Ensemble
   ↓
Matching / Localization
```

### Main contribution

Makes representations from different models compatible enough to be compared and fused.

**Modified stage:** representation learning.

---

# Weakly and Self-Supervised VCL

## VCSA — ICASSP 2025

**VCSA: Video Copy Localization Via Single Frame Annotation**

### Standard supervision

```text
Copied Segment
[start, end]
   ↓
VCL Training
```

### VCSA

```text
[Modified] Single-Frame Annotation
   ↓
Weakly-Supervised Learning
   ↓
Copied Segment Localization
```

### Main contribution

Reduces expensive segment-level annotation requirements.

**Modified stage:** supervision.

---

## RTR — ECCV 2024

RTR also introduces transitivity-based self-supervised data generation to reduce dependence on manually annotated copied segments.

---

# Multimodal Video Copy Detection

## Transformer-Based Audio-Visual Video Copy Detection — 2026

### Pipeline

```text
Video
 ├─ Visual Stream
 └─ Audio Stream
       ↓
[Modified] Multimodal Feature Extraction
       ↓
[Modified] Transformer Self/Cross-Attention
       ↓
Similarity Representation
       ↓
[Modified] Temporal Localization
       ↓
Copied Segment
```

### Main contribution

Uses audio as complementary evidence when visual correspondence is weakened by heavy editing.

**Modified stages:** modality + representation + matching + localization.

---

# Competition and Challenge Methods

## SAM — arXiv 2023 / VSC22 Challenge

**A Similarity Alignment Model for Video Copy Segment Matching**

### Pipeline

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

### Main idea

```text
Align → Refine
```

**Modified stages:** alignment + boundary refinement.

---

## Dual-Level Detection — arXiv 2023 / VSC22 Challenge

### Pipeline

```text
Video
 ├─ Video-Level Detection
 └─ Frame-Level Scene Detection
          ↓
      Copy Decision
```

### Main contribution

Combines coarse video-level and detailed frame-level evidence.

---

# Method Taxonomy

```text
Video
 │
 ▼
① Frame Sampling
 │
 ├── Uniform Sampling
 │     ├── ViSiL — ICCV 2019
 │     ├── VSAL — ACM MM 2021
 │     ├── VCSL Baselines — CVPR 2022
 │     ├── TransVCL — AAAI 2023
 │     └── RTR — ECCV 2024
 │
 ├── Self-Supervised Keyframe Selection
 │     └── SSAN / SKE — ACM MM 2021
 │
 └── Inter-Frame Difference
       ├── Extremely Compact Video Representation
       │      — Pattern Recognition 2025
       └── Counteracting Temporal Attacks
              — ACIIDS 2025
 │
 ▼
② Feature Extraction / Representation
 │
 ├── Global CNN Features
 │     ├── ViSiL — ICCV 2019
 │     ├── Fast Partial VCD — WACV 2022
 │     └── Fast Video Deduplication — TCSVT 2024
 │
 ├── Transformer / ViT Features
 │     ├── TransVCL — AAAI 2023
 │     └── RTR — ECCV 2024
 │
 ├── Regional Features
 │     ├── ViSiL — ICCV 2019
 │     ├── RTR — ECCV 2024
 │     └── FiGVCL — TPAMI 2025
 │
 ├── Instance-Level Features
 │     └── iESTA — TCSVT 2025
 │
 ├── Self-Supervised Features
 │     ├── S²VS — CVPRW 2023
 │     └── RTR — ECCV 2024
 │
 ├── Multi-Backbone Features
 │     └── FCPL — arXiv 2023 / VSC22
 │
 ├── Compact Features
 │     ├── Extremely Compact Video Representation
 │     │      — Pattern Recognition 2025
 │     └── Logic Gate Networks
 │            — ACIIDS 2026
 │
 └── Multimodal Features
       └── Transformer Audio-Visual VCD — 2026
 │
 ▼
③ Candidate Retrieval
 │
 ├── KNN Global Database
 │     └── Fast Partial VCD — WACV 2022
 │
 ├── k-d Tree
 │     └── Fast Video Deduplication and Localization
 │            — TCSVT 2024
 │
 └── HNSW
       └── MLT-Dedup — KDD 2026
 │
 ▼
④ Frame / Region Matching
 │
 ├── Cosine Similarity
 │     ├── VCSL Baselines — CVPR 2022
 │     ├── VSAL — ACM MM 2021
 │     └── TransVCL — AAAI 2023
 │
 ├── Chamfer Similarity
 │     └── ViSiL — ICCV 2019
 │
 ├── Learned Correlation / Similarity
 │     ├── VSAL — ACM MM 2021
 │     └── TransVCL — AAAI 2023
 │
 ├── Regional Matching
 │     ├── RTR — ECCV 2024
 │     └── FiGVCL — TPAMI 2025
 │
 ├── Instance-Level Matching
 │     └── iESTA — TCSVT 2025
 │
 └── Multimodal Matching
       └── Transformer Audio-Visual VCD — 2026
 │
 ▼
⑤ Similarity Representation
 │
 ├── Dense Similarity Matrix
 │     ├── VSAL — ACM MM 2021
 │     ├── SSAN / SPD — ACM MM 2021
 │     ├── TransVCL — AAAI 2023
 │     ├── VCDT — ICANN 2023
 │     ├── RTR — ECCV 2024
 │     ├── iESTA — TCSVT 2025
 │     └── FiGVCL — TPAMI 2025
 │
 ├── Refined Similarity Matrix
 │     ├── ViSiL — ICCV 2019
 │     ├── VSAL — ACM MM 2021
 │     └── TransVCL — AAAI 2023
 │
 └── Sparse Correspondences
       ├── Fast Partial VCD — WACV 2022
       ├── Fast Video Deduplication — TCSVT 2024
       └── MLT-Dedup — KDD 2026
 │
 ▼
⑥ Temporal Alignment / Detection
 │
 ├── Dynamic Programming / DTW
 │     └── Traditional VCL / VCSL baselines
 │
 ├── Similarity Pattern Detection
 │     └── SSAN / SPD — ACM MM 2021
 │
 ├── Mask + Step Prediction
 │     └── VSAL — ACM MM 2021
 │
 ├── Transformer Alignment
 │     ├── TransVCL — AAAI 2023
 │     └── iESTA — TCSVT 2025
 │
 ├── Transformer Segment Detection
 │     └── VCDT — ICANN 2023
 │
 ├── Align → Refine
 │     └── SAM — arXiv 2023 / VSC22
 │
 ├── Temporal Consistency
 │     ├── Fast Video Deduplication
 │     │      — TCSVT 2024
 │     └── MLT-Dedup — KDD 2026
 │
 └── Fine-Grained Temporal Correspondence
       └── FiGVCL — TPAMI 2025
 │
 ▼
⑦ Boundary Localization / Refinement
 │
 ├── DP / Alignment Boundary
 │     └── Traditional VCL
 │
 ├── Similarity-Pattern Boundary
 │     └── SSAN / SPD — ACM MM 2021
 │
 ├── Transformer Boundary Prediction
 │     └── VCDT — ICANN 2023
 │
 ├── Alignment Refinement
 │     ├── SAM — arXiv 2023 / VSC22
 │     └── TransVCL — AAAI 2023
 │
 ├── Temporal-Chain Localization
 │     └── Fast Video Deduplication
 │            — TCSVT 2024
 │
 └── Fine-Grained Boundary / Correspondence
       └── FiGVCL — TPAMI 2025
 │
 ▼
Copied Segment
```

---

# Challenges

## C1. Robustness to Spatial Transformations and Partial Copying

### Problem

Real copied content may undergo:

```text
crop
resize
picture-in-picture
watermark
text / logo overlay
filtering
background replacement
camcording
partial-region copying
```

These transformations can severely weaken simple global frame similarity.

A major difficulty is that:

```text
same source content
≠
same global appearance
```

### Representative works

- **ViSiL — ICCV 2019:** regional visual similarity.
- **RTR — ECCV 2024:** regional token representation for PIP and local copy.
- **iESTA — TCSVT 2025:** instance-level correspondence.
- **FiGVCL — TPAMI 2025:** fine-grained local embeddings and correspondence.

### Evolution

```text
Global Frame Feature
        ↓
Regional Feature
        ↓
Instance-Level Feature
        ↓
Fine-Grained Local Feature
```

---

## C2. Robustness to Temporal Transformations

### Problem

Copied videos may be temporally edited through:

```text
speed up
slow down
frame insertion
frame deletion
frame duplication
temporal shift
segment trimming
segment rearrangement
```

This breaks simple one-to-one frame correspondence and can distort the diagonal structure in a similarity matrix.

### Representative works

- **VSAL — ACM MM 2021:** learned partial temporal alignment.
- **TransVCL — AAAI 2023:** sequence-level attention and correlation.
- **iESTA — TCSVT 2025:** spatial-temporal alignment.
- **Counteracting Temporal Attacks — ACIIDS 2025:** explicit temporal-attack robustness.

---

## C3. Fine-Grained Temporal Correspondence and Boundary Localization

### Problem

Video-level or segment-level detection is easier than precisely recovering:

```text
startA
endA
startB
endB
```

Short copied segments are especially difficult because they occupy only a small region in the similarity representation.

### Representative works

- **SSAN / SPD — ACM MM 2021:** similarity-pattern localization.
- **VCDT — ICANN 2023:** transformer segment prediction.
- **TransVCL — AAAI 2023:** alignment and refinement.
- **FiGVCL — TPAMI 2025:** fine-grained temporal correspondence.
- **Audio-Visual VCD — 2026:** temporal localization for difficult/short copies.

---

## C4. Copy-Aware Matching vs. Generic Visual Similarity

### Problem

Two frames can look similar without being copies.

Examples:

```text
same football field
same TV studio
same person
same event category
same background
```

but:

```text
visual similarity ≠ copy identity
```

This creates hard negatives.

The actual question is not merely:

> Do these frames look similar?

but:

> Do these frames originate from the same copied source content?

### Representative works

- **VSAL — ACM MM 2021:** combines spatial similarity with temporal alignment.
- **Fast Video Deduplication — TCSVT 2024:** temporal consistency removes isolated visual false matches.
- **TransVCL — AAAI 2023:** contextualized cross-video representation.
- **FiGVCL — TPAMI 2025:** fine-grained correspondence rather than only coarse similarity.

---

## C5. Computational Efficiency and Scalability

### Problem

For:

```text
Query = M frames
Reference = N frames
```

dense matching requires approximately:

```text
M × N
```

frame comparisons.

At repository scale:

```text
1 query
   ×
millions of reference frames / videos
```

this becomes expensive in compute, memory, storage, and latency.

### Representative works

- **Fast Partial VCD — WACV 2022:** KNN-based candidate retrieval.
- **Fast Video Deduplication — TCSVT 2024:** k-d tree retrieval + sparse temporal consistency.
- **MLT-Dedup — KDD 2026:** multi-level representation + HNSW + fine-grained verification.
- **Extremely Compact Video Representation — Pattern Recognition 2025:** compact frame representation.
- **Logic Gate Networks — ACIIDS 2026:** lightweight binary representation.

### General trend

```text
Dense Matching
      ↓
Sparse Retrieval
      ↓
Hierarchical / Coarse-to-Fine Matching
```

---

## C6. Expensive Segment-Level Annotation

### Problem

Supervised VCL often requires manually annotating:

```text
Video A: startA / endA
Video B: startB / endB
```

for every copied segment.

This is expensive, especially when:

- one video pair contains multiple copied regions;
- boundaries are ambiguous;
- fine-grained temporal correspondence is required.

### Representative works

- **TransVCL — AAAI 2023:** flexible supervision.
- **RTR — ECCV 2024:** transitivity-based self-supervised data construction.
- **VCSA — ICASSP 2025:** single-frame annotation.
- **FiGVCL — TPAMI 2025:** improves fine-grained annotation/evaluation despite higher labeling complexity.

---

## C7. Limited Visual-Only Evidence

### Problem

Heavy visual editing may destroy much of the visual correspondence:

```text
strong crop
overlay
PIP
severe degradation
camcording
background replacement
```

However, other modalities may remain informative.

### Representative works

- **Transformer-Based Audio-Visual VCD — 2026:** visual + audio evidence.

### Potential evidence sources

```text
Visual
Audio
OCR text
ASR text
Metadata
```

---

## C8. Cross-Domain and Real-World Generalization

### Problem

A method that performs well on one benchmark may fail under:

```text
new platforms
new content domains
new editing styles
unseen transformations
different video qualities
different capture devices
```

Real-world copy operations continually evolve.

### Representative resources / works

- **VCSL — CVPR 2022:** large-scale realistic copy benchmark.
- **RTR — ECCV 2024:** self-supervised learning for broader robustness.
- **FiGVCL — TPAMI 2025:** challenging real-world fine-grained benchmark.
- **S²VS — CVPRW 2023:** self-supervised video similarity representation.

---

# Challenge-to-Method Map

| Challenge | Representative Works |
|---|---|
| Spatial transformations / local copying | ViSiL (ICCV 2019), RTR (ECCV 2024), iESTA (TCSVT 2025), FiGVCL (TPAMI 2025) |
| Temporal transformations | VSAL (MM 2021), TransVCL (AAAI 2023), iESTA (TCSVT 2025), Counteracting Temporal Attacks (ACIIDS 2025) |
| Fine-grained localization | SSAN/SPD (MM 2021), VCDT (ICANN 2023), TransVCL (AAAI 2023), FiGVCL (TPAMI 2025) |
| Hard negatives / similarity ≠ copy | VSAL (MM 2021), TransVCL (AAAI 2023), Fast Video Deduplication (TCSVT 2024), FiGVCL (TPAMI 2025) |
| Efficiency / scalability | Fast Partial VCD (WACV 2022), Fast Video Deduplication (TCSVT 2024), Extremely Compact Representation (PR 2025), MLT-Dedup (KDD 2026) |
| Annotation cost | TransVCL (AAAI 2023), RTR (ECCV 2024), VCSA (ICASSP 2025) |
| Visual-only limitation | Audio-Visual VCD (2026) |
| Generalization | VCSL (CVPR 2022), S²VS (CVPRW 2023), RTR (ECCV 2024), FiGVCL (TPAMI 2025) |

---

# Publication Timeline

| Year | Work | Venue |
|---|---|---|
| 2019 | ViSiL | ICCV |
| 2021 | VSAL | ACM MM |
| 2021 | SSAN | ACM MM |
| 2022 | VCSL | CVPR |
| 2022 | Fast Partial Video Copy Detection | WACV |
| 2023 | TransVCL | AAAI |
| 2023 | VCDT | ICANN |
| 2023 | S²VS | CVPR Workshops |
| 2023 | FCPL | arXiv / VSC22 Challenge |
| 2023 | SAM | arXiv / VSC22 Challenge |
| 2023 | Dual-Level Detection | arXiv / VSC22 Challenge |
| 2024 | RTR | ECCV |
| 2024 | Fast Video Deduplication and Localization | IEEE TCSVT |
| 2025 | iESTA | IEEE TCSVT |
| 2025 | FiGVCL | IEEE TPAMI |
| 2025 | VCSA | ICASSP |
| 2025 | Extremely Compact Video Representation | Pattern Recognition |
| 2025 | Counteracting Temporal Attacks | ACIIDS |
| 2026 | MLT-Dedup | ACM SIGKDD |
| 2026 | Efficient Logic Gate Networks | ACIIDS |
| 2026 | Transformer-Based Audio-Visual VCD | Journal article |

---

# Related Tasks

## Video Copy Detection

```text
Video A + Video B
        ↓
Copy / Non-Copy
```

Main question:

> Do the two videos contain copied content?

---

## Partial Video Copy Detection

```text
Query Video
    ↓
Reference Repository
    ↓
Detect whether a partial copy exists
```

Often combines retrieval and temporal verification.

---

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

Main question:

> Is this video already represented in the repository?

Some modern deduplication systems also localize the duplicated temporal portion.

---

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

Main question:

> Which temporal segment of Video A corresponds to which temporal segment of Video B?

---

## Video Similarity

```text
Video A + Video B
        ↓
Similarity Model
        ↓
Similarity Score
```

A similarity model is often used as a component inside VCD/VCL, but high visual or semantic similarity alone does not necessarily imply copy identity.

---

# High-Level Evolution of the Field

A simplified view of the field is:

```text
Global Visual Similarity
        ↓
Fine-Grained Regional Similarity
        ↓
Learned Temporal Alignment
        ↓
Transformer-Based Matching
        ↓
Regional / Instance-Level Correspondence
        ↓
Fine-Grained Temporal Correspondence
```

In parallel, efficiency-oriented work evolves roughly as:

```text
Dense Frame-to-Frame Matching
        ↓
KNN / ANN Candidate Retrieval
        ↓
Sparse Temporal Correspondence
        ↓
Multi-Level / Hierarchical Retrieval
```

Supervision evolves roughly as:

```text
Full Segment Annotation
        ↓
Flexible / Weak Supervision
        ↓
Self-Supervised Data Construction
        ↓
Single-Frame Annotation
```

---

# Topics

`video-copy-localization` · `video-copy-detection` · `partial-video-copy-detection` · `video-similarity` · `video-deduplication` · `temporal-alignment` · `near-duplicate-video-retrieval` · `video-retrieval` · `fine-grained-video-matching`

---

# Contributing

Contributions are welcome.

Useful additions include:

- newly published VCL / PVCD / VCD papers;
- datasets and benchmarks;
- official code repositories;
- evaluation protocols;
- efficiency-oriented methods;
- multimodal copy detection;
- self-supervised / weakly supervised localization;
- surveys and reproducibility resources.

Please keep each entry concise and, when possible, include:

```text
Paper title
Venue + year
Paper link
Code link
Dataset link
Modified pipeline stage
One-sentence contribution
```

---

# Disclaimer

This repository groups closely related tasks together for research convenience. Some included works focus primarily on video similarity, retrieval, copy detection, or deduplication rather than strict segment-level Video Copy Localization. Their relevance is indicated by the pipeline component they contribute to.
