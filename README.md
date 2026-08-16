# Agentic-TORGO-ASR

## Agentic AI-Assisted Dysarthric Speech Recognition

An end-to-end research framework for **dysarthric speech recognition** using the **TORGO dataset**, fine-tuned **Whisper ASR**, GAN-based augmentation experiments, and a modular **Agentic AI correction pipeline**.

The project studies whether specialized post-ASR correction agents can reduce transcription errors while preserving the meaning of the original speech.

---

## 1. Project Overview

Dysarthric speech is challenging for automatic speech recognition because articulation differences, pronunciation variability, speaking rate, and other speech characteristics can cause substantial recognition errors.

This project addresses the problem in two stages:

1. **Improve/benchmark the upstream ASR system**
2. **Correct residual transcription errors using specialized agents**

### Final research pipeline

```text
TORGO Dataset
      |
      v
Data Validation & Preprocessing
      |
      v
Whisper Tiny / Whisper Small
      |
      v
GAN Experiments
(CWGAN-GP / ACGAN)
      |
      v
Best Upstream ASR Selection
      |
      v
Raw ASR Transcription
      |
      v
Character Agent v4
      |
      v
Word Agent v3
      |
      +-----------------------------+
      |                             |
      | Sentence Agent v2           |
      | tested experimentally       |
      | but NOT frozen               |
      |                             |
      +-----------------------------+
      |
      v
Final Verification Agent
      |
      v
Final Agentic Transcript
      |
      v
WER / CER / BLEU / ROUGE-L /
Sentence Accuracy / Runtime
```

> **Important:** GAN models and Sentence Agent v2 are experimental/ablation components. They are not claimed as part of the final frozen improvement pipeline when they did not improve the selected validation objective.

---

# 2. Main Contributions

### 1. Speaker-independent dysarthric ASR evaluation

A structured pipeline was developed for training, validation, model selection, and final held-out test evaluation.

### 2. Whisper model comparison

The project evaluates:

- Whisper Tiny
- Whisper Small

Whisper Small was selected as the stronger upstream ASR baseline.

### 3. GAN augmentation study

Two GAN approaches were investigated:

- CWGAN-GP
- ACGAN

The important research question was whether GAN-based augmentation produces a downstream ASR improvement.

The experiments did **not** provide sufficient improvement over the baseline, so GAN enhancement was rejected from the final selected improvement path.

### 4. Multi-stage Agentic AI correction

The main contribution is a modular correction pipeline:

```text
Raw Whisper Transcript
        |
        v
Character Agent v4
        |
        v
Word Agent v3
        |
        v
Final Verification Agent
        |
        v
Final Transcript
```

### 5. Controlled ablation study

Each major component was evaluated separately before deciding whether to retain it.

### 6. Frozen held-out test evaluation

The final test set contains **3,030 samples** and is reserved for final evaluation after development/validation decisions.

---

# 3. System Architecture

```text
                       +----------------------+
                       |     TORGO DATASET    |
                       | Dysarthric Speech    |
                       | Ground-Truth Text    |
                       +----------+-----------+
                                  |
                                  v
                       +----------------------+
                       | PREPROCESSING & EDA  |
                       | - Validation         |
                       | - Cleaning           |
                       | - Normalization      |
                       | - Speaker analysis   |
                       +----------+-----------+
                                  |
                                  v
                 +------------------------------------+
                 |             WHISPER ASR             |
                 |                                    |
                 |        Tiny       |      Small     |
                 +------------------+-----------------+
                                  |
                                  v
                 +------------------------------------+
                 |          GAN EXPERIMENTS            |
                 |                                    |
                 |       CWGAN-GP     |     ACGAN      |
                 +-------------------+----------------+
                                  |
                                  v
                       +----------------------+
                       | BEST UPSTREAM ASR    |
                       +----------+-----------+
                                  |
                                  v
                       +----------------------+
                       | RAW TRANSCRIPTION    |
                       +----------+-----------+
                                  |
                                  v
                       +----------------------+
                       | CHARACTER AGENT v4   |
                       | Fine-grained errors  |
                       +----------+-----------+
                                  |
                                  v
                       +----------------------+
                       | WORD AGENT v3        |
                       | Context/repetition   |
                       +----------+-----------+
                                  |
                                  v
                       +----------------------+
                       | VERIFICATION AGENT   |
                       | Conservative check   |
                       +----------+-----------+
                                  |
                                  v
                       +----------------------+
                       | FINAL TRANSCRIPT     |
                       +----------+-----------+
                                  |
                                  v
              +-----------------------------------------+
              |              EVALUATION                 |
              | WER | CER | BLEU | ROUGE-L | Sent. Acc |
              +-----------------------------------------+
```

