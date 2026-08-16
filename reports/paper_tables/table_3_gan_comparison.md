**GAN speech-enhancement comparison: quality-only proxy metrics (Phase 7) vs the downstream Whisper WER/CER selection actually used (Phase 8).**

| GAN      |   Generated Samples |   Accepted Samples |   Acceptance Rate |   Mean SNR |   Mean Energy |   Mean Max Abs |   Quality Score | Selected as Best (downstream WER)   |
|:---------|--------------------:|-------------------:|------------------:|-----------:|--------------:|---------------:|----------------:|:------------------------------------|
| CWGAN-GP |                9950 |               9226 |          0.927236 |    9.50626 |     0.0110926 |       0.999995 |        0.849965 | False                               |
| ACGAN    |                9950 |               9884 |          0.993367 |   12.3028  |     0.0118224 |       0.838812 |        0.996683 | False                               |
