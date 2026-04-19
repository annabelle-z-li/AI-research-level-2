# research journal

## spaces

**link:** https://huggingface.co/spaces/tencent/SongGeneration

this song generating space was very cool and worked very well. I generated a song about one of my friend's love lives, and the AI encapsulated the sadness of it very well. I didn't even say that it should be sad, but it figured it out itself! very impressive. 9/10.

**link:** https://huggingface.co/spaces/ThomasSimonini/Huggy

this space where you play with a dog named Huggy was cute and scary at the same time. The whole time it looked like Huggy was having a seizure, and the music and sound effects were a bit uncanny as well. Moreover, I could only throw the frisbee like 5 inches. But overall, it's pretty impressive. I rate it a 7/10.

**link:** https://huggingface.co/spaces/Xenova/doodle-dash

this drawing game was very similar to Google's drawing game, except that its prompts were a lot more varied. however, this plus in variety is exchanged for with accuracy. as many times I've found that the AI guesser seemed to be woefully bad at guessing some of my drawings. I want to be self-assured that this was not my fault whatsoever, so I'm declaring that the AI is the problem. But overall it is a pretty good game. 8/10.

**link:** https://huggingface.co/spaces/ASLP-lab/DiffRhythm

if the SongGeneration space was Beyonce, then this space would be that guy who didn't know the lyrics to "Sorry" by Justin Bieber and became a viral meme for it. it was a lot more tedious than the other song generating space, as it requred for me to put the specific minute, second, and millisecond for each lyric, leading to the whole process taking roughly 20 minutes. more importantly, the results were dissapointing. my lyrics were clearly sad, and the audio sample I inputted was sad as well, but somehow the space interpreted that as a happy techno song? Very dissapointing. 4/10.

## models

**link:** https://huggingface.co/deepseek-ai/DeepSeek-R1

we already know that deepseek is great, but it still surprised me with its capability--on par with chatgpt in my opinion. both models gave almost the same answers to my questions, which is crazy because open-sourced models are supposed to be behind close-sourced ones by a few months. 9.5/10.

**link:** https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2

although this model was a pain in the butthole to use in google colab, it works pretty well. although its not very useful to me right now, ill make sure to remember that i have this tool in case it'll come in handy. 7/10.

## creating spaces

dino facts explorer https://huggingface.co/spaces/annabelle-li/dino-fact-explorers-fa8or

i didn't actually create this space necessariy, i created it through another huggingface space called deepsite vibe coding. i simply prompted it to make a simple game where pressing a button will give the user a random dinosaur fact. while i was playing with the space it created, i was wondering whether the facts it gave was ai generated or not. so, i looked in the files myself and saw that it actually just had 25 dinosaur facts in the file script.js that it would repeat. in fact, the facts were actually a smaller part of the coding compared to the aesthetics of it all. there were hundreds of lines designing the animations, such as bonunce, float and shake all in the file style.css. there was also a lot of detail in the font colors and background designs, which was a nice touch. overall, i learned a lot from this experience!

dictionary https://huggingface.co/spaces/annabelle-li/dictionary

