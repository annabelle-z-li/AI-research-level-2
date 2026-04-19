# When the Map Loses the City: What a Lightweight AI Transcription Tool Actually Outputs — and What It Misses

## 1. What I Built

For this project, I built a Hugging Face Space called **Music to Sheet Music** (`annabelle-li/music-to-sheet-music`). The idea is simple: a user uploads an audio recording — either a file or a live microphone recording — and the Space returns three things: a MIDI file, a MusicXML file, and an inline sheet music viewer that renders the transcribed score directly in the browser.

Under the hood, the pipeline works in three stages. First, Spotify's **Basic Pitch** model — running on the ONNX backend — analyzes the audio and detects pitches, converting them into a MIDI file. Second, the Python library **music21** parses that MIDI, quantizes the note durations to a rhythmic grid (the smallest unit being a sixteenth note), and converts the result into a MusicXML score. Third, **LilyPond** — a professional music engraving program — renders that score into SVG pages that appear directly in the browser as sheet music.

One design choice that matters a lot: the Space runs on a **free CPU tier** on Hugging Face Spaces. This means no GPU acceleration, which directly limits which transcription models are even possible to use. Basic Pitch was chosen specifically because it is small and fast enough to run on CPU. That constraint — CPU-only, lightweight model — is not just a technical footnote. It is the central tension this paper investigates.

It is also worth being honest about how difficult it was to get this Space running at all. I went through multiple rounds of errors: Python 3.13 was incompatible with TensorFlow, which Basic Pitch depends on; the ONNX and TFLite backends conflicted with different NumPy versions; Gradio's own schema builder had a bug that crashed the app at startup and required a monkeypatch fix. Every one of these errors was a reminder that even *deploying* a lightweight model — before evaluating a single note of output — requires navigating a complex and fragile stack of dependencies. Accessibility is not just about whether a model exists. It is about whether a student with a free-tier account can actually get it to run.

## 2. My Research Question

**How do the architectural and computational constraints of AI music transcription models affect their ability to accurately represent the full range of musical information — including pitch, rhythm, dynamics, and instrumentation — and what are the implications for making these tools accessible and useful in real musical contexts?**

This question came from a specific frustration. When I ran my first test recording through the Space, the sheet music viewer loaded — which felt like a success — but when I actually looked at the score, almost everything was wrong. The pitches were wrong. The rhythms were wrong. The time signature was wrong or missing. The clef was wrong. It was not one error but a cascade of errors across every dimension of musical information simultaneously.

That observation pushed me toward a deeper question: is this just a bug I can fix, or is it revealing something more fundamental about what this category of model — small, CPU-compatible, designed for accessibility — can and cannot do? I started to suspect it was the latter. The gap between what the Space outputs and what a musician could actually use is not a debugging problem. It is a research question.

## 3. Why This Matters to Me

I have been studying music seriously for several years. I have gone through NYSSMA (New York State School Music Association) evaluations, which involve sight-singing — reading and performing music you have never seen before, in real time, with attention to pitch, rhythm, dynamics, and phrasing. That training gave me a specific standard for what accurate musical notation looks like and what it feels like to read and perform from a score.

When I looked at the output of my Space, I was not just looking at a file. I was reading it the way I would read any piece of music before performing it. And what I saw was not usable. The rhythms did not make musical sense. Notes that should have been grouped as quarter notes or dotted rhythms were smeared into strange durations. The time signature, which tells a performer how to feel the beat and count the measure, was either absent or wrong. A musician trying to perform from this output would be more confused than helped.

This matters beyond my own experience. One of the promises of AI transcription tools is that they can make music more accessible — that a musician who cannot afford a professional transcriber, or who does not have the theory training to write out music by ear, could use a tool like this instead. If the output is this far from usable, then the tool is not actually delivering on that promise. Understanding *why* it fails, and *which* musical dimensions fail first, is the first step toward knowing what it would actually take to close that gap.