---

# 4. Experimental Workflow

## Phase 1-3 — Dataset Preparation

The initial stages establish the experimental dataset and evaluation protocol.

Main operations include:

- Data validation
- Audio/transcript checking
- Text normalization
- Dataset statistics
- Speaker analysis
- Train/validation/test preparation
- Leakage checks
- Reproducible configuration

The evaluation is designed around speaker-independent generalization.

---

## Phase 4 — Baseline Whisper

Two Whisper configurations are evaluated:

```text
Whisper Tiny
      |
      v
Evaluation

Whisper Small
      |
      v
Evaluation

      |
      v
Automatic Comparison
      |
      v
Best Whisper
```

Whisper Small is used as the main upstream ASR baseline.

---

# 5. GAN Experiments

The project investigates whether GAN-generated/augmented speech can improve downstream Whisper ASR.

```text
                         +--> CWGAN-GP --+
                         |              |
TORGO Speech ------------+              +--> Whisper --> Evaluation
                         |              |
                         +--> ACGAN ----+
```

### Evaluation criteria

- WER
- CER
- Sentence Accuracy
- Runtime

The GAN selection rule is based on validation performance rather than test-set performance.

### Result

Neither CWGAN-GP nor ACGAN provided sufficient downstream ASR improvement over the baseline.

Therefore:

```text
CWGAN-GP  -> Rejected
ACGAN      -> Rejected
Baseline   -> Retained
```

This negative result is intentionally retained as part of the ablation study.

---

# 6. Agentic AI Correction Layer

The Agentic AI layer is the central post-ASR component.

Rather than allowing one model to rewrite the complete transcript, the system decomposes correction into specialized stages.

```text
                 RAW ASR
                    |
                    v
          +-------------------+
          | Character Agent v4|
          +---------+---------+
                    |
                    v
          +-------------------+
          |   Word Agent v3   |
          +---------+---------+
                    |
                    v
          +-------------------+
          | Verification Agent|
          +---------+---------+
                    |
                    v
              FINAL TEXT
```

---

# 7. Character Agent v4

## Objective

Character Agent v4 handles fine-grained word-level transcription errors.

It is designed to be conservative and operates using train-derived vocabulary and similarity/context checks.

### Main responsibilities

- Detect suspicious tokens
- Find near-match vocabulary candidates
- Character similarity matching
- Confidence filtering
- Margin filtering
- Avoid unnecessary corrections
- Protect valid tokens

Conceptually:

```text
ASR Word
   |
   v
Is the token suspicious?
   |
   v
Generate vocabulary candidates
   |
   v
Character similarity
   |
   v
Context / confidence checks
   |
   +---- Reject ----> Keep original
   |
   +---- Accept ----> Corrected word
```

### Example

```text
ASR:
    yestarday

Character Agent:
    yesterday
```

---

# 8. Word Agent v3

Word Agent v3 operates on the output of Character Agent v4.

### Responsibilities

- Word-level correction
- Context-aware correction
- Confusion correction
- Repetition handling
- Candidate ranking
- Conservative substitution

Pipeline:

```text
Raw Whisper
     |
     v
Character Agent v4
     |
     v
Word Agent v3
     |
     v
Improved Transcript
```

The agent uses training-derived vocabulary/context information and does not use test ground truth to make correction decisions.

---

# 9. Sentence Agent v2

A sentence-level correction agent was also experimentally investigated.

### Intended functions

- Grammar correction
- Context correction
- Sentence refinement
- Fluency improvement

However, Sentence Agent v2 was **not frozen into the final pipeline**.

The validation experiments showed that although it produced a small WER/CER change, sentence-level accuracy slightly decreased.

Therefore:

```text
Sentence Agent v2
        |
        v
     Tested
        |
        v
Validation analysis
        |
        v
NOT FROZEN
```

