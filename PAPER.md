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

I have been studying music seriously for several years, including NYSSMA (New York State School Music Association) evaluations, which involve sight-singing — reading and performing music you have never seen before, in real time, with attention to pitch, rhythm, dynamics, and phrasing. That training gave me a specific standard for what accurate musical notation looks like and what it feels like to read and perform from a score. In sight-singing, you have to look at a piece of notation and immediately translate it into sound — which means you are very aware, at a glance, when a score is unreadable. You know when the rhythm makes no sense and when the pitches fall outside any logical key.

When I looked at the output of my Space, I was not just looking at a file. I was reading it the way I would read any piece of music before performing it. And what I saw was not usable. The rhythms did not make musical sense. Notes that should have been grouped as quarter notes or dotted rhythms were smeared into strange durations. The time signature, which tells a performer how to feel the beat and count the measure, was either absent or wrong. A musician trying to perform from this output would be more confused than helped.

This matters beyond my own experience. One of the promises of AI transcription tools is that they can make music more accessible — that a musician who cannot afford a professional transcriber, or who does not have the theory training to write out music by ear, could use a tool like this instead. If the output is this far from usable, then the tool is not actually delivering on that promise. Understanding *why* it fails, and *which* musical dimensions fail first, is the first step toward knowing what it would actually take to close that gap.

My earlier work in this class also pointed me here. When I was building Music Starter: Opera & Jazz — a text generation Space — I noticed that the model was especially bad at musical specifics. When I asked it to generate chord progressions, it invented chord names that do not exist. That observation made me curious about a different question: not whether AI can write *about* music, but whether it can accurately *represent* music. Transcription felt like the more concrete, more testable version of that question.

## 4. What I Tried

I ran three audio recordings through the Space — a C major scale, *Twinkle Twinkle Little Star*, and the Minuet in G — chosen deliberately to represent increasing levels of musical complexity. The scale is the simplest possible input: monophonic, stepwise, no rhythmic ambiguity. Twinkle is a familiar melody that any transcription tool should handle. The Minuet in G is a real piece of repertoire with a clear phrase structure and recognizable harmonic motion. If the tool fails on all three, that tells us something important about the category of failure.

All three tests were run using Basic Pitch (ONNX backend) on the free CPU tier, with music21 handling MIDI-to-score conversion and LilyPond rendering the final SVG output.

---

**Test 1: C Major Scale**

*Prompt (audio input):* A single-voice ascending C major scale, played cleanly on a melodic instrument.

*Output:* The treble clef and common time signature were assigned correctly — the only test where the time signature was right. The ascending contour of the scale is loosely visible: the output does rise from lower to higher pitches in roughly the right direction. But the rhythm is wrong throughout. The first note was rendered as a half note instead of a quarter note. Dotted rhythms appear mid-scale where there should be uniform quarter notes. A tie appears with no musical justification. The final measure contains what looks like a grace note cluster. The scale is *recognizable* in the output, but it would not be performable from this score without already knowing what it was supposed to sound like.

*What this shows:* Even the simplest possible input — eight notes, all the same duration, no harmony — produces rhythm errors. The pitch contour survives; the rhythmic structure does not. This is consistent with what the pipeline audit predicted: Basic Pitch detects where pitches occur but does not track beat or meter, so music21 has nothing to work from when it assigns durations.

---

**Test 2: Twinkle Twinkle Little Star**

*Prompt (audio input):* A single-voice performance of *Twinkle Twinkle Little Star*, one of the most recognizable melodies in Western music.

*Output:* Three pages of notation — for what should be a 16-bar melody that fits comfortably on one page. Almost every measure contains chord clusters with four or five notes stacked vertically, far below and above the expected pitch range of this melody. Rests appear mid-phrase with no musical logic. The time signature changes partway through the score. Notes drop into ledger line territory that makes no sense for a melody that sits entirely in the middle register.

This is the output that most clearly illustrates the overtone problem. The model is not hallucinating pitches at random — it is detecting the real overtones produced by the instrument and treating each partial as a separate simultaneous note. The result looks like a dense piano reduction of something that was originally a single melodic line. As a musician, looking at this score, there is no way to identify it as *Twinkle Twinkle* without being told. The melody is completely buried.

*What this shows:* When audio contains any resonance or sustain — which almost all real recordings do — Basic Pitch multiplies every note into a chord. The more resonant the instrument, the worse the output. This is not a quantization error or a rhythm error. It is a fundamental misunderstanding of what "a note" means in musical context.

---

**Test 3: Minuet in G**

*Prompt (audio input):* A performance of Bach's Minuet in G, a piece with clear phrase structure, a recognizable melody, and a moderate tempo in 3/4 time.

*Output:* Four pages of notation. The same chord cluster problem from Twinkle Twinkle appears here, but worse — almost every beat has four to six stacked notes. The time signature defaults to common time (4/4), not 3/4, which means the bar lines fall in the wrong places and the rhythmic groupings make no musical sense. There is no bass clef, despite this being a piano piece with a distinct left-hand part. The final system on the last page suddenly becomes nearly empty — just a few sparse notes — suggesting the model lost track of the audio entirely in the final phrase. Nothing about this output is performable or readable as the Minuet in G.

