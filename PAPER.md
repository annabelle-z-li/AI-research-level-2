# When the Map Loses the City:
## What a Lightweight AI Transcription Tool Actually Outputs — and What It Misses

*Annabelle Li | AI Research Level 2*

---

## 1. What I Wanted to Build

The use case is real: a musician — a student, a hobbyist, a composer sketching at a piano — plays something and wants it on paper. Hiring a professional transcriber is expensive. Writing it out by ear requires substantial theory training. The more-interesting version of this tool would close that gap entirely: upload an audio recording and receive accurate, performable sheet music in return.

My Space, **Music to Sheet Music** (*annabelle-li/music-to-sheet-music*), attempts exactly this. A user uploads an audio file or records live via microphone. The Space returns three outputs: a MIDI file, a MusicXML file, and an inline sheet music viewer that renders the transcribed score directly in the browser using SVG pages.

The version I actually built — constrained by free-tier infrastructure — is a three-stage pipeline: Spotify's **Basic Pitch** model (ONNX backend) analyzes the audio and detects pitches, outputting a MIDI file. **music21** parses that MIDI, quantizes note durations to a sixteenth-note grid, and converts the result to MusicXML. **LilyPond** renders that score into SVG pages displayed in the browser.

The more-interesting version would handle beat tracking, meter detection, overtone filtering, and polyphonic voice separation simultaneously — and it exists, in the form of models like Google Magenta's MT3. The question this paper investigates is why I could not deploy that version, and what the gap between what I built and what I wanted to build reveals about the current state of accessible AI music transcription.

---

## 2. The Rudimentary Baseline (Space 2)

Before building *Music to Sheet Music*, my second Space was **NYSSMA Sight-Singing Practice** — a Gradio app that generates sight-singing exercises across six NYSSMA levels, renders them as ABC notation via abcjs, and scores user recordings against a reference melody using librosa-based pitch and rhythm detection.

That Space demonstrated what a lightweight audio analysis pipeline *can* do reliably: it can detect whether a user's pitch is approximately correct relative to a target, measure rough rhythmic alignment, and give pass/fail feedback on a known expected melody. It was sufficient for a closed-loop practice tool because the reference answer was always known in advance.

What it could not do — and what made it insufficient as a transcription tool — was operate without a reference. Scoring a performance against a known melody is fundamentally different from transcribing an unknown one. The NYSSMA Space never had to infer meter, identify key, separate voices, or decide which of several simultaneous frequencies was the *intended* note. When I moved to open transcription in Space 3, every one of those inferences became necessary, and the lightweight pipeline had no mechanism for any of them.

---

## 3. The Constraint — The Wall

**The wall is the CPU-only free tier on Hugging Face Spaces, and it manifests in three specific ways.**

**First, model size.** MT3 (Multi-Task Multitrack Music Transcription, Gardner et al. 2022, ICLR) is the state-of-the-art model for notation-level transcription. It handles beat tracking, meter detection, polyphonic voice separation, and multi-instrument transcription simultaneously. It requires a GPU to run at any practical speed. A single inference pass on a 30-second audio clip takes approximately 45–90 seconds on a T4 GPU. On a free-tier CPU, that same inference would take several minutes per clip and would likely time out the Gradio request entirely. MT3 was not an option.

**Second, dependency conflicts during deployment.** Getting even Basic Pitch running on the free tier required resolving a cascade of incompatibilities. Python 3.13 — the default on newer Spaces — is incompatible with TensorFlow, which Basic Pitch's standard backend requires. Pinning to Python 3.10 resolved that, but introduced a conflict between the ONNX and TFLite backends and NumPy 2.0 (*numpy>=2.0 breaks onnxruntime<=1.16*). The fix was `numpy<2.0` and `basic-pitch[onnx]==0.3.3`. Separately, Gradio 4.x had a schema builder bug — `AttributeError: 'NoneType' object has no attribute 'get'` on startup — that required a runtime monkeypatch before the app could launch at all. Every one of these errors appeared before a single note of audio had been processed.

**Third, the MIDI ceiling.** MIDI records note events — pitch, onset time, offset time, velocity — but contains no information about beat, meter, key, or harmony [6]. Converting MIDI to readable sheet music requires inferring all of those things. My pipeline has no beat tracking step, no meter detection step, and no key inference step. music21's quantizer receives raw MIDI with no tempo map and must assign durations entirely by comparing onset and offset timestamps to a fixed sixteenth-note grid. When those timestamps are noisy — which they always are with real audio — the quantizer produces musically nonsensical rhythms. This is not a bug I can fix with a parameter change. It is a structural gap.

