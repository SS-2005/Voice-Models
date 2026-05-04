
# Benchmark Conclusion and Result

## Dataset Used
This benchmark was run on a small internal speaker-verification sample set designed to evaluate
voice-matching quality for speaker-to-name mapping.

- Reported distinct speaker identities used for this write-up: **6**
- Real human voices: **5**
- Synthetic AI voice(s): **1**
- Recordings captured in more realistic / noisy conditions: **2**
- Approximate clip duration per source file: **20–40 seconds**

In this benchmark, the available data included both:
- speakers with **multiple source recordings**, where the first file was used strictly for enrollment and the remaining file(s) were used only for testing
- speakers with **a single longer source recording**, where the first **20 seconds** were used strictly for enrollment and the remaining portion was used only for testing

### Split Notes
- Multi-file speakers: **Girl1, SID, AI**
- Single-file auto-split speakers: **g2, ethan, p1**

## Model Comparison
- **ecapa**: accuracy = **1.0000**, avg top-1 score = **0.8802**, avg top-1/top-2 gap = **0.6959**, test clips = **8**
- **wavlm_plus_sv**: accuracy = **1.0000**, avg top-1 score = **0.9800**, avg top-1/top-2 gap = **0.1754**, test clips = **8**

## Key Result
The best-performing model in this benchmark was **ecapa**.

- Best-model accuracy: **1.0000**
- Average top-1 similarity score: **0.8802**
- Average top-1 vs top-2 separation gap: **0.6959**
- Total evaluated test clips: **8**
- Auto-assigned clips at the current confidence gate: **6**
- Review-needed clips at the current confidence gate: **2**

## Insights
1. **The benchmark indicates that voice matching is feasible on the current sample set.**  
   The best model achieved strong performance and showed usable separation between the top-1 and
   top-2 candidates, which is important for confidence-gated speaker-name assignment.

2. **The confidence-gap signal is as important as raw accuracy.**  
   Even when a model predicts the correct speaker, production use should prefer cases where the
   top match is clearly separated from the second-best match. This reduces the chance of assigning
   the wrong real name in downstream transcripts.

3. **This is a strong first benchmark, but not yet final production validation.**  
   For speakers that had only one source file, enrollment and testing were created from different
   parts of the same original recording. That is still a valid first-pass benchmark, but it is
   easier than fully independent train/test data captured on different days, devices, or noise conditions.

4. **Real-world variability is still the main challenge.**  
   The inclusion of noisy / real-environment samples is useful because it makes the test more relevant
   to actual meeting conditions. However, the benchmark should be expanded further with more speakers,
   more sessions per speaker, and more varied acoustic conditions before locking a final production model.

## Conclusion
Based on this benchmark, **ecapa** is the strongest candidate among the models evaluated in this run
and is the most suitable starting point for the next phase of implementation.

The result suggests that:
- the overall voice-matching direction is valid
- the current pipeline can distinguish speaker identity with promising precision on a small sample set
- a confidence-gated matching strategy remains necessary before assigning real names automatically

## Recommended Next Steps
- Expand the benchmark with **independent enrollment and test recordings** for each speaker
- Add more **real meeting audio conditions** such as background noise, different microphones, and short utterances
- Evaluate the same model on **more speakers** and **more sessions per speaker**
- Use the best-performing model as the baseline for integrating the **Voice Identity Table + candidate-filtered matching flow**
