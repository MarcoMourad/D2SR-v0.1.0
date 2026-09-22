# D²SR — Digital Data Signature Report

**A multi-signal digital media authenticity assessment platform**

---

## Overview

D²SR does not ask a single question — *"Is this image Real or Fake?"*

Instead, it asks:

> **"How much independent evidence supports the authenticity of this image?"**

The system runs multiple independent analysis engines, collects their evidence, and combines it into a single explainable authenticity report — instead of relying on one AI model or one piece of metadata.

```mermaid
flowchart TD
    A[Image] --> B[Preprocessing]
    B --> C[Multiple Analysis Engines]
    C --> D[Evidence Collection]
    D --> E[Evidence Aggregation]
    E --> F[Trust Engine]
    F --> G[Authenticity Report]
```

---

## 1. Problem Statement

**AI-Generated Images** Generative models now produce highly realistic images, making visual inspection alone unreliable.

**Image Manipulation** Real images can be altered via copy-move, splicing, removal, or inpainting. Beyond detecting *that* manipulation occurred, the system should identify **where** the suspicious region is.

**Metadata Loss** Metadata is often lost or altered after social media uploads, recompression, screenshots, editing, or file conversion. Metadata cannot be treated as the only source of truth.

**Single-Detector Limitation** A single AI detector can produce false positives/negatives, generalize poorly to unseen generators, and degrade after image transformations. This is why D²SR relies on multiple independent evidence sources rather than one.

---

## 2. Proposed Solution

D²SR combines several independent signals:

| Signal | Purpose |
| --- | --- |
| AI Detection | Detect likely AI-generated images |
| Metadata | Analyze available image metadata |
| Hash / Fingerprinting | Detect similarity and related images |
| Image Integrity | Analyze compression/resizing artifacts |
| Image Forensics | Analyze low-level forensic evidence |
| Manipulation Detection | Detect and localize image manipulation |
| Digital Provenance / C2PA | Verify available digital credentials |

> **No single engine determines the final result.** Each engine produces evidence, which is passed to the Trust Engine.

```mermaid
flowchart LR
    A[AI Detection] --> H[Evidence Aggregation]
    B[Metadata] --> H
    C[Image Integrity] --> H
    D[Forensics] --> H
    E[Manipulation Detection] --> H
    F[Fingerprinting] --> H
    G[C2PA / Provenance] --> H
    H --> I[Trust Engine]
    I --> J[Authenticity Report]
```

---

## 3. Related Work & Project Differentiation

This section compares D²SR conceptually with existing platforms, tools, and research addressing image authenticity — not to rank or attack them, but to clarify the gap D²SR aims to fill.

### 3.1 Existing Solutions

| Category | Main Focus | Evidence Provided | Limitation (in this context) |
| --- | --- | --- | --- |
| AI-generated image detection tools | Classify an image as AI-generated or not | A single probability/verdict | Single-signal; accuracy varies across unseen generators |
| Image manipulation / forensic tools | Detect tampering (splicing, copy-move, etc.) | Forensic traces, sometimes a heatmap | Usually standalone; not combined with AI-generation or provenance checks |
| Metadata / EXIF analysis tools | Inspect embedded file metadata | Camera info, timestamps, software tags | Metadata is frequently missing or stripped, limiting coverage |
| C2PA / Content Credentials solutions | Verify cryptographically signed provenance | Presence/absence of signed credentials | Only useful when credentials were embedded at capture/export; adoption is still limited |
| Reverse image search / similarity systems | Find similar or duplicate images online | Related sources, near-duplicates | Identifies *matches*, not authenticity of the specific file |
| Academic AI-generated image detection research | Advance detection accuracy on benchmark datasets | Model performance metrics | Often evaluated in isolation, not integrated into an end-to-end user-facing report |

Each category above addresses one piece of the authenticity question. D²SR's premise is that combining them — rather than relying on any one in isolation — produces a more resilient and more explainable assessment.