---

## 4. What I Tried First — the Failed and Partial Moves

**Beat tracking with librosa.** After seeing the rhythm errors on my first test, I added a beat-tracking step using `librosa.beat.beat_track()` with pretty_midi tempo correction before music21 quantization. The call ran without error and returned a tempo estimate of approximately 120 BPM for a C major scale recording. I passed that tempo to pretty_midi and set the music21 quantizer's smallest duration to match. The output was identical to the unmodified pipeline — same incorrect half note at the start, same spurious dotted rhythms mid-scale, same grace note cluster at the end. The beat tracker was running, but its output was not propagating into the quantizer in a way that changed the note duration assignments. I flagged this as an open research question rather than a solved problem.

**Filtering overtone clusters in MIDI.** The Twinkle Twinkle test produced three pages of dense chord clusters from what should have been a monophonic melody. Basic Pitch's multipitch estimation framework detects all frequencies simultaneously, including harmonic overtones of each fundamental pitch. I attempted to filter the MIDI output by removing notes whose velocities fell below a threshold (velocity < 40) on the theory that overtones would register as quieter than fundamentals. The cluster density decreased slightly — from roughly 4–5 stacked notes per beat to 3–4 — but the fundamental melody remained buried. The overtone pitches and the fundamental pitches have overlapping velocity distributions in Basic Pitch's output; there is no clean threshold that separates them.

**Trying MuseScore as an alternative renderer.** LilyPond requires installation and a compiled binary, which created deployment friction. I explored MuseScore's command-line rendering mode as an alternative. MuseScore is not available on the Hugging Face free tier without a custom Docker image, which requires a paid account to deploy. That path was closed.

---

## 5. The Move That Worked (Space 3) — AMT Report Card

Since I could not fix the transcription pipeline itself, I built a second Space — **AMT Report Card** (*annabelle-li/amt-report-card*) — that reframes the problem entirely. Instead of trying to produce accurate sheet music, it measures and explains *how inaccurate* the transcription is.

The architecture is as follows: a user uploads two files — an audio recording and a reference MIDI of what the audio *should* sound like. The Space runs the same Basic Pitch (ONNX) → music21 pipeline to produce a transcription MIDI, then scores it against the reference MIDI across four dimensions: pitch accuracy (percentage of correct pitches within a half-step tolerance), timing accuracy (mean onset deviation in seconds), rhythm accuracy (ratio of correctly quantized note durations), and dynamics preservation (correlation of velocity profiles). Each dimension receives a numerical score and a letter grade.

The second component is an LLM-based musician's review via the Groq API (*llama3-8b-8192* model). The four scores are passed to Groq along with a prompt that asks for a plain-language assessment written from a musician's perspective — not a data summary, but an evaluation of whether the transcription would be usable for performance, practice, or notation purposes. This runs on Groq's free tier, which provides fast inference with no local compute cost.

The deployment surface: Hugging Face Spaces (free CPU tier) for the Gradio frontend and Basic Pitch pipeline; Groq API for LLM inference. No GPU required. The key architectural insight is that *evaluating* a transcription requires much less compute than *producing* one — scoring MIDI against MIDI is arithmetic, and the LLM call is stateless and fast on Groq.

---

## 6. What the Move Cost Me

**External dependency on Groq.** The musician's review requires a live Groq API call. If the Groq API is down, rate-limited, or the key expires, that component of the Space silently fails. The Space degrades gracefully (the numerical scores still display), but the most useful output — the plain-language review — disappears. I have no control over Groq's uptime or rate limits.

**Latency.** The full pipeline — Basic Pitch transcription, MIDI scoring, and Groq LLM call — takes approximately 15–25 seconds on the free CPU tier for a 30-second audio clip. For a tool meant to give real-time feedback, that latency is noticeable. MT3 would be slower still, but it would at least produce output worth waiting for.

**Loss of the original goal.** AMT Report Card measures transcription quality but does not improve it. A musician using this tool still receives unusable sheet music — the Space just now tells them *how* unusable it is. I shifted from trying to solve the transcription problem to trying to make the failure legible. That is a real move, but it is not the same as solving the problem.