This is reported as an ablation/negative result rather than hidden.

---

# 10. Final Verification Agent

The verification stage is designed to make the final correction pipeline conservative.

It checks the corrected transcript and prevents unnecessary modifications.

```text
Character Agent v4
        |
        v
Word Agent v3
        |
        v
Final Verification
        |
        v
Final Agentic Transcript
```

The objective is not to rewrite the sentence freely, but to retain useful corrections while reducing undesirable changes.

---

# 11. Validation Results

The validation/development experiments were used for model and agent selection.

| Pipeline | WER ↓ | CER ↓ | Sentence Accuracy ↑ |
|---|---:|---:|---:|
| Best Whisper Small (baseline) | 0.3017 | — | — |
| + Character Agent v4 | 0.3254 | 0.3399 | 0.7243 |
| **+ Word Agent v3** | **0.2091** | **0.2380** | **0.7243** |
| + Sentence Agent v2 (T5) | 0.2079 | 0.2370 | 0.7238 |
| **+ Final Verification Agent** | **0.2059** | **0.2345** | **0.7243** |

### Validation interpretation

- Character Agent v4 provided fine-grained correction.
- Word Agent v3 produced the major WER improvement.
- Sentence Agent v2 was tested but was not frozen because sentence accuracy decreased.
- Final Verification was retained in the final selected pipeline.

---

# 12. Final Held-Out Test Evaluation

The final test set contains:

```text
3030 samples
```

The final selected pipeline was evaluated after the development decisions were completed.

### Final test pipeline

```text
Whisper Small / selected upstream Whisper
              |
              v
       Raw Transcript
              |
              v
     Character Agent v4
              |
              v
        Word Agent v3
              |
              v
    Final Verification Agent
              |
              v
     Final Agentic Transcript
```

---

# 13. Final Test Results

| System | WER ↓ | CER ↓ | BLEU ↑ | ROUGE-L ↑ | Sentence Accuracy ↑ |
|---|---:|---:|---:|---:|---:|
| Whisper Small | **0.4978** | **0.5980** | 51.69 | 0.7041 | 0.6353 |
| **Whisper + Agentic AI** | **0.2975** | **0.4239** | **69.51** | **0.7060** | **0.6366** |

### Improvement over Whisper Small

| Metric | Whisper Small | Agentic AI | Change |
|---|---:|---:|---:|
| WER ↓ | 0.4978 | **0.2975** | **~40.2% relative reduction** |
| CER ↓ | 0.5980 | **0.4239** | **~29.1% relative reduction** |
| BLEU ↑ | 51.69 | **69.51** | Improved |
| ROUGE-L ↑ | 0.7041 | **0.7060** | Improved |
| Sentence Accuracy ↑ | 0.6353 | **0.6366** | Improved |

### Key result

The final Agentic AI correction layer substantially reduced word and character error rates compared with the Whisper Small baseline on the held-out test set.

---

# 14. Final Results at a Glance

```text
                         WHISPER       AGENTIC ASR

WER                      0.4978        0.2975
                         ██████████    ██████

CER                      0.5980        0.4239
                         ████████████  ████████

BLEU                     51.69         69.51
                         ██████████    ██████████████

ROUGE-L                  0.7041        0.7060

Sentence Accuracy        0.6353        0.6366
```

---

# 15. Final Ablation Summary

| Component | Decision | Main Observation |
|---|---|---|
| Whisper Tiny | Baseline | Lightweight comparison |
| Whisper Small | **Selected** | Stronger upstream ASR |
| CWGAN-GP | Rejected | No sufficient downstream improvement |
| ACGAN | Rejected | No sufficient downstream improvement |
| Character Agent v4 | **Selected** | Fine-grained correction |
| Word Agent v3 | **Selected** | Major WER improvement |
| Sentence Agent v2 | Rejected from frozen pipeline | Slight sentence-accuracy decrease |
| Final Verification Agent | **Selected** | Conservative final checking |

---

# 16. Error Analysis

The project supports sample-level error analysis using:

```text
Ground Truth
      |
      v
Raw Whisper Output
      |
      v
Character Corrected Output
      |
      v
Word Corrected Output
      |
      v
Final Agentic Output
```

This enables investigation of:

