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
| `research-journal.md` | Running log of Spaces explored, models tested, Spaces built, and weekly reflections |
| `week-6-research-question.md` | Deep dive into the Week 6 research question: how it was developed using Claude, what the three candidate questions were, and why the final version was chosen |

---

## Spaces Explored

| Space | Rating | Notes |
|-------|--------|-------|
| [SongGeneration](https://huggingface.co/spaces/tencent/SongGeneration) | 9/10 | Impressively captured the emotional tone of a prompt without being told to |
| [Huggy](https://huggingface.co/spaces/ThomasSimonini/Huggy) | 7/10 | Cute but the physics were a bit seizure-y |
| [Doodle Dash](https://huggingface.co/spaces/Xenova/doodle-dash) | 8/10 | More varied prompts than Google Quick Draw, less accurate guessing |
| [DiffRhythm](https://huggingface.co/spaces/ASLP-lab/DiffRhythm) | 4/10 | Very tedious to use and produced a happy techno song from sad lyrics |
| [Simple Image Classifier](https://huggingface.co/spaces/Nuno-Tome/simple_image_classifier) | — | Used for Week 4 classification vs. generation demo |
| [C4AI Command](https://huggingface.co/spaces/CohereLabs/c4ai-command) | — | Used for Week 4 generation demo |

---

## Models Explored

| Model | Rating | Notes |
|-------|--------|-------|
| [DeepSeek-R1](https://huggingface.co/deepseek-ai/DeepSeek-R1) | 9.5/10 | On par with ChatGPT — impressive for an open-source model |
| [all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) | 7/10 | Sentence transformer; painful to set up in Colab but works well |

---

## Spaces Built

### [Dino Fact Explorer](https://huggingface.co/spaces/annabelle-li/dino-fact-explorers-fa8or)
Built using DeepSite vibe coding. Clicking a button returns a random dinosaur fact. Learned that the "facts" were actually 25 hardcoded strings in `script.js`, and that the majority of the code was actually CSS animations (bounce, float, shake).

### [Dictionary](https://huggingface.co/spaces/annabelle-li/dictionary)
A simple lookup app built with Gradio. Spent a long time debugging before realizing the issue was missing `import gradio as gr` and `import requests`. Lesson: read the source material first.

### [Silly Phrase Finder](https://huggingface.co/spaces/annabelle-li/silly-phrase-finder/tree/main)
Built in class. Later encountered an `init` error and fixed it by resetting the Space in settings. Lesson: `init` errors → just reset.

### [Music Starter: Opera & Jazz](https://huggingface.co/spaces/annabelle-li/music-starter)
A text-generation app for composers and students. Uses `SmolLM2-360M-Instruct` with five music modes (Opera Libretto, Opera Aria Lyrics, Opera Chord Progression, Jazz Song Lyrics, Jazz Chord Progression), each with a different hidden system instruction. Has sliders for Temperature, Top-p, and Max New Tokens.

Key bug fixed: the model was loading but generating nothing, because the prompt wasn't being passed through `tokenizer.apply_chat_template()`. Once switched from `pipeline` to direct `model.generate()` calls with the chat template applied, it worked immediately.

**Finding:** High temperature improved aria lyrics but destroyed chord progressions (generated nonexistent chords).

### [Music to Sheet Music](https://huggingface.co/spaces/annabelle-li/music-transcription) *(Research Space)*
The central research artifact. Takes audio input (file upload or microphone) and transcribes it to sheet music, outputting MIDI, MusicXML, and an inline SVG score via LilyPond.

**Tech stack:** `basic_pitch` (Spotify) → `music21` → LilyPond

**Finding:** Output was broadly inaccurate across all dimensions simultaneously — wrong pitches, wrong clef, wrong rhythms, wrong time signature. This is a cascade of compounding errors across three pipeline stages, documented in detail in `research-journal.md`.

**Root causes identified:**

| Error | Stage | Cause |
|-------|-------|-------|
| Wrong pitches | `basic_pitch` | Overtone/noise misidentification |
| Wrong clef | `music21` | Phantom pitches skew pitch range |
| Wrong rhythms | Both | No beat tracking; coarse quantization grid |
| Wrong time signature | `music21` | No meter detection in pipeline |

**What's missing:** Beat tracking (e.g., `librosa.beat.beat_track`) and meter detection before quantization.

---

## Weekly Topics

| Week | Topic | Key Takeaway |
|------|-------|--------------|
| 4 | Classification vs. Generation | Classification needs labeled data and is more constrained; generation is more flexible but less grounded — DistilGPT would ramble about eggs when given a math problem about eggs |
| 5 | Adding Controls | Built the Opera & Jazz Space; learned that instruct models require `apply_chat_template` or they don't recognize the input as a prompt at all |
| 6 | Research Question | Shifted focus from text generation to transcription; developed a research question about AMT accuracy constraints and what they mean for real musicians |

---

## What's Next

- **AMT Accuracy Tester Space** — upload a test audio clip and a reference MIDI, run Basic Pitch automatically, and compare output against ground truth across four dimensions: pitch (% correct), timing (average onset error in ms), rhythm (duration match), and dynamics (whether soft notes were dropped)
- Instrument comparison Space (piano vs. guitar vs. voice)
- Compute tradeoff Space (Basic Pitch vs. a heavier model on the same clip)
- Lead sheet / chord chart generation from transcription output

---

## References

- Bitteur et al., *basic_pitch* — https://github.com/spotify/basic-pitch
- Cuthbert & Ariza, *music21* — https://web.mit.edu/music21/
- McFee et al., *librosa* — https://librosa.org
- Kong et al., *High-resolution Piano Transcription with Pedals by Regressing Onset and Offset Times* (2021)
- AI Music Transcription survey — https://arxiv.org/html/2603.27528v1