this was a long trial of errors on my part. it was just supposed to be a simple application that i could copy off of the build little worlds space in class, but my dumb ah forgot to put the lines "import gradio as gr" and "import requests". this led to a very long time of troubleshooting, basically trying everything to fix it when i have literally zero python experiemce ;(. eventually i just asked gemini and realized my mistake. this lesson has taught me to look at the source material more.

silly phrase finder https://huggingface.co/spaces/annabelle-li/silly-phrase-finder/tree/main

the space silly phrase finder was just something we did in class, and i had no part in creating it. however recently i saw that it wasn't running for some reason, and the error was very confusing. something about "init". my time with the dictionary space told me that its better if i just ask gemini, so i did. it said to just go to settings and reset the space. i subsequently followed its directions and the space is working again. yippee! this error has taught me that whenever i see an error that says "init" in it, just reset it.

# 🎼 Music Transcription Space

**Overview:** Takes an audio recording (uploaded file or microphone input) and transcribes it into sheet music, producing three outputs: a MIDI file, a MusicXML file, and an inline SVG score viewer.

---

## Tech Stack

| Component | Role |
|-----------|------|
| `basic_pitch` (Spotify) | Audio-to-MIDI inference via pre-bundled ONNX model |
| `music21` | MIDI parsing, quantization, and MusicXML conversion |
| `LilyPond` | External binary for rendering the score as SVG pages |
| `Gradio 4/5` | UI framework (includes monkeypatch for known schema bug) |

---

## Features

- **Audio input:** supports file upload or microphone recording
- **Sheet music viewer:** inline SVG rendered from LilyPond, with graceful fallback if LilyPond is unavailable
- **Downloads:** MIDI and MusicXML files for use in notation software like MuseScore

---

## Limitations

- Best results on **single-instrument audio** (piano, guitar, voice)
- Recordings should be **under 2 minutes** on CPU-tier hardware
- SVG score viewer **requires LilyPond** to be installed on the Space

---

## Pipeline

\```
Audio Input
    ↓
basic_pitch (ONNX) → MIDI
    ↓
music21 → Quantized Score → MusicXML
    ↓
LilyPond → SVG pages → Inline HTML viewer
\```

## Week 4 — Classification vs. Generation

### This Week's Big Idea
classification classifies content, but generation creates it. classification needs labeled data. for generation, you could feed it anything and it can make stuff out of it simply through prediction.

### The Demo
distilgpt was very, very bad at answering the prompts. you could tell that none of the data it was being fed was labeled, because it chose its next word through places where they would find the words in your prompt. so if you gave it a math problem about eggs, it might just ramble about how to cook scrambled eggs from a recipe website.
because classification is a simpler task for ai, i believe that this generation model was a lot less "smart". again, the classification models were fed human-made labeled data, which allowed them to output more accurate answers for human beings, but the generation model we looked at was just spouting algorithmic stuff, so it was kind of confusing and random. moreover, the generation models worked with parameters, temperature, and tokens, which is a whole other realm. 

### How I Explored It

Classifier: https://huggingface.co/spaces/Nuno-Tome/simple_image_classifier

Generator: https://huggingface.co/spaces/CohereLabs/c4ai-command

The inputs I used were a totoro movie poster, which the classifier guessed as a comic, and the generator said was a movie poster for totoro, going into much more specifics. this was basically the same for the two other inputs.

### The Training Data Connection
Classification models use labels created from humans, which takes more time and is hard to scale. However, generation doesn't need humans to label anything, so you can train it on anything, which exists in millions of tokens.

### What I Want to Try Next
i would like to work with more songs, which generation would be better for. classification might be better for medical images.

## Week 5 — Add Controls

### What I Built

I finished a Hugging Face Space called Music Starter: Opera & Jazz. It's a text-generation app, that can help composers, students, and curious people write songs. You type a prompt, for example "write an opening aria for a queen who has just learned her kingdom will fall", and the model writes back.
The Space runs on free CPU. The model is SmolLM2-360M-Instruct. It knows the difference between writing an operatic aria and writing a jazz chorus. That distinction lives in the Music Mode dropdown: five options, each one wiring up a different hidden instruction that shapes how the model responds. Opera modes get elevated, dramatic language. Jazz modes get something looser, bluesier, closer to the ground.
There are sliders for Temperature, Top-p, and Max New Tokens. Temperature controls how wild the model gets. Top-p keeps it from wandering too far into improbable territory. Max New Tokens decides when it stops.
I ran into a  problem partway through. The app was loading fine but generating nothing.That turned out to be a misunderstanding of how instruct models work: they're not raw text predictors, they're trained on a specific conversation format, and if you don't apply the chat template, they don't recognise your input as a prompt at all. The fix was switching from the pipeline to direct model.generate() calls and running everything through tokenizer.apply_chat_template() first. Once that clicked, it worked immediately.
There are also five example prompts: A queen watching her kingdom fall. A city at 5am. A love that keeps coming back. A late-night ballad in F major. I wanted something that could hand a songwriter the beginning of an idea.

(this was generated by claude because i didnt feel like writing all the details)

### What I Tried
on the mode opera: chord progression and the prompt "Create a chord progression for a cool, late-night jazz ballad in F major", when i increased the temperature to the highest level, the output was very bad. it was talking about chords that do not exist. so i guess this shows that high temp is not good.

### What Surprised Me
in the mode opera: aria lyrics i upped the temperature all the way with the prompt "Write an opening aria for a queen who has just learned her kingdom will fall." and i actually thought that the higher temperature output was better than the average temperature one. i guess this shows that high temps are good for lyrics but not chords.

### What I Want to Try Next
what i want to try next is to customize it more, because right now its kind of boring with its black background and orange buttons.

## Week 6 — Finding My Research Question

### Where I Started
I was interested in switching from just musical text generation that will only help musicians in the beginning of their journey in composing a song, to creating transcribing tools that can help any musician in the beginning of even well into their musical career. I also had noticed in my space work that the models that I was working with were not that good with musical terms or just music things in general. For example, when I tried to generate chord progression ideas, it just spouted a bunch of random chords that would probably sound terrible if played in real life.

### What Happened in Class
In class, we talked about developing a research question. While I was listening, I decided that simply researching about music with ai text generation was a bit mundane. So, I decided to switch to transcribing.

### My Research Question (Current Version)
How do the architectural and computational constraints of AI music transcription models affect their ability to accurately represent the full range of musical information — including pitch, rhythm, dynamics, and instrumentation — and what are the implications for making these tools accessible and useful in real musical contexts?

### What I Want to Build Next
I already saw how the models I've been using to help with music transcription are actually really bad at music. There are several reasons for this: (1) lack of large openly available datasets for training and evaluation, (2) absence of commonly adopted benchmarks for comparing different methods, (3) limited incentives to attract researchers compared to other fields in AI, and (4) the inherent complexity of music signals. But I believe that one of the biggest things holding me back is my lightweight cpu, which is partly why the output for my first space was so terrible. So, for my research I want to look at these constraints and try to use them to test their musical information, and also think about what would a real musician need? 

For Space 2 I want to build an AMT Accuracy Tester Space — one that makes the experiment in your research question runnable by anyone, not just me. Concretely it would:

-Let the user upload a test audio clip and a reference MIDI file (the known ground truth)

-Run Basic Pitch on the audio automatically and compare the output MIDI against the reference MIDI across your four dimensions:

1. Pitch: % of correct pitches detected

2. Timing: average onset error in milliseconds

3. Rhythm: whether note durations match the reference

4. Dynamics: whether soft notes were dropped

Finally, it would display a simple scorecard showing which dimensions passed and which failed. This goes more into the specifics in which musical aspects AI transcription models usually fail in, effectively allowing me to explore my research question more deeply.

# Research Entry: Music Transcription Space — Transcription Quality Issues

**Date:** 2026-04-18
**Space:** Music to Sheet Music (basic_pitch + music21 + LilyPond)
**Status:** Known limitation — not a bug, but a structural accuracy ceiling

---

## Problem Summary

The transcription output is broadly inaccurate across multiple dimensions simultaneously:

- Pitches are wrong
- Clef assignment is wrong
- Rhythms are wrong
- Time signature is wrong or missing

This is not a single failure but a cascade of compounding errors across the full pipeline.

---

## Pipeline Audit

### Stage 1: `basic_pitch` (audio → MIDI)

**What it does:** Uses a neural network (ONNX model from Spotify's ICASSP 2022 paper) to detect pitches from raw audio and output a MIDI file.

**Known limitations:**

- Designed primarily for **monophonic or lightly polyphonic** single-instrument audio. Complex recordings (chords, harmonics, reverb, multiple voices) cause false positives and missed notes.
- **Pitch detection accuracy degrades** on:
  - Vocals with heavy vibrato or ornamentation
  - Instruments with strong overtones (e.g., piano, guitar) — overtones can be misread as pitches
  - Audio with background noise or reverb
  - Very high or very low registers near the model's trained range
- The model outputs **continuous pitch probabilities**, not discrete note events. The conversion to MIDI note-on/note-off events involves a thresholding step that can create spurious notes or drop real ones.
- `basic_pitch` was trained on relatively clean studio recordings. Microphone input or recordings with room noise will perform worse.

**Likely cause of pitch errors:** The model is picking up overtones, noise artifacts, or bleed from other frequencies as separate pitches.

---

### Stage 2: `music21` quantization (MIDI → score)

**What it does:** Parses the MIDI file, quantizes note durations to a rhythmic grid, and converts to a music21 `Score` object.

```python
score = music21.converter.parse(midi_path)
score = score.quantize(quarterLengthDivisors=(4,))
```

**Known limitations:**

- `quarterLengthDivisors=(4,)` means the smallest rhythmic unit is a **sixteenth note**. Any rhythmic content finer than that (e.g., triplets, grace notes, syncopation) gets snapped to the nearest sixteenth — often creating incorrect rhythms.
- MIDI has no concept of meter or time signature. `music21` infers these from the MIDI tempo track, which `basic_pitch` may not write accurately or at all.
- If `basic_pitch` outputs MIDI without a reliable tempo/meter track, `music21` will fall back to a default time signature (often 4/4) regardless of the actual meter of the source audio.
- The quantization step has no knowledge of musical context — it cannot distinguish between an intentional dotted rhythm and a slightly off-beat note that should be straight.
- **Clef assignment** is automatic based on pitch range of detected notes. If `basic_pitch` outputs phantom high or low pitches, `music21` may assign the wrong clef.

**Likely cause of rhythm and time signature errors:** The pipeline has no beat tracking or tempo analysis step. It goes directly from audio → MIDI → quantization with no intermediate meter detection.

---

### Stage 3: LilyPond rendering (score → SVG)

**What it does:** Takes the music21 score, writes a `.ly` file, and renders it to SVG.

**Known limitations:**

- LilyPond renders faithfully whatever it receives — it does not correct or interpret. If the score object from music21 is wrong, the SVG will be wrong.
- No errors at this stage contribute to the transcription inaccuracy itself.

---

## Root Cause Summary

| Error Type | Root Cause | Stage |
|------------|-----------|-------|
| Wrong pitches | Overtone/noise misidentification | `basic_pitch` |
| Wrong clef | Phantom pitches skew range | `music21` |
| Wrong rhythms | No beat tracking; coarse quantization | `basic_pitch` + `music21` |
| Wrong time signature | No meter detection in pipeline | `music21` |

---

## What Is Missing From the Pipeline

The current pipeline skips two critical music information retrieval (MIR) steps:

1. **Beat tracking / tempo analysis** — needed before quantization so the rhythmic grid aligns to actual beats in the audio
2. **Meter detection** — needed to correctly assign time signatures

Libraries that could fill these gaps:

| Library | Purpose |
|---------|---------|
| `librosa` | Beat tracking (`librosa.beat.beat_track`), onset detection, tempo estimation |
| `madmom` | High-accuracy beat tracking, downbeat detection, key/meter detection |
| `pretty_midi` | Better MIDI manipulation between basic_pitch output and music21 input |

---

## Potential Fixes (Ranked by Complexity)

### Fix 1 — Improve audio input quality (low effort, partial fix)
- Add guidance in the UI for users to record clean, dry, single-instrument audio
- Recommend headphone use when recording via microphone to reduce bleed
- Add a loudness normalization step before passing audio to `basic_pitch`

### Fix 2 — Add `librosa` beat tracking before quantization (medium effort)
- Run `librosa.beat.beat_track()` on the original audio to get a BPM estimate
- Write that BPM into the MIDI tempo track before passing to `music21`
- Use the detected tempo to set a smarter quantization grid

```python
import librosa
tempo, beat_frames = librosa.beat.beat_track(y=y, sr=sr)
# Write tempo into MIDI, then pass to music21
```

### Fix 3 — Add onset detection to clean up `basic_pitch` output (medium effort)
- Use `librosa.onset.onset_detect()` to get note onset times
- Filter or align `basic_pitch` note events to those onsets
- Reduces spurious notes caused by overtone detection

### Fix 4 — Replace `basic_pitch` with a higher-accuracy AMT model (high effort)
- `MT3` (Magenta) — multi-instrument transcription, significantly more accurate but requires GPU
- `piano_transcription` (Kong et al.) — state-of-the-art for piano specifically, CPU-compatible
- These are larger models and may not fit in a free-tier Hugging Face CPU Space

### Fix 5 — Add a post-processing correction layer (high effort)
- After generating the score, prompt an LLM with the MusicXML and ask it to detect and correct obvious errors
- Experimental but potentially powerful for a Space already in the HuggingFace/AI ecosystem

---

## Notes for Future Iterations

- `basic_pitch` was never designed to be a full automatic music transcription (AMT) system — it is a pitch detection tool. The current Space is asking it to do more than it was built for.
- The free-tier CPU constraint rules out the most accurate models, which are GPU-dependent.
- A realistic quality ceiling on CPU with current open-source tools is: accurate pitch detection on clean, monophonic, sustained-note audio (e.g., a simple vocal melody, a whistled tune, a single flute line). Anything more complex will require GPU resources or a commercial API.

---

## References

- Bitteur et al., *basic_pitch* — https://github.com/spotify/basic-pitch
- Cuthbert & Ariza, *music21* — https://web.mit.edu/music21/
- McFee et al., *librosa* — https://librosa.org
- Kong et al., *High-resolution Piano Transcription with Pedals by Regressing Onset and Offset Times* (2021)
