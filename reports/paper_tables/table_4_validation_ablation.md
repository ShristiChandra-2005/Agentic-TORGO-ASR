**Stage-by-stage validation ablation: contribution of each agent in the frozen pipeline (n = 3,849). Sentence Agent v2 was evaluated but excluded from the frozen pipeline (see text).**

| System                                        | Prediction Column          |      WER |      CER |   Sentence Accuracy |   Samples |   WER Reduction vs Whisper |   CER Reduction vs Whisper |
|:----------------------------------------------|:---------------------------|---------:|---------:|--------------------:|----------:|---------------------------:|---------------------------:|
| E5 Final Verified Agentic                     | final_agentic_corrected    | 0.205862 | 0.234462 |            0.724344 |      3849 |                 0.368518   |                 0.310417   |
| E6 Character + Word + Sentence v2             | sentence_corrected         | 0.207862 | 0.237016 |            0.723824 |      3849 |                 0.362381   |                 0.302906   |
| E3 Whisper + Character + Word                 | word_corrected             | 0.209063 | 0.237998 |            0.724344 |      3849 |                 0.358699   |                 0.300017   |
| E4 Selected Character + Word Pre-Verification | pre_verification_corrected | 0.209063 | 0.237998 |            0.724344 |      3849 |                 0.358699   |                 0.300017   |
| E2 Whisper + Character                        | character_corrected        | 0.325398 | 0.339888 |            0.724344 |      3849 |                 0.00184106 |                 0.00034664 |
| E1 Whisper Raw                                | whisper_raw                | 0.325998 | 0.340006 |            0.723305 |      3849 |                 0          |                 0          |
