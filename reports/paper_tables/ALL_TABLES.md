# Paper Tables -- Auto-Generated From Frozen Pipeline Outputs

Cross-check every number against the source CSV before submission. Tables are pulled directly from your Phase 1-10 report files; nothing here is recomputed or re-tuned.

## Table 1: Dataset Splits

| Split      |   Samples |
|:-----------|----------:|
| Train      |      9950 |
| Validation |      3849 |
| Test       |      3030 |

## Table 2: Whisper Baseline Comparison

| Model                      |   Validation WER |   Validation CER |
|:---------------------------|-----------------:|-----------------:|
| Whisper Tiny (fine-tuned)  |         2.61338  |         3.52035  |
| Whisper Small (fine-tuned) |         0.301691 |         0.280231 |

## Table 3: GAN Comparison

| GAN      |   Generated Samples |   Accepted Samples |   Acceptance Rate |   Mean SNR |   Mean Energy |   Mean Max Abs |   Quality Score | Selected as Best (downstream WER)   |
|:---------|--------------------:|-------------------:|------------------:|-----------:|--------------:|---------------:|----------------:|:------------------------------------|
| CWGAN-GP |                9950 |               9226 |          0.927236 |    9.50626 |     0.0110926 |       0.999995 |        0.849965 | False                               |
| ACGAN    |                9950 |               9884 |          0.993367 |   12.3028  |     0.0118224 |       0.838812 |        0.996683 | False                               |

## Table 4: Validation Ablation

| System                                        | Prediction Column          |      WER |      CER |   Sentence Accuracy |   Samples |   WER Reduction vs Whisper |   CER Reduction vs Whisper |
|:----------------------------------------------|:---------------------------|---------:|---------:|--------------------:|----------:|---------------------------:|---------------------------:|
| E5 Final Verified Agentic                     | final_agentic_corrected    | 0.205862 | 0.234462 |            0.724344 |      3849 |                 0.368518   |                 0.310417   |
| E6 Character + Word + Sentence v2             | sentence_corrected         | 0.207862 | 0.237016 |            0.723824 |      3849 |                 0.362381   |                 0.302906   |
| E3 Whisper + Character + Word                 | word_corrected             | 0.209063 | 0.237998 |            0.724344 |      3849 |                 0.358699   |                 0.300017   |
| E4 Selected Character + Word Pre-Verification | pre_verification_corrected | 0.209063 | 0.237998 |            0.724344 |      3849 |                 0.358699   |                 0.300017   |
| E2 Whisper + Character                        | character_corrected        | 0.325398 | 0.339888 |            0.724344 |      3849 |                 0.00184106 |                 0.00034664 |
| E1 Whisper Raw                                | whisper_raw                | 0.325998 | 0.340006 |            0.723305 |      3849 |                 0          |                 0          |

## Table 5: Final Test Results (Headline)

| Pipeline                                             |      WER |      CER |    BLEU |   ROUGE-L |   Sentence Accuracy |   Runtime (s) |   Seconds / Sample |   Samples |
|:-----------------------------------------------------|---------:|---------:|--------:|----------:|--------------------:|--------------:|-------------------:|----------:|
| Best Whisper (no GAN improved baseline) + Agentic AI | 0.297481 | 0.42393  | 69.5073 |  0.706027 |            0.636634 |       984.491 |           0.324915 |      3030 |
| Whisper Small                                        | 0.497846 | 0.598041 | 51.6885 |  0.704128 |            0.635314 |       951.358 |           0.31398  |      3030 |
| Best Whisper (no GAN improved baseline)              | 0.497846 | 0.598041 | 51.6885 |  0.704128 |            0.635314 |       928.061 |           0.306291 |      3030 |
| Whisper Tiny                                         | 3.50946  | 3.80436  | 10.0845 |  0.522431 |            0.419472 |       499.663 |           0.164905 |      3030 |

## Table 6: Qualitative Examples (sample)

| sample_id    | ground_truth   | whisper_raw                                                                                                                                                                                                                                                                                                                                                                                 | final_agentic_corrected   |   wer_raw |   wer_final |
|:-------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------|----------:|------------:|
| torgo_009599 | left           | yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes                                                                                                                             | yes yes                   |        64 |           2 |
| torgo_007982 | relax          | relife all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all all                                                                                                                              | relife all all            |        63 |           3 |
| torgo_007915 | lip            | slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip slip                                                                  | slip slip                 |        63 |           2 |
| torgo_007969 | pit            | pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit pit                                                                                                                             | pit pit                   |        63 |           1 |
| torgo_007378 | air            | another story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story story | another story story       |        63 |           3 |
| torgo_009651 | pat            | pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat pat                                                                                                                             | pat pat                   |        63 |           1 |
| torgo_009758 | sip            | sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip sip                                                                                                                                 | sip sip                   |        62 |           1 |
| torgo_009739 | tear           | tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear tear                                                                  | tear tear                 |        62 |           1 |