- Substitution errors
- Deletion errors
- Insertion errors
- Spelling-like errors
- Word confusions
- Repetitions
- Successful corrections
- Incorrect corrections
- Remaining failure cases

The final evaluation also records whether an individual sample was improved or worsened by the Agentic AI layer.

---

# 17. Evaluation Metrics

## Word Error Rate — WER

Measures word-level transcription errors.

```text
WER = (Substitutions + Deletions + Insertions)
      / Number of Reference Words
```

Lower is better.

## Character Error Rate — CER

Measures character-level transcription errors.

Lower is better.

## BLEU

Measures n-gram overlap between predicted and reference text.

Higher is better.

## ROUGE-L

Measures sequence similarity using the longest common subsequence.

Higher is better.

## Sentence Accuracy

Measures exact sentence-level correctness after evaluation normalization.

Higher is better.

## Runtime

Measures inference/evaluation time and efficiency.

---

# 18. Experimental Discipline

The project follows a controlled experimental workflow:

```text
Training Data
      |
      v
Validation / Development
      |
      v
Model & Agent Selection
      |
      v
Freeze Configuration
      |
      v
Held-Out Test Set
      |
      v
Final Evaluation
```

The test set is not used for tuning the Character Agent, Word Agent, or Sentence Agent decisions.

This separation is important for obtaining an unbiased final evaluation.

---

# 19. Project Structure

```text
Agentic-TORGO-ASR/
│
├── dataset/
│   ├── train.csv
│   ├── validation.csv
│   └── test.csv
│
├── models/
│   ├── whisper_tiny_finetuned/
│   ├── whisper_small_finetuned/
│   └── ...
│
├── predictions/
│   ├── phase_8_*.csv
│   ├── phase_9_*.csv
│   └── phase_10_final_test_all_systems_predictions.csv
│
├── reports/
│   ├── phase_8_*.csv
│   ├── phase_9_*.csv
│   ├── phase_10_final_test_comparison.csv
│   ├── error_analysis.csv
│   └── *.json
│
├── checkpoints/
│   ├── phase_8_*.json
│   └── phase_10_*.json
│
├── figures/
│   ├── wer/
│   ├── cer/
│   ├── ablation/
│   └── error_analysis/
│
├── scripts/
│   ├── preprocessing/
│   ├── whisper/
│   ├── gan/
│   ├── agents/
│   └── evaluation/
│
├── configs/
│
├── requirements.txt
├── environment.yml
└── README.md
```

---

# 20. Installation

## Conda environment

```bash
conda env create -f environment.yml
conda activate torgo_agentic_asr
```

## Or install Python dependencies

```bash
python -m pip install -r requirements.txt
```

For final text-quality evaluation:

```bash
python -m pip install sacrebleu rouge-score
```

---

# 21. Running the Project

Run the project phase-by-phase.

```text
Phase 1
   ↓
Dataset preparation
   ↓
Phase 2-3
   ↓
Preprocessing / validation
   ↓
Phase 4
   ↓
Whisper baseline
   ↓
Phase 5-6
   ↓
GAN experiments / Whisper selection
   ↓
Phase 7-9
   ↓
Agentic correction experiments
   ↓
Phase 10
   ↓
Final held-out test evaluation
   ↓
Error analysis & reports
```

The exact scripts/configuration files should be kept with the repository so that each experiment can be reproduced from its saved checkpoint and configuration.

---

# 22. Output Artifacts

Important outputs include:

### Predictions

```text
predictions/
```

Contains raw and corrected ASR predictions.

### Reports

```text
reports/
```

Contains:

- Model comparisons
- Agent comparisons
- Error analysis
- Final test metrics
- Configuration files
- JSON summaries

### Checkpoints

```text
checkpoints/
```

Contains frozen experiment configurations and evaluation checkpoints.

### Figures

```text
figures/
```

Contains:

- WER comparison
- CER comparison
- Ablation plots
- Error-analysis plots

---

# 23. Research Questions

### RQ1
Which Whisper model performs better for dysarthric speech?

**Whisper Tiny vs Whisper Small**

### RQ2
Does GAN-based augmentation improve downstream ASR?

**CWGAN-GP / ACGAN vs Whisper baseline**

### RQ3
Can character-level correction improve transcription?

**Whisper → Character Agent**

### RQ4
Can contextual word correction provide additional improvement?

