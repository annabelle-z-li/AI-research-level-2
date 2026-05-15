# Week 09 — Citations & Reference Verification
**For:** Week 10 Research Brief — References Section

---

## Peer Review Status

Three of the four sources on this list are peer-reviewed: Benetos et al. (2013) was published in the *Journal of Intelligent Information Systems* (Springer); Benetos et al. (2019) was published in *IEEE Signal Processing Magazine*; and Gardner et al. (2022) was published at ICLR 2022, a competitive peer-reviewed conference. The fourth source — Bereket & Shi (2017) — is a Stanford CS229 final project report. It is not peer-reviewed and should be labeled as such in the brief. It is included because it offers a concrete, citable illustration of the acoustic modeling / score generation distinction and the note-fading artifact, neither of which is easily found in a brief passage from the larger survey papers.

---

## Citations with Claim Verification

---

### Source 1 — Bereket & Shi (2017)
**NOT PEER-REVIEWED — Stanford CS229 course project report**

**APA Citation:**
Bereket, M., & Shi, K. (2017). *An AI approach to automatic natural music transcription* [CS229 Final Project Report]. Stanford University. https://cs229.stanford.edu/proj2017/final-reports/5244388.pdf

**DOI:** None (course report, URL only). URL verified: resolves to the correct PDF.

**Claim I plan to cite:** A structural mismatch between audio input and MIDI ground truth causes the model to systematically truncate note durations — real piano audio fades in volume before the note ends, so the CNN cannot identify the trailing portion, and no amount of architectural tuning resolves this.

**Supporting passage from paper (Section 7, Experiments and Results):**
> "There is still some noise picked up and not all notes are identified for their full duration. However, this is likely to be due to the fading volumes of some notes; while the MIDI file would show the full duration of the note, our audio input would not hear the end of some notes with the same amplitude, and thus, our CNN model would not be able to identify the trailing ends as easily."

*(Section 7, Experiments and Results)*

---

### Source 2 — Gardner et al. (2022)
**PEER-REVIEWED — ICLR 2022 conference paper**

**APA Citation:**
Gardner, J., Simon, I., Manilow, E., Hawthorne, C., & Engel, J. (2022). MT3: Multi-task multitrack music transcription. *Proceedings of the International Conference on Learning Representations (ICLR 2022)*. https://arxiv.org/abs/2111.03017

**DOI:** arXiv:2111.03017 — verify at https://arxiv.org/abs/2111.03017 before submitting; confirm ICLR publication status at https://openreview.net/forum?id=iMSjopcOn0p

**Claim I plan to cite:** AMT is structurally a low-resource problem — music datasets are far smaller than comparable speech datasets, which is one reason transcription models lag behind speech recognition; and the most capable models require compute unavailable on free-tier deployment infrastructure.

**Supporting passage from paper (Section 1, Introduction):**
> "Compounding this challenge, many music datasets are relatively small in comparison to the datasets used to train large-scale sequence models in other domains such as NLP or ASR. Existing open-source music transcription datasets contain between one and a few hundred hours of audio, while standard ASR datasets LibriSpeech and CommonVoice contain 1k and 9k+ hours of audio, respectively. LibriSpeech alone contains more hours of audio than all of the AMT datasets we use in this paper, combined."

*(Section 1, Introduction)*

---

### Source 3 — Benetos et al. (2013)
**PEER-REVIEWED — Journal of Intelligent Information Systems (Springer)**

**APA Citation:**
Benetos, E., Dixon, S., Giannoulis, D., Kirchhoff, H., & Klapuri, A. (2013). Automatic music transcription: Challenges and future directions. *Journal of Intelligent Information Systems, 41*(3), 407–434. https://doi.org/10.1007/s10844-013-0258-3

**DOI:** https://doi.org/10.1007/s10844-013-0258-3 — verify this resolves to the Springer article page before submitting.

**Claim I plan to cite:** AMT decomposes into distinct subtasks — pitch detection, onset/offset detection, rhythm extraction, dynamics estimation, time quantization — and a model that only solves one of them well will still produce unusable output overall, because failures cascade.

**Supporting passage from paper (Section 1 / Introduction):**
> "The AMT problem can be divided into several subtasks, which include: multi-pitch detection, note onset/offset detection, loudness estimation and quantisation, instrument recognition, extraction of rhythmic information, and time quantisation. The core problem in automatic transcription is the estimation of concurrent pitches in a time frame, also called multiple-F0 or multi-pitch detection."

*(Section 1, Introduction — available via open-access postprint at https://openaccess.city.ac.uk/id/eprint/2524/)*

---

### Source 4 — Benetos et al. (2019)
**PEER-REVIEWED — IEEE Signal Processing Magazine**

**APA Citation:**
Benetos, E., Dixon, S., Duan, Z., & Ewert, S. (2019). Automatic music transcription: An overview. *IEEE Signal Processing Magazine, 36*(1), 20–30. https://doi.org/10.1109/MSP.2018.2869928

**DOI:** https://doi.org/10.1109/MSP.2018.2869928 — verify this resolves to the IEEE article page before submitting.

**Claim I plan to cite:** MIDI output and music notation are not the same thing — beat, meter, key, and harmony are absent from MIDI — so "audio to MIDI" and "audio to readable sheet music" are fundamentally different problems, and the gap between them is where lightweight pipelines fail.

**Supporting passage from paper:**
> "It comprises several subtasks, including (multi-)pitch estimation, onset and offset detection, instrument recognition, beat and rhythm tracking, interpretation of expressive timing and dynamics, and score typesetting."

---

## Reflection

All four Week 7 sources are carried forward into this citation file and none were dropped at the formatting stage. But one source from the Week 7 near-miss list, the 2024 MT3 follow-up (YourMT3+), may be worth revisiting if I need a more current reference for the state-of-the-art gap argument, but only if I can verify the claim and find the supporting passage.