### 3.2 What Makes D²SR Different

- **Multi-signal analysis:** No dependency on a single AI detector — D²SR combines AI detection, manipulation detection, metadata, image integrity, provenance/C2PA, and similarity/fingerprinting signals.
- **Evidence-based Trust Score:** Instead of a binary "Real" or "AI Generated" label, the system aggregates multiple evidence sources into an explainable Trust Score and Confidence Score.
- **Explainable authenticity report:** Users see the evidence behind the result — warnings, suspicious regions/heatmaps, metadata findings, provenance status, and other available signals.
- **Resilience to metadata loss:** The system does not depend on metadata alone, since screenshots, compression, editing, and social-media processing frequently remove or alter it.
- **Technology integration:** The project combines Angular, ASP.NET Core, Python AI services, computer vision, digital forensics, provenance standards such as C2PA, databases, Docker, and security controls into one platform.
- **Academic and non-profit orientation:** D²SR is an academic project, designed to provide its core authenticity-analysis service free of charge rather than as a profit-oriented commercial offering.
- **Unified platform:** Instead of requiring separate tools for AI detection, metadata inspection, forensic analysis, provenance, and similarity checking, D²SR brings these signals into one workflow and one report.

### 3.3 D²SR Technical Fingerprint

D²SR's identity is defined by the combination of techniques it integrates, rather than any single novel algorithm:

```text
Multi-Signal Analysis + AI Detection + Image Forensics + Provenance/C2PA
+ Metadata + Integrity + Similarity/Fingerprinting
+ Evidence-Based Trust Scoring + Explainable Reporting
```

The novelty lies in **integrating complementary techniques into one explainable authenticity assessment pipeline**, rather than inventing each individual technique from scratch.

### 3.4 Comparison

| Capability | Typical Single-Purpose Tools | D²SR |
| --- | --- | --- |
| AI-generated image detection | ✓ / Limited | ✓ |
| Manipulation detection | Limited / Tool-dependent | ✓ |
| Metadata analysis | Tool-dependent | ✓ |
| Image integrity analysis | Limited | ✓ |
| C2PA / provenance | Tool-dependent | ✓ |
| Similarity / fingerprinting | Tool-dependent | ✓ |
| Multi-signal analysis | Usually limited | ✓ |
| Evidence-based Trust Score | Usually limited | ✓ |
| Explainable authenticity report | Varies | ✓ |
| Free academic service | Varies | ✓ |
| Unified workflow | Usually fragmented | ✓ |

This table reflects design scope and differentiation, not a claim that D²SR objectively outperforms every existing solution.

D²SR's contribution is the **integration, explainability, evidence aggregation, and accessibility** of multiple authenticity signals into a single academic platform.

---

## 4. Role of AI

The AI team does not simply answer *"Real or Fake?"* — it analyzes visual content through two distinct tasks.

### AI Engineer 1 — AI-Generated Image Detection

Determines whether an image was likely generated by an AI system (e.g. Midjourney, Stable Diffusion, or other generative models).

```text
AI Generated Probability = 0.87
```

*(a model probability/estimate, not absolute truth)*

Responsibilities: dataset preparation, preprocessing, model selection, transfer learning where appropriate, training/fine-tuning, evaluation, classification, model serving, API integration.

### AI Engineer 2 — Manipulation Detection & Localization

Determines whether an originally real image has been manipulated (copy-move, splicing, removal, inpainting), and where possible, localizes the affected region.

```text
Prediction: Forged
Suspicious Region: [Heatmap / Localization Map]
```

Responsibilities: tampered-image dataset preparation, preprocessing, model development, classification, localization, heatmap generation, evaluation, model serving, API integration.

> **Key distinction:** AI Engineer 1 asks *"was this generated?"* — AI Engineer 2 asks *"was this real image altered, and where?"*

---

## 5. Other Analysis Components