**Reference MIDI dependency.** To use AMT Report Card, the user must already have a reference MIDI — which means they either need to know how to create one or have access to one from another source. The musicians who most need help with transcription (those without theory training or notation software) are least likely to have a reference MIDI on hand. The tool's utility is partially inverted from its intended audience.

**Privacy.** Audio files uploaded to a Hugging Face Space on the free tier are processed in a shared compute environment. Users should not upload recordings of proprietary or commercially sensitive performances. This is a real constraint for professional musicians even if it does not affect student use cases.

---

## 7. What I'd Do Next

**The next constraint: quantitative output at scale.** The AMT Report Card produces scores, but I have only run it on three test recordings. To make claims about *how much* Basic Pitch fails, and under what conditions, I need a batch evaluation pipeline — something that runs dozens of audio/MIDI pairs and produces aggregate accuracy statistics across pitch complexity, polyphony level, and instrument type. That is the next thing I would build.

**The next move: accessing GPU inference.** Hugging Face's Inference API provides GPU-backed inference for certain models without requiring a paid Space. If MT3 or a comparable beat-tracking model becomes available there, the pipeline could be restructured to call it via HTTP rather than running locally — trading local CPU compute for an external API call, similar to what I did with Groq. That architectural pattern is already proven in AMT Report Card; it just needs to be applied to the transcription stage rather than the evaluation stage.

**What's still real as a limitation: the training data problem.** Gardner et al. note that all existing open-source music transcription datasets combined contain fewer total hours of audio than a single standard speech recognition dataset. Even if I had GPU access and could deploy MT3, the model's accuracy ceiling is constrained by how little labeled music data was available to train it. This is a structural problem that individual researchers cannot solve — it requires coordinated dataset creation across the music research community. The gap between speech recognition accuracy (~95%+) and music transcription accuracy (~60–70% on clean monophonic audio) is partly a compute story, but it is also a data story.

**Open research question.** The librosa beat-tracking fix ran without error but produced no measurable improvement in rhythm output. I do not know why. The tempo estimate was plausible (120 BPM for the scale recording); the propagation into music21's quantizer did not behave as expected. Resolving that — either by confirming the fix is structurally impossible within the current pipeline, or by finding the correct integration point — would be the first specific debugging task before attempting any further rhythm improvement.

---

## Acknowledgments

This research was conducted entirely on free-tier infrastructure. The following tools made it possible:

- **Hugging Face Spaces** (free CPU tier) — deployment platform for both Spaces
- **Basic Pitch** (Spotify) — lightweight polyphonic note transcription model, Apache 2.0 license
- **music21** (MIT / Michael Cuthbert) — MIDI parsing, quantization, and MusicXML conversion
- **LilyPond** — professional music engraving and SVG rendering
- **Groq API** (free tier) — low-latency LLM inference for musician's review generation
- **Gradio** — interactive web UI for both Spaces
- **librosa** — audio analysis and beat tracking utilities

---

## References

[1] Bittner, R. M., Bosch, J. J., Rubinstein, D., Meseguer-Brocal, G., & Ewert, S. (2022). A lightweight instrument-agnostic model for polyphonic note transcription and multipitch estimation. *Proceedings of ICASSP.* https://github.com/spotify/basic-pitch

[2] Jamshidi, F., Pike, G., Das, A., & Chapman, R. (2024). Machine learning techniques in automatic music transcription: A systematic survey. *arXiv preprint arXiv:2406.15249.* https://arxiv.org/abs/2406.15249

[3] Gardner, J., Simon, I., Manilow, E., Hawthorne, C., & Engel, J. (2022). MT3: Multi-task multitrack music transcription. *ICLR 2022.* https://arxiv.org/abs/2111.03017

[4] Bereket, M., & Shi, K. (2017). An AI approach to automatic natural music transcription. *CS229 Final Project Report.* Stanford University. https://cs229.stanford.edu/proj2017/final-reports/5244388.pdf

[5] Benetos, E., Dixon, S., Giannoulis, D., Kirchhoff, H., & Klapuri, A. (2013). Automatic music transcription: Challenges and future directions. *Journal of Intelligent Information Systems, 41*(3), 407–434. https://doi.org/10.1007/s10844-013-0258-3

[6] Benetos, E., Dixon, S., Duan, Z., & Ewert, S. (2019). Automatic music transcription: An overview. *IEEE Signal Processing Magazine, 36*(1), 20–30. https://doi.org/10.1109/MSP.2018.2869928
