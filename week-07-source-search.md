# Week 07 — Source Search
**Research question:** How do the architectural and computational constraints of AI music transcription models affect their ability to accurately represent the full range of musical information — including pitch, rhythm, dynamics, and instrumentation — and what are the implications for making these tools accessible and useful in real musical contexts?

---

## Included Sources

---

### Source 1
**Title:** An AI Approach to Automatic Natural Music Transcription
**Authors:** Michael Bereket, Karey Shi
**Year:** 2017
**Publication:** Stanford CS229 Final Project Report

**Extract (from paper):**
> "Automatic music transcription (AMT) remains a fundamental and difficult problem in music information research, and current music transcription systems are still unable to match human performance. AMT aims to automatically generate a score representation given a polyphonic acoustical signal... Music is often performed with an element of emotional expressiveness; as a result, the observed rhythms and tempos in the audio recordings of piano performances are often irregular. However, the outputs of our acoustic model provide us with exact representations of our original audio, corresponding precisely to the manner in which the piano piece was performed. If a score were to be exactly generated from this output without further transformation, unnatural and inconsistent patterns may arise, and it would not likely resemble a human expert's transcription due to these irregularities."

**URL:** https://cs229.stanford.edu/proj2017/final-reports/5244388.pdf

**Why it's on the shortlist:** This paper is on the shortlist because it describes exactly the same two-stage problem my Space runs into — pitch detection is one task, and converting that raw pitch data into readable notation is a completely separate (and harder) task.

**Claim I might cite:** The distinction between acoustic modeling (detecting pitches from audio) and score generation (converting raw pitch data into natural-looking notation) — this directly explains why my pipeline fails even when Basic Pitch detects pitches reasonably well.

---

### Source 2
**Title:** MT3: Multi-Task Multitrack Music Transcription
**Authors:** Josh Gardner, Ian Simon, Ethan Manilow, Curtis Hawthorne, Jesse Engel (Google Research, Brain Team)
**Year:** 2022
**Publication:** ICLR 2022 (Conference Paper)

**Extract (from paper):**
> "Automatic Music Transcription (AMT), inferring musical notes from raw audio, is a challenging task at the core of music understanding. Unlike Automatic Speech Recognition (ASR), which typically focuses on the words of a single speaker, AMT often requires transcribing multiple instruments simultaneously, all while preserving fine-scale pitch and timing information. Further, many AMT datasets are 'low-resource', as even expert musicians find music transcription difficult and time-consuming. Thus, prior work has focused on task-specific architectures, tailored to the individual instruments of each task... Compounding this challenge, many music datasets are relatively small in comparison to the datasets used to train large-scale sequence models in other domains such as NLP or ASR. Existing open-source music transcription datasets contain between one and a few hundred hours of audio, while standard ASR datasets LibriSpeech and CommonVoice contain 1k and 9k+ hours of audio, respectively. LibriSpeech alone contains more hours of audio than all of the AMT datasets we use in this paper, combined."

**URL:** https://arxiv.org/pdf/2111.03017
**DOI:** arXiv:2111.03017

**Why it's on the shortlist:** This paper represents the state of the art that my CPU-bound Space cannot use — it's the clearest evidence that the models capable of solving the transcription problem properly require transformer architectures and GPU resources that a free-tier Space cannot run.

**Claim I might cite:** AMT is fundamentally a low-resource problem — music datasets are tiny compared to speech datasets, which is one structural reason why transcription models lag behind speech recognition models; and the most capable models (like MT3 itself) are too large to run on CPU, which directly connects to my research question about accessibility vs. accuracy.

---

### Source 3
**Title:** Automatic Music Transcription: Challenges and Future Directions
**Authors:** Emmanouil Benetos, Simon Dixon, Dimitrios Giannoulis, Holger Kirchhoff, Anssi Klapuri
**Year:** 2013
**Publication:** Journal of Intelligent Information Systems, vol. 41, no. 3, pp. 407–434
**DOI:** 10.1007/s10844-013-0258-3

**Extract (from paper):**
> "Automatic music transcription is considered by many to be a key enabling technology in music signal processing. However, the performance of transcription systems is still significantly below that of a human expert, and accuracies reported in recent years seem to have reached a limit, although the field is still very active. In this paper we analyse limitations of current methods and identify promising directions for future research. Current transcription methods use general purpose models which are unable to capture the rich diversity found in music signals... The AMT problem can be divided into several subtasks, which include: multi-pitch detection, note onset/offset detection, loudness estimation and quantisation, instrument recognition, extraction of rhythmic information, and time quantisation. The core problem in automatic transcription is the estimation of concurrent pitches in a time frame, also called multiple-F0 or multi-pitch detection."

