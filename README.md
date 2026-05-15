# AI Research — Level 2

A research journal and project portfolio from an applied AI course, exploring Hugging Face Spaces, machine learning models, and hands-on music AI experiments.

---

## Table of Contents

- [About This Repository](#about-this-repository)
- [Research Question](#research-question)
- [Files](#files)
- [Spaces Explored](#spaces-explored)
- [Models Explored](#models-explored)
- [Spaces Built](#spaces-built)
- [Weekly Topics](#weekly-topics)

---

## About This Repository

This repository documents a semester of applied AI research, including reflections on existing Hugging Face Spaces and models, original Spaces built from scratch, and a deepening investigation into AI music transcription. The central thread is a research question about what automatic music transcription (AMT) models can and can't do — and what that means for real musicians trying to use them.

---

## Research Question

> **How do the architectural and computational constraints of AI music transcription models affect their ability to accurately represent the full range of musical information — including pitch, rhythm, dynamics, and instrumentation — and what are the implications for making these tools accessible and useful in real musical contexts?**

This question came out of firsthand observation: after building a music transcription Space using `basic_pitch`, `music21`, and LilyPond, the output was broadly inaccurate across pitch, rhythm, clef, and time signature simultaneously. That failure became the starting point for a structured research brief.

**Testable sub-problems this question opens up:**

- Do lightweight CPU models like Basic Pitch perform systematically worse on polyphonic audio than monophonic audio, and at what level of polyphony does accuracy degrade?
- How do model architecture and compute requirements trade off against transcription accuracy?
- What do musicians actually need from a transcription tool — and does current output meet that bar?
- Can we build a side-by-side comparison of Basic Pitch vs. a heavier model on the same audio clip?

---

## Files

| File | Description |
|------|-------------|
| `PAPER.md` | Research brief: what the pipeline outputs, why it fails, and what that means for accessibility and usability |
| `research-journal.md` | Running log of Spaces explored, models tested, Spaces built, and weekly reflections |
| `paper-starter.md` | Early draft notes and outline for the research brief |
| `week-06-research-question.md` | Deep dive into the Week 6 research question: how it was developed, the three candidate questions, and why the final version was chosen |
| `week-07-source-search.md` | Source shortlist with extracts, inclusion/exclusion record, and reflection |
| `week-08-paper-read.md` | Close reading of Bereket & Shi (2017): field, terms, methods, and what to cite |
| `week-09-citations.md` | Verified citations for all four sources used in the brief, with claim-level evidence and peer-review status |

---

## Spaces Explored

| Space | Status | Notes |
|-------|--------|-------|
| [SongGeneration](https://huggingface.co/spaces/tencent/SongGeneration) | 9/10 — not pursuing | Impressively captured emotional tone without being told to; led me to the idea of music + AI, but generation isn't my focus |
| [DiffRhythm](https://huggingface.co/spaces/ASLP-lab/DiffRhythm) | 4/10 — rejected | Fast generation but required `[mm:ss:ms]` timestamps for each lyric; not focusing on generation |
| [Piano Transcriptor](https://huggingface.co/spaces/xGPU-Explorers/piano_trans) | Relevant | Uses librosa, directly relevant to what I'm studying |
| [Midi Music Generator](https://huggingface.co/spaces/skytnt/midi-composer) | Relevant | Generates MIDI files — the same format my pipeline outputs |
| [Music Descriptor](https://huggingface.co/spaces/m-a-p/Music-Descriptor) | Relevant | Analyzes music for genres, instruments, and emotions — useful for helping growing musicians |
| [Music Genre Classifier](https://huggingface.co/spaces/ardneebwar/music-genre-classifier) | Relevant | Classifies music genres from audio — useful for music creation context |
| [Giant Music Transformer](https://huggingface.co/spaces/asigalov61/Giant-Music-Transformer) | Relevant | Fast multi-instrumental music transformer — useful for music creativity |
| [Music Arena Leaderboard](https://huggingface.co/spaces/ArtificialAnalysis/Music-Arena-Leaderboard) | Saved for later | Shows which generation model is best; Suno is #1 but generation isn't my current focus |
| [Huggy](https://huggingface.co/spaces/ThomasSimonini/Huggy) | 7/10 | Cute but the physics were a bit seizure-y |
| [Doodle Dash](https://huggingface.co/spaces/Xenova/doodle-dash) | 8/10 | More varied prompts than Google Quick Draw, less accurate guessing |
| [Simple Image Classifier](https://huggingface.co/spaces/Nuno-Tome/simple_image_classifier) | — | Used for Week 4 classification vs. generation demo |
| [C4AI Command](https://huggingface.co/spaces/CohereLabs/c4ai-command) | — | Used for Week 4 generation demo |

---

## Models Explored

| Model | Status | Notes |
|-------|--------|-------|
| [DeepSeek-R1](https://huggingface.co/deepseek-ai/DeepSeek-R1) | 9.5/10 | On par with ChatGPT — impressive for an open-source model |
| [all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) | 7/10 | Sentence transformer; painful to set up in Colab but works well |
| [ACE-Step 1.5](https://huggingface.co/ACE-Step/Ace-Step1.5) | Saved for later | Best open-source music generation model; not my current focus |
| [HeartMuLa-oss-3B](https://huggingface.co/HeartMuLa/HeartMuLa-oss-3B-happy-new-year) | Saved for later | Also a strong music generation model |

---

## Spaces Built

### [Dino Fact Explorer](https://huggingface.co/spaces/annabelle-li/dino-fact-explorers-fa8or)
Built using DeepSite vibe coding. Clicking a button returns a random dinosaur fact. Learned that the "facts" were actually 25 hardcoded strings in `script.js`, and that the majority of the code was CSS animations (bounce, float, shake).

### [Dictionary](https://huggingface.co/spaces/annabelle-li/dictionary)
A simple lookup app built with Gradio. Spent a long time debugging before realizing the issue was missing `import gradio as gr` and `import requests`. Lesson: read the source material first.

### [Silly Phrase Finder](https://huggingface.co/spaces/annabelle-li/silly-phrase-finder/tree/main)
Built in class. Later encountered an `init` error and fixed it by resetting the Space in settings. Lesson: `init` errors → just reset.

### [Music Starter: Opera & Jazz](https://huggingface.co/spaces/annabelle-li/music-starter)
A text-generation app for composers and students. Uses `SmolLM2-360M-Instruct` with five music modes (Opera Libretto, Opera Aria Lyrics, Opera Chord Progression, Jazz Song Lyrics, Jazz Chord Progression), each with a different hidden system instruction. Has sliders for Temperature, Top-p, and Max New Tokens.

Key bug fixed: the model was loading but generating nothing, because the prompt wasn't being passed through `tokenizer.apply_chat_template()`. Once switched from `pipeline` to direct `model.generate()` calls with the chat template applied, it worked immediately.

**Finding:** High temperature improved aria lyrics but destroyed chord progressions (generated nonexistent chords).

### [Music to Sheet Music](https://huggingface.co/spaces/annabelle-li/music-to-sheet-music) *(Primary Research Space)*
The central research artifact. Takes audio input (file upload or microphone) and transcribes it to sheet music, outputting MIDI, MusicXML, and an inline SVG score via LilyPond. Also runs an AI analysis of the transcription via Groq.

**Tech stack:** `basic_pitch` (Spotify) → `librosa` beat tracking → `music21` → LilyPond

**Finding:** Output was broadly inaccurate across all dimensions simultaneously — wrong pitches, wrong clef, wrong rhythms, wrong time signature. This is a cascade of compounding errors across three pipeline stages, documented in detail in `research-journal.md`.

**Root causes identified:**

| Error | Stage | Cause |
|-------|-------|-------|
| Wrong pitches | `basic_pitch` | Overtone/noise misidentification |
| Wrong clef | `music21` | Phantom pitches skew pitch range |
| Wrong rhythms | Both | No beat tracking; coarse quantization grid |
| Wrong time signature | `music21` | No meter detection in pipeline |

**Week 10 update:** Added `librosa.beat.beat_track()` to detect BPM from audio and write it into the MIDI via `pretty_midi` before quantization. Rhythm output on the C major scale test did not change — the failure appears to be upstream of tempo correction, likely in how Basic Pitch encodes note durations before meter is relevant.

### [AMT Report Card](https://huggingface.co/spaces/annabelle-li/amt-report-card) *(Research Tool)*
Scores an AI music transcription against a reference MIDI across four dimensions — pitch, timing, rhythm, and dynamics — then asks an LLM to review the results like a musician would. Includes three built-in examples (C major scale, Twinkle Twinkle Little Star, Minuet in G) that reproduce the tests from the research brief. Also includes the beat tracking fix applied to the hypothesis MIDI before scoring.

**Tech stack:** `basic_pitch` → `librosa` beat tracking → `music21` → Groq (LLaMA 3.3 70B) → radar chart + scorecard UI

**Architecture:** Basic Pitch (ONNX, CPU-compatible) detects note events from audio → `music21` parses and quantizes the hypothesis MIDI → scores are computed against the reference MIDI across four dimensions → MusicXML is passed to Groq's LLaMA 3.3 70B for a musician's-eye review → results displayed as a letter-grade scorecard, radar chart, and sheet music viewer.

**The constraint Music to Sheet Music hit:** The pipeline had no tempo information between Basic Pitch and music21 quantization. Without a detected BPM, music21 snapped note durations to an arbitrary default grid — meaning even when pitch detection was correct (as in the C major scale), the rhythmic output was meaningless.

**The move:** Added `librosa.beat.beat_track()` to estimate BPM from the original audio, wrote that tempo into the hypothesis MIDI using `pretty_midi`, and passed the corrected MIDI to music21 so quantization ran against a beat-aligned grid.

**The cost:** The fix added two new dependencies (`librosa`, `pretty_midi`) and a second audio-loading pass — adding overhead on an already slow CPU tier. More importantly, the rhythm output did not change. The failure appears to be upstream of tempo correction, likely in how Basic Pitch encodes note durations before meter is relevant. The fix is dead weight until the root cause is identified.

---

## Weekly Topics

| Week | Topic | Key Takeaway |
|------|-------|--------------|
| 4 | Classification vs. Generation | Classification needs labeled data and is more constrained; generation is more flexible but less grounded — DistilGPT would ramble about eggs when given a math problem about eggs |
| 5 | Adding Controls | Built the Opera & Jazz Space; learned that instruct models require `apply_chat_template` or they don't recognize the input as a prompt at all |
| 6 | Research Question | Shifted focus from text generation to transcription; developed a research question about AMT accuracy constraints and what they mean for real musicians |
| 7 | Source Search | Found four sources directly supporting the research question; Benetos et al. (2019) gave the clearest academic framing for the MIDI-to-notation gap |
| 8 | Paper Read | Close reading of Bereket & Shi (2017); the note-fading artifact (model truncates note durations because piano audio fades before the MIDI ground truth ends) is the most citable finding |
| 9 | Citations & Testing | All four sources survived verification; tested AMT Report Card on three examples — C major scale got pitch right but rhythm wrong; Twinkle and Minuet failed across all dimensions |
| 10 | The Wall and the Move | Added librosa beat tracking to fix rhythm errors; the fix ran without errors but produced no change in output — the failure is upstream of tempo correction |

---

## What's Next

- Determine why beat tracking didn't improve rhythm output — whether the issue is in how Basic Pitch encodes note durations or in how music21 quantizes regardless of BPM
- Instrument comparison Space (piano vs. guitar vs. voice)
- Compute tradeoff Space (Basic Pitch vs. a heavier model on the same clip)
- Lead sheet / chord chart generation from transcription output

---

## References

- Bittner, R. M., Bosch, J. J., Rubinstein, D., Meseguer-Brocal, G., & Ewert, S. (2022). A lightweight instrument-agnostic model for polyphonic note transcription and multipitch estimation. *ICASSP 2022*. https://github.com/spotify/basic-pitch
- Jamshidi, F., Pike, G., Das, A., & Chapman, R. (2024). Machine learning techniques in automatic music transcription: A systematic survey. *arXiv:2406.15249*. https://arxiv.org/abs/2406.15249
- Gardner, J., Simon, I., Manilow, E., Hawthorne, C., & Engel, J. (2022). MT3: Multi-task multitrack music transcription. *ICLR 2022*. https://arxiv.org/abs/2111.03017
- Bereket, M., & Shi, K. (2017). An AI approach to automatic natural music transcription. *Stanford CS229*. https://cs229.stanford.edu/proj2017/final-reports/5244388.pdf
- Benetos, E., Dixon, S., Giannoulis, D., Kirchhoff, H., & Klapuri, A. (2013). Automatic music transcription: Challenges and future directions. *Journal of Intelligent Information Systems, 41*(3), 407–434. https://doi.org/10.1007/s10844-013-0258-3
- Benetos, E., Dixon, S., Duan, Z., & Ewert, S. (2019). Automatic music transcription: An overview. *IEEE Signal Processing Magazine, 36*(1), 20–30. https://doi.org/10.1109/MSP.2018.2869928
- Cuthbert, M. S., & Ariza, C. music21. https://web.mit.edu/music21/
- McFee, B. et al. librosa. https://librosa.org