**Metadata** — EXIF, camera info, timestamps, software info. Missing metadata does **not** mean an image is fake.

**Hash / Fingerprinting** — Perceptual hashing and image embeddings to identify similar or near-duplicate images and potential sources.

**Digital Signature / C2PA** — Checks for available provenance / Content Credentials. `No C2PA Found` means no provenance evidence is available — **not** that the image is fake.

**Image Integrity** — Compression artifacts, double compression, resizing, quantization traces.

**Image Forensics** — Noise patterns, sensor consistency, Error Level Analysis where appropriate.

**Manipulation Detection** — Identifies editing and, where supported, localizes suspicious regions.

---

## 6. Trust Engine

The Trust Score is **not** simply `100 - AI Probability`.

```mermaid
flowchart TD
    A[Multiple Evidence Sources] --> B[Evidence Reliability]
    B --> C[Evidence Aggregation]
    C --> D[Conflict Analysis]
    D --> E[Trust Score + Confidence Score]
```

Different signals carry different reliability:

- Verified provenance provides strong evidence.
- Missing metadata is not proof of manipulation.
- An AI detector prediction is one signal among several.
- Conflicting evidence reduces confidence.

The final report contains both a **Trust Score** and a **Confidence Score**.

> **Illustrative Example — Not Benchmark Results**

```text
Trust Score: 87%
AI Generated Probability: 8%
Manipulation Probability: 11%
Confidence: 91%
```

---

## 7. Methodology

```mermaid
flowchart LR
    A[Dataset Research] --> B[Data Preparation]
    B --> C[AI Model Development]
    C --> D[Forensic & Metadata Analysis]
    D --> E[Evidence Aggregation]
    E --> F[Trust Engine]
    F --> G[Backend Integration]
    G --> H[Frontend Integration]
    H --> I[Security & QA]
    I --> J[Evaluation]
```

| Stage | Summary |
| --- | --- |
| 1. Dataset Research | Identify and compare datasets for AI-generated, real, and manipulated images |
| 2. Data Preparation | Cleaning, label validation, deduplication, splitting, preprocessing, augmentation |
| 3. AI Model Development | AI-generation detection, manipulation detection & localization, transfer learning |
| 4. Forensic & Metadata Analysis | Metadata extraction, fingerprinting, compression analysis, forensics, C2PA |
| 5. Evidence Aggregation | Combine outputs from all analysis engines |
| 6. Trust Engine | Generate Trust Score, Confidence Score, evidence summary |
| 7. Backend Integration | Connect AI and analysis services via APIs |
| 8. Frontend Integration | Upload, progress, results, evidence visualization, heatmaps, report |
| 9. Security & QA | Functional, integration, regression, API security, upload/malicious-file, auth testing |
| 10. Evaluation | Accuracy, precision, recall, F1, ROC-AUC, localization metrics, FP/FN analysis |

---

## 8. Datasets

**Candidate datasets currently under evaluation** — no final selection has been made.

