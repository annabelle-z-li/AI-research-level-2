# Annabelle — Paper Starter
Use this with the shared `PAPER-TEMPLATE.md`.
Your AI draft should be built from your actual music transcription project evidence, not from a generic "AI and music" summary.

## Main question
How do the architectural and computational constraints of AI music transcription models affect their ability to accurately represent the full range of musical information — including pitch, rhythm, dynamics, and instrumentation — and what are the implications for making these tools accessible and useful in real musical contexts?

## Backup question
What is actually lost when a musician tries to use a lightweight, CPU-based transcription model in a real musical workflow — and is the gap between AI output and usable sheet music a technical problem, a musical problem, or both?

## Best artifact to anchor the paper
Use **music-to-sheet-music** (your HuggingFace Space at `annabelle-li/music-to-sheet-music`) as the main project.

This Space takes any audio recording, runs it through Spotify's Basic Pitch model, and outputs MIDI, MusicXML, and an inline sheet music viewer. The long debugging process you went through — Python version conflicts, TFLite vs ONNX backend errors, Gradio schema bugs — is itself relevant evidence: it shows how much friction exists just to *deploy* a lightweight model, before you even get to evaluating its musical output.

You can mention your NYSSMA background in the "Why This Matters to Me" section to explain why you have a real musical standard to evaluate the output against.

## What to paste into the master prompt
- 1–3 observations about what the transcription output looked like — was it faint, sparse, missing notes, or hard to read as sheet music?
- the specific model you used (Basic Pitch by Spotify, ONNX backend, CPU tier on HuggingFace Spaces)
- one concrete example where the output MIDI or sheet music was clearly wrong — e.g. a chord that came out as a single note, a rhythm that got smeared, or dynamics that disappeared
- one note about how your musical training (NYSSMA sight-singing, opera/jazz background) gives you a different standard than someone who has never read sheet music
- one honest limitation: you only tested one model, on one CPU tier, with a small set of recordings, so you can't generalize to GPU models or other architectures yet

## What kind of draft you should expect
You should be able to get a real rough `PAPER.md` draft from this.
The draft should:
- describe the Space clearly, including what it does and what model powers it
- focus on one specific musical dimension where the transcription visibly failed (pitch, timing, rhythm, or dynamics — pick the one you observed most clearly)
- make one claim about what that failure reveals about the gap between model constraints and real musical utility
- include one honest limitation about scope
- connect your personal musical background to why you noticed the problem in the first place

## What the paper is NOT about
This is not a paper about:
- whether AI can compose music
- whether AI will replace musicians
- a general survey of music AI tools

It IS a paper about one specific question: what does a real musician lose when the only available transcription tool is a small, CPU-constrained model — and what does that tell us about who these tools are actually built for?

## If the AI draft sounds too generic
Push it back toward:
- the exact model name and backend (Basic Pitch, ONNX, Spotify)
- the exact failure you observed in the sheet music viewer
- the specific musical vocabulary you'd use to describe what was wrong (e.g. "the inner voices of the chord were dropped", "the eighth notes were quantized into quarter notes", "pianissimo notes were omitted entirely")
- your real music background and what standard you're holding the output to
- the deployment friction as evidence that accessibility is part of the research question, not separate from it

## Connecting to future Spaces
Your paper starter should leave room to mention that this is the first of six planned Spaces investigating the broader research question. The next Space (AMT Accuracy Tester) will let you generate the quantitative evidence this paper can only gesture at right now.