*What this shows:* The failure is not just worse for more complex music — it is categorically different. With the scale, the pitch contour survived even if the rhythm failed. With the Minuet, even the contour is unrecognizable. The interaction between overtone multiplication, wrong meter, and missing bass clef produces output that has no relationship to the input a musician could identify.

---

**Summary table across all three tests:**

| Musical Dimension | C Major Scale | Twinkle Twinkle | Minuet in G |
|---|---|---|---|
| Pitch contour | Roughly correct | Buried in overtone clusters | Unrecognizable |
| Rhythm | Wrong throughout | Wrong throughout | Wrong throughout |
| Time signature | Correct (4/4) | Changes mid-score | Wrong (4/4 instead of 3/4) |
| Clef | Correct | Correct | Missing bass clef |
| Dynamics | None preserved | None preserved | None preserved |
| Readability | Barely | Not at all | Not at all |

The pattern across all three outputs is consistent: rhythm fails in every case, dynamics are never preserved, and overtone multiplication gets worse as the audio gets more complex. The only dimension where the tool shows any success is pitch contour — and even that disappears for polyphonic or harmonically rich audio.

## 5. What I Learned

The most important thing I learned is that there is a fundamental difference between **detecting pitches** and **representing music**. Basic Pitch is genuinely good at detecting where sounds occur in audio. The problem is that music notation is not a record of sounds — it is an abstraction that a human performer uses to reconstruct a musical intention. As one reviewer of a similar AI transcription tool put it, sheet music and tab are not precise documentation of the notes as performed; they are abstractions. If a piece of music is like a city, then a notated score is like a street map. You have to leave out a huge amount of information if you want the map to be readable. Deciding what to include and what to omit is an art, not a science.

Basic Pitch — and tools like it — are good at describing the city. They are not yet capable of drawing the map in a way a musician can navigate.

I also learned that the CPU constraint is not just a performance limitation — it is a filter that excludes the models capable of actually solving this problem. The models that can handle beat tracking, meter detection, and polyphonic transcription simultaneously (like Google Magenta's MT3) require GPU resources that are not available on a free-tier Space. This means that the tools most accessible to students and musicians with limited resources are also, by necessity, the least musically accurate. Accessibility and accuracy are currently in tension, and that tension is the core of my research question.

## 6. What Still Needs Work / Who It Might Fail For

This paper is grounded in three real tests, but those tests have clear limitations. I ran each audio file once, through one model, on one hardware tier. I cannot currently say whether the errors are consistent across repeated runs, whether different recordings of the same melody would produce different results, or whether the overtone problem is worse for some instruments than others. The next step is my planned AMT Accuracy Tester Space, which will let users upload audio alongside a reference MIDI and receive a scored comparison across pitch, timing, rhythm, and dynamics — turning these qualitative observations into quantitative measurements.

A second limitation is that all three of my test inputs were relatively simple Western tonal melodies. I chose them deliberately to set the bar low — if the tool cannot handle a C major scale or *Twinkle Twinkle*, that is a meaningful finding. But it also means I have not tested the tool on the inputs where it might do better (very clean monophonic recordings) or the inputs where it would certainly do worse (vocal music with vibrato, jazz with slides and bends, or music in non-Western tuning systems).

The tool is also likely to perform worst for exactly the musicians who might need it most: those working with complex, polyphonic music where harmony and counterpoint are the whole point. The Minuet in G test showed this clearly — a piece with two independent voices produced output with no readable relationship to either of them. Simple monophonic melodies produce the closest thing to a usable result. The musicians who most need help transcribing complex music are the ones the tool will help least.

## 7. Sources to Add or Cite

1. **Bittner et al. — Basic Pitch (Spotify, ICASSP 2022)** — the original paper describing the model powering this Space. Available at `github.com/spotify/basic-pitch`. Essential citation for describing what the model was designed to do and what its known limitations are.
2. **"Machine Learning Techniques in Automatic Music Transcription: A Systematic Survey" (arXiv 2406.15249)** — a 2024 survey of AMT methods that describes why notation-level transcription (producing readable sheet music) is significantly harder than MIDI-level transcription (detecting note events). Directly relevant to the gap this paper describes.
3. **"Music's AI Problem, AI's Music Problem" — Journal of the American Musicological Society (2025)** — discusses how Basic Pitch handles overtones and misidentifies pitches in complex chords. Includes a direct analysis of Basic Pitch on a real musical example. Strong source for section 4.
4. **Gardner et al. — MT3: Multi-Task Multitrack Music Transcription (Google Magenta, 2022)** — describes the GPU-based transformer model that represents the current state of the art in music transcription. Useful for explaining *why* the CPU constraint matters and what a better model looks like architecturally. Available at `arxiv.org/abs/2111.03017`.
5. **An AI Approach to Automatic Natural Music Transcription** — describes the distinction between acoustic modeling (detecting pitches from audio) and score generation (converting raw pitch data into natural-looking notation) — this directly explains why my pipeline fails even when Basic Pitch detects pitches reasonably well.
6. **Automatic Music Transcription: An Overview** — explicitly states that MIDI is not the same as notation — that beat, bar, meter, key, and harmony are absent from MIDI — which is exactly what my pipeline gets wrong when it tries to go from MIDI to sheet music without beat tracking or meter detection.