My earlier work in this class also pointed me here. When I was building Music Starter: Opera & Jazz — a text generation Space — I noticed that the model was especially bad at musical specifics. When I asked it to generate chord progressions, it invented chord names that do not exist. That observation made me curious about a different question: not whether AI can write *about* music, but whether it can accurately *represent* music. Transcription felt like the more concrete, more testable version of that question.

## 4. What I Tried

The clearest test I ran was examining the pipeline failure in detail — not by running a formal experiment, but by auditing each stage of the pipeline individually to understand where errors were introduced.

**What I expected:** that the output would be imperfect but approximately correct — maybe a few wrong notes, some rhythm approximations, but broadly readable as sheet music.

**What happened:** the errors were not minor approximations. They were systematic failures across every musical dimension:

| Musical Dimension | What Went Wrong | Pipeline Stage |
|---|---|---|
| Pitch | Overtones misread as separate notes; phantom pitches added | Basic Pitch (audio → MIDI) |
| Clef | Wrong clef assigned because phantom pitches skewed the pitch range | music21 (MIDI → score) |
| Rhythm | Note durations incorrect; no beat tracking before quantization | Basic Pitch + music21 |
| Time signature | Defaulted to 4/4 regardless of actual meter | music21 (no meter detection) |
| Dynamics | No velocity or dynamic information preserved | Basic Pitch model design |

The root cause of most of these errors is the same: **Basic Pitch was not designed to be a full automatic music transcription system**. It is a pitch detection tool. It detects where pitches occur in audio, but it does not understand meter, beat, or musical context. When music21 receives that raw pitch data and tries to turn it into notation, it has no beat tracking information to work from — so it guesses at rhythm by snapping everything to a sixteenth-note grid, and it guesses at time signature by defaulting to 4/4. The result is technically a score, but not a musical one.

One specific failure that stood out: piano audio with strong overtones caused Basic Pitch to detect the overtones as separate pitches. This is a known limitation. As one researcher put it, Basic Pitch assessed the low sounds not as the struck note but as noise, omitting that pitch and instead selecting two of its overtones as the active pitches — ultimately transcribing a very different chord from the one was actually played.

This is exactly what I observed. The model is not wrong about the frequencies it detects. It is wrong about what those frequencies *mean* musically.

## 5. What I Learned

The most important thing I learned is that there is a fundamental difference between **detecting pitches** and **representing music**. Basic Pitch is genuinely good at detecting where sounds occur in audio. The problem is that music notation is not a record of sounds — it is an abstraction that a human performer uses to reconstruct a musical intention. As one reviewer of a similar AI transcription tool put it, sheet music and tab are not precise documentation of the notes as performed; they are abstractions. If a piece of music is like a city, then a notated score is like a street map. You have to leave out a huge amount of information if you want the map to be readable. Deciding what to include and what to omit is an art, not a science.

Basic Pitch — and tools like it — are good at describing the city. They are not yet capable of drawing the map in a way a musician can navigate.

