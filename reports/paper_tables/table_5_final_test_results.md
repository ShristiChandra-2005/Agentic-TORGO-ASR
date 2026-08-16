**Final held-out test-set comparison (n = 3,030), evaluated exactly once.**

| Pipeline                                             |      WER |      CER |    BLEU |   ROUGE-L |   Sentence Accuracy |   Runtime (s) |   Seconds / Sample |   Samples |
|:-----------------------------------------------------|---------:|---------:|--------:|----------:|--------------------:|--------------:|-------------------:|----------:|
| Best Whisper (no GAN improved baseline) + Agentic AI | 0.297481 | 0.42393  | 69.5073 |  0.706027 |            0.636634 |       984.491 |           0.324915 |      3030 |
| Whisper Small                                        | 0.497846 | 0.598041 | 51.6885 |  0.704128 |            0.635314 |       951.358 |           0.31398  |      3030 |
| Best Whisper (no GAN improved baseline)              | 0.497846 | 0.598041 | 51.6885 |  0.704128 |            0.635314 |       928.061 |           0.306291 |      3030 |
| Whisper Tiny                                         | 3.50946  | 3.80436  | 10.0845 |  0.522431 |            0.419472 |       499.663 |           0.164905 |      3030 |