| Dataset | Link | Use Case |
| --- | --- | --- |
| CIFAKE — Real and AI-Generated Synthetic Images | [kaggle.com/datasets/birdy654/cifake-real-and-ai-generated-synthetic-images](https://www.kaggle.com/datasets/birdy654/cifake-real-and-ai-generated-synthetic-images) | AI-generated image detection |
| CASIA 2.0 Image Tampering Detection Dataset | [kaggle.com/datasets/divg07/casia-20-image-tampering-detection-dataset](https://www.kaggle.com/datasets/divg07/casia-20-image-tampering-detection-dataset) | Manipulation detection — closest current candidate |
| CommunityForensics-Small | [huggingface.co/datasets/OwensLab/CommunityForensics-Small](https://huggingface.co/datasets/OwensLab/CommunityForensics-Small) | AI-generation side — closest current candidate |

---

## 9. Technical Challenges & Mitigation

### Challenge 1 — Metadata Loss

**Problem:** Social media, screenshots, editing, or recompression can strip or alter metadata and signatures, so it may no longer represent the original file.

**Mitigation:** Never depend on metadata alone.

```text
Metadata + AI Detection + Forensics + Image Integrity
+ Manipulation Detection + Fingerprinting + C2PA (when available)
```

AI and forensic signals let the system keep analyzing an image even when metadata is unavailable.

### Challenge 2 — Very Large AI Datasets

Some candidate datasets may reach hundreds of millions of images, creating storage, processing, training-time, and GPU challenges beyond current team resources.

**Mitigation:**

- Carefully selected subsets
- Pretrained models and transfer learning
- Representative, controlled experiments
- Data augmentation
- Cloud/GPU resources only when necessary
- Benchmarking on smaller datasets before scaling

> The project does **not** need to train on 500+ million images. The goal is a realistic, evaluated prototype — not massive-scale training.

---

## 10. Other Expected Challenges

| Risk | Mitigation |
| --- | --- |
| Model Generalization — unseen generators may reduce accuracy | Diverse datasets, cross-generator evaluation, transfer learning |
| False Positives / False Negatives — no detector is perfect | Multiple signals, report confidence rather than absolute verdicts |
| Compression / Social Media Transformations | Test against simulated transformations |
| Dataset Bias — differing distributions/generation methods | Dataset diversity, cross-dataset evaluation, bias analysis |
| Computational Cost — multiple engines increase processing time | Background jobs, async processing, caching, selective execution |
| C2PA Availability — not all images have Content Credentials | Treat missing provenance as unavailable evidence, not proof of manipulation |
| Explainability — a raw score is hard to interpret | Surface the evidence behind the final assessment |

---

## 11. Technologies

| Area | Technologies |
| --- | --- |
| Frontend | Angular, TypeScript, Tailwind CSS / Angular Material, Recharts |
| Backend | ASP.NET Core, C#, JWT, Swagger / OpenAPI |
| AI / Computer Vision | Python, FastAPI, PyTorch, OpenCV, Hugging Face |
| Database | PostgreSQL and/or SQL Server |
| Background Processing | Hangfire |
| Infrastructure | Docker, Linux, Cloud Deployment |
| Development | Git, GitHub, CI/CD |

---

## 12. System Architecture

```mermaid
flowchart TD
    UI[Angular Frontend] --> API[ASP.NET Core API]
    API --> ORCH[Analysis Orchestrator]
    ORCH --> AI1[AI Detection]
    ORCH --> META[Metadata]
    ORCH --> INTEG[Image Integrity]
    ORCH --> FORE[Forensics]
    ORCH --> MANIP[Manipulation Detection]
    ORCH --> HASH[Fingerprinting]
    ORCH --> C2PA[C2PA / Provenance]
    AI1 --> AGG[Evidence Aggregation]
    META --> AGG
    INTEG --> AGG
    FORE --> AGG
    MANIP --> AGG
    HASH --> AGG
    C2PA --> AGG
    AGG --> TRUST[Trust Engine]
    TRUST --> REPORT[Authenticity Report]
```

The analysis engines run as independent Python/FastAPI services, orchestrated by the ASP.NET Core backend, and feed a shared aggregation layer before reaching the Trust Engine.

---

## 13. Team Tracks

| Track | Members | Main Responsibility |
| --- | --- | --- |
| UI/UX | 1 | User experience and interface design |
| Frontend / Angular | 2 | Web application and visualization |
| Backend / .NET | 3 | APIs, orchestration, database and services |
| AI | 2 | AI generation + manipulation detection |
| Data Analyst | 1 | Datasets, evaluation and analysis |
| Security & QA Testing | 1 | Security testing and quality assurance |
| **Total** | **10** |  |

### UI/UX — 1

User flows, wireframes, upload experience, dashboard UX, analysis-progress experience, authenticity report design, design system, usability considerations.

### Frontend / Angular — 2

Angular application, image upload, dashboard, analysis progress, Trust Score visualization, evidence visualization, heatmap visualization, authenticity report, backend API integration, loading/error states.

### Backend / .NET — 3

ASP.NET Core API, authentication/authorization, image upload handling, analysis job management, AI-service communication, database integration, evidence storage, report APIs, analysis orchestration, background processing.

### AI — 2

- **AI Engineer 1:** AI-generated image detection (see Section 4).
- **AI Engineer 2:** Manipulation detection + localization (see Section 4).

### Data Analyst — 1

Dataset research and comparison, data quality, label validation, dataset balancing, train/validation/test strategy, statistical analysis, model evaluation, cross-dataset comparison, bias analysis, experimental reporting.

### Security & QA Testing — 1

**Security:** threat modeling, secure file-upload testing, malicious file testing, API security, auth testing, OWASP Top 10, OWASP API Security, input validation, abuse-case testing. **QA:** functional, integration, regression, API, and end-to-end testing, AI pipeline testing, report verification, test-case management.

---

## 14. Team Workflow

```mermaid
flowchart TD
    UX[UI/UX] --> FE[Frontend]
    FE --> BE[Backend]
    BE --> ORCH[Analysis Orchestrator]
    ORCH --> ENGINES["AI Detection · Manipulation Detection · Metadata
Forensics · Image Integrity · Fingerprinting · C2PA/Provenance"]
    ENGINES --> AGG[Evidence Aggregation]
    AGG --> TRUST[Trust Engine]
    TRUST --> REPORT[Report]
    REPORT --> QA[Security + QA]
```

All tracks — AI, Backend, Frontend, Data, UI/UX, and Security/QA — collaborate continuously rather than working in isolation.

---

## 15. MVP Scope

**MVP**

- Still-image analysis
- AI-generated image detection
- Manipulation detection and localization (where supported)
- Metadata analysis
- Image integrity analysis
- Image forensics
- Fingerprinting
- C2PA / provenance checking
- Evidence aggregation
- Trust Score + Confidence Score
- Explainable authenticity report
- Web dashboard

**Future Expansion (out of scope)**

- Video authenticity
- Audio analysis
- Document analysis
- Advanced provenance capabilities
- Enterprise-scale forensic deployment
- Massive-scale AI training

---

## 16. Expected Output

> **Illustrative Example — Not Actual Results**

```text
D²SR AUTHENTICITY REPORT

Trust Score: 87%
Confidence: 91%

AI Generated Probability: 8%
Manipulation Probability: 11%

Metadata: Partially Consistent
Image Integrity: Double Compression Detected
Provenance: No C2PA Found

Evidence:
- ...
- ...
- ...

Forensic Heatmap: [Visualization]
```

---

## 17. Project Value

D²SR aims to provide:

- Multi-signal image authenticity assessment
- Explainable evidence instead of a black-box verdict
- AI-generated content detection
- Manipulation detection and localization
- Metadata and provenance analysis
- Forensic indicators
- A structured authenticity report

No claims of guaranteed or 100% accurate detection are made.

---

## 18. Future Roadmap

```mermaid
flowchart LR
    P1[Phase 1: Image Analysis] --> P2[Phase 2: Video Authenticity]
    P2 --> P3[Phase 3: Audio / Documents]
    P3 --> P4[Phase 4: Advanced Provenance]
    P4 --> P5[Phase 5: Enterprise / Forensics Platform]
```

Only **Phase 1** is within the current graduation-project scope.

---

## 19. Conclusion

D²SR treats image authenticity as a question of accumulated evidence rather than a single yes/no prediction. By combining AI detection, metadata analysis, forensic signals, integrity checks, fingerprinting, manipulation detection, and provenance verification into one explainable workflow, the system produces a transparent, evidence-backed authenticity report instead of an opaque verdict.