**URL:** https://openaccess.city.ac.uk/id/eprint/2524/1/JIIS-MIRrors-AMT-postprint.pdf?utm_source=consensus

**Why it's on the shortlist:** This is a foundational paper that names all the subtasks of AMT — pitch, onset/offset, loudness, rhythm, time quantization — which maps almost exactly onto the four dimensions (pitch, timing, dynamics, rhythm) I want to test in my research.

**Claim I might cite:** The decomposition of AMT into distinct subtasks is useful for framing why my pipeline fails in a cascade — each subtask (pitch, rhythm, time signature) is a separate problem, and a lightweight model that only solves one of them well will still produce unusable output overall.

---

### Source 4
**Title:** Automatic Music Transcription: An Overview
**Authors:** Emmanouil Benetos, Simon Dixon, Zhiyao Duan, Sebastian Ewert
**Year:** 2019
**Publication:** IEEE Signal Processing Magazine, vol. 36, no. 1, pp. 20–30

**Extract (from search results / Semantic Scholar):**
> "The capability of transcribing music audio into musicnotation is a fascinating example of human intelligence. It involves perception (analyzing complex auditory scenes), cognition (recognizing musical objects), knowledge representation (forming musical structures) and inference (testing alternative hypotheses). Automatic Music Transcription (AMT), i.e., the design of computational algorithms to convert acoustic music signals into some form of music notation, is a challenging task in signal processing and artificial intelligence. It comprises several subtasks, including (multi-)pitch estimation, onset and offset detection, instrument recognition, beat and rhythm tracking, interpretation of expressive timing and dynamics, and score typesetting. Given the number of subtasks it comprises and its wide application range, it is considered a fundamental problem in the fields of music signal processing and music information retrieval (MIR)"

**URL:** https://labsites.rochester.edu/air/publications/benetatos19automaticmusic.pdf

**Why it's on the shortlist:** This 2019 overview paper explicitly states that MIDI is not the same as notation — that beat, bar, meter, key, and harmony are absent from MIDI — which is exactly what my pipeline gets wrong when it tries to go from MIDI to sheet music without beat tracking or meter detection.

**Claim I might cite:** MIDI output is not the same as music notation, and the gap between them — beat, meter, key, harmony — is where my pipeline fails; this gives me a credible academic framing for why "audio to MIDI" and "audio to readable sheet music" are fundamentally different problems.

---

## Near-Misses (Inclusion/Exclusion Record)

These papers looked relevant from the title or abstract but turned out to be adjacent to my question rather than directly addressing it.

- **"Machine Learning Techniques in Automatic Music Transcription: A Systematic Survey" (arXiv 2406.15249, 2024)** — Comprehensive survey of AMT methods. Useful background but too broad to cite for a specific claim; better as a reference for the field overview than for my specific pipeline argument.

- **"YourMT3+: Multi-instrument Music Transcription with Enhanced Transformer Architectures and Cross-dataset Stem Augmentation" (arXiv 2407.04822, 2024)** — Strong follow-up to MT3, relevant to what a better model would look like, but too advanced for my current Space; good candidate for a later paper when I compare models.

- **"Source Separation & Automatic Transcription for Music" (arXiv 2412.06703, 2024)** — Combines source separation with transcription; interesting pipeline approach but more complex than my current scope and more focused on multi-instrument separation than on the CPU/GPU accessibility question.

- **Songscription review, MusicRadar (2025)** — Useful for real-world framing and the "city/map" metaphor, but not a peer-reviewed source; better as supporting illustration than as a citable reference.

---

## Reflection

My research question turned out to be reasonably well-supported by the literature — there are real papers that name the exact subtasks I am investigating (Benetos et al. 2013, 2019), and there is a clear state-of-the-art model (MT3) that represents the gap between what I built and what is possible with more compute. What surprised me is how old some of the foundational framing is: the 2013 Benetos paper already identified rhythm, meter, and dynamics as separate hard problems, which means the failure modes I observed in my Space are not new discoveries — they are well-documented limitations of the field that still have not been fully solved a decade later. That actually strengthens my research question rather than weakening it. I did not need to reshape the question, but I did learn that I should be more precise about which subtask I am focusing on, since "the full range of musical information" is a very large target. The 2019 Benetos overview's distinction between MIDI output and notation output is probably the single most useful conceptual tool for sharpening the paper — it gives me academic language for the gap I observed between what Basic Pitch produces and what a musician could actually read and perform from.