I also learned that the CPU constraint is not just a performance limitation — it is a filter that excludes the models capable of actually solving this problem. The models that can handle beat tracking, meter detection, and polyphonic transcription simultaneously (like Google Magenta's MT3) require GPU resources that are not available on a free-tier Space. This means that the tools most accessible to students and musicians with limited resources are also, by necessity, the least musically accurate. Accessibility and accuracy are currently in tension, and that tension is the core of my research question.

## 6. What Still Needs Work / Who It Might Fail For

This paper is early-stage. The clearest limitation is that I have not yet run a controlled experiment. I have observed and documented errors, and I have audited the pipeline to understand where they come from — but I have not yet measured the errors quantitatively. I cannot currently say, for example, that pitch accuracy drops by X% when polyphony increases from one voice to three. That measurement is the goal of my next Space (an AMT Accuracy Tester), which will let users upload audio alongside a reference MIDI and see a scored comparison across pitch, timing, rhythm, and dynamics.

A second limitation is that I only tested one model (Basic Pitch) on one hardware tier (CPU). I cannot say whether the same failures appear on GPU-based models, or whether they are specific to the lightweight architecture. That comparison is a later phase of this research.

The tool is also likely to perform worst for exactly the musicians who might need it most: those working with complex, polyphonic music — piano with full chord voicings, guitar with harmonics and overtones, or vocal music with heavy vibrato and ornamentation. Current AI lacks embodied cognition — it doesn't understand that a guitarist leans into a note to convey urgency, or releases pressure to create vulnerability. Until models incorporate performer gesture data and stylistic context, they'll remain pitch-accurate but expression-blind. Simple monophonic melodies — a whistled tune, a single flute line — will produce the best results. The musicians who most need help transcribing complex music are the ones the tool will help least.

## 7. Sources to Add or Cite

1. **Bittner et al. — Basic Pitch (Spotify, ICASSP 2022)** — the original paper describing the model powering this Space. Available at `github.com/spotify/basic-pitch`. Essential citation for describing what the model was designed to do and what its known limitations are.

2. **"Machine Learning Techniques in Automatic Music Transcription: A Systematic Survey" (arXiv 2406.15249)** — a 2024 survey of AMT methods that describes why notation-level transcription (producing readable sheet music) is significantly harder than MIDI-level transcription (detecting note events). Directly relevant to the gap this paper describes.

3. **"Music's AI Problem, AI's Music Problem" — Journal of the American Musicological Society (2025)** — discusses how Basic Pitch handles overtones and misidentifies pitches in complex chords. Includes a direct analysis of Basic Pitch on a real musical example. Strong source for section 4.

4. **Gardner et al. — MT3: Multi-Task Multitrack Music Transcription (Google Magenta, 2022)** — describes the GPU-based transformer model that represents the current state of the art in music transcription. Useful for explaining *why* the CPU constraint matters and what a better model looks like architecturally. Available at `arxiv.org/abs/2111.03017`.

5. **An AI Approach to Automatic Natural Music Transcription** — describes the distinction between acoustic modeling (detecting pitches from audio) and score generation (converting raw pitch data into natural-looking notation) — this directly explains why my pipeline fails even when Basic Pitch detects pitches reasonably well.

6. **Automatic Music Transcription: An Overview** — explicitly states that MIDI is not the same as notation — that beat, bar, meter, key, and harmony are absent from MIDI — which is exactly what my pipeline gets wrong when it tries to go from MIDI to sheet music without beat tracking or meter detection.

---

## Revision Notes

**Strongest parts of this draft:**
- Section 4 (What I Tried) — the pipeline audit table is specific, accurate, and grounded in real work. This is the most original part of the paper.
- Section 3 (Why This Matters) — the NYSSMA connection is genuine and gives you a real evaluative standard most students would not have. Keep this and make it more specific if you can.
- The framing around accessibility vs. accuracy as a core tension — this is a real and interesting claim that goes beyond "the model was bad."

**Parts that still need more evidence from you:**
- Section 4 needs a concrete example from your own output — describe one specific moment where you looked at the sheet music viewer and saw something clearly wrong. What did it look like? What should it have looked like?
- Section 5 would be stronger with one sentence about what you personally felt when you saw the output as a trained musician — not just what was wrong technically, but what it would mean for someone trying to use it.
- The dynamics row in the table is thin — did you observe dynamics being missing, or is that inferred from the pipeline audit? Be honest about which is observed and which is inferred.

**Parts that sound too generic and should be rewritten in your own words:**
- The opening of Section 3 ("I have been studying music seriously for several years") — replace with something more specific, like the exact NYSSMA level you have done or a specific sight-singing experience.
- The last paragraph of Section 6 — currently it cites a source about guitarists, but your Space is more relevant to voice or piano. Either find a more relevant source or describe your own observation of who the tool would fail for.