**Character Agent → Word Agent**

### RQ5
Does sentence-level correction improve the final transcript?

**Sentence Agent v2 experimental evaluation**

### RQ6
Does the final modular Agentic AI pipeline improve held-out test performance?

**Whisper → Character → Word → Verification**

---

# 24. Key Research Findings

### Finding 1 — Whisper Small is the stronger upstream model

Whisper Small was selected over the lightweight Tiny baseline.

### Finding 2 — GAN augmentation did not improve downstream ASR sufficiently

CWGAN-GP and ACGAN were evaluated, but neither was retained as a beneficial final component.

### Finding 3 — Character correction alone is insufficient

Character Agent v4 provides targeted correction but is not responsible for the largest overall WER reduction.

### Finding 4 — Word Agent v3 provides the major validation improvement

The addition of Word Agent v3 substantially reduced validation WER and CER.

### Finding 5 — Sentence Agent v2 was not retained

Sentence-level rewriting caused a small decrease in sentence accuracy, so it was rejected from the frozen pipeline.

### Finding 6 — Agentic correction improves final held-out performance

The final Agentic ASR system reduced WER from **0.4978 to 0.2975** and CER from **0.5980 to 0.4239** on the held-out test set.

---

# 25. Limitations

The current study has several limitations:

- The evaluation is based on the TORGO dataset.
- Dysarthric speech characteristics vary considerably across speakers.
- The final test CER is higher than the corresponding validation CER, indicating a generalization gap that requires further analysis.
- GAN enhancement did not improve the downstream ASR objective in the current experiments.
- Sentence-level correction was not retained in the final frozen pipeline.
- Human evaluation of transcript usefulness is a potential future extension.
- Larger multi-dataset evaluation would strengthen generalization claims.

---

# 26. Future Work

Potential extensions include:

- Larger dysarthric speech datasets
- Cross-dataset evaluation
- More unseen speakers
- Severity-aware ASR
- Phoneme-aware correction
- Pronunciation-aware modeling
- Confidence calibration
- Better contextual correction
- Human evaluation
- Statistical significance testing
- Real-time inference
- Interactive assistive communication interface
- TTS-based communication support
- More advanced multi-agent coordination

---

# 27. Final Research Story

```text
Dysarthric Speech
       |
       v
Data Validation
       |
       v
Whisper ASR
       |
       v
Raw Transcription
       |
       v
Character-Level Correction
       |
       v
Word-Level Contextual Correction
       |
       v
Final Verification
       |
       v
Final Agentic Transcript
       |
       v
Quantitative Evaluation
       |
       v
Improved Dysarthric Speech Transcription
```

The project therefore treats dysarthric ASR as a complete pipeline rather than only a model-selection problem:

```text
Speech Recognition
        +
Error Detection
        +
Specialized Correction
        +
Verification
        =
Agentic Dysarthric ASR
```

---

# 28. Final Result

### Best reported held-out test result

| Metric | Whisper Small | Final Agentic ASR |
|---|---:|---:|
| **WER ↓** | 0.4978 | **0.2975** |
| **CER ↓** | 0.5980 | **0.4239** |
| **BLEU ↑** | 51.69 | **69.51** |
| **ROUGE-L ↑** | 0.7041 | **0.7060** |
| **Sentence Accuracy ↑** | 0.6353 | **0.6366** |

### Summary

**WER:** ~40.2% relative reduction  
**CER:** ~29.1% relative reduction  
**BLEU:** improved from 51.69 → 69.51  
**ROUGE-L:** improved from 0.7041 → 0.7060  
**Sentence Accuracy:** improved from 0.6353 → 0.6366

---

# 29. Citation / Dataset

This project uses the **TORGO dysarthric speech dataset** for research and evaluation.

Please follow the dataset's original licensing and citation requirements when redistributing or publishing results.

---

# 30. Author

**Shristi Chandra**

B.Tech — Computer Science & Engineering (Artificial Intelligence)

Indira Gandhi Delhi Technical University for Women (IGDTUW)

---

## One-Line Project Description

> **An Agentic AI framework for dysarthric speech recognition that combines Whisper ASR with specialized character-level, word-level, and verification agents, while systematically evaluating GAN augmentation and sentence-level correction through controlled ablation experiments.**
