# Week 6 Research Question

## Observation

After I made an AI music transcription huggingface space, I observed that it was not accurate at all, partly because i was using a lightweight cpu. I also noticed how it was better at pitch than timing. I read an article about it and I learned more. The reason why there are currently no amazing ai transcription models is (1) lack of large openly available datasets for training and evaluation, (2) absence of commonly adopted benchmarks for comparing different methods, (3) limited incentives to attract researchers compared to other fields in AI, and (4) the inherent complexity of music signals. So I went to Claude to help me research this.

## Prompt

I'm a high school student in an applied AI course. I'm interested in AI music transcription and I made a huggingface space that uses AI to transcribe music, but it is not accurate at all because its only a CPU model. I also read this article (https://arxiv.org/html/2603.27528v1) about AI music transcription and I want to include that in a a short research brief I want to write  on this but I don't have a research question yet — I just have the observation. Can you help me sharpen this into a researchable question? Don't give me the answer. Give me three candidate questions at different levels of specificity, and explain what would count as evidence for each one.

## Broad Question

"What are the primary factors limiting the accuracy of current automatic music transcription systems, and how do hardware constraints interact with model architecture choices?"

## Medium Question

"How does model size and compute requirement trade off against transcription accuracy for polyphonic audio, and is there a point of diminishing returns?"

## Narrow Question

"Does a lightweight CPU-compatible model like Basic Pitch produce systematically worse transcription on polyphonic recordings than on monophonic ones, and if so, at what level of polyphony does accuracy degrade most sharply?"

## What I Chose

I liked the narrow question, but I wanted Claude to broaden it a little more. So finally I ended up with this research question

"How do the architectural and computational constraints of AI music transcription models affect their ability to accurately represent the full range of musical information — including pitch, rhythm, dynamics, and instrumentation — and what are the implications for making these tools accessible and useful in real musical contexts?"

Which includes these testable sub-problems:

-Architectural constraints → your current Basic Pitch Space (lightweight CPU model)

-Accuracy across musical dimensions → the AMT Accuracy Tester we just discussed

-Instrumentation → a Space testing how well models handle different instruments (piano vs. guitar vs. voice)

-Accessibility → a Space comparing what a musician can actually do with the output (can they read it? edit it? play from it?)

-Real musical contexts → a Space that takes the transcription and does something musically useful with it, like generating a lead sheet or chord chart

-Compute tradeoff → a Space that lets users compare Basic Pitch vs. a heavier model side by side on the same clip

I want to chase this question because it connects your hands-on work to a bigger question. not just "is Basic Pitch accurate" but "what does accuracy even mean for a musician trying to use these tools," which is a much richer and more original angle for a high school research brief. Success would mean being able to point to concrete examples of where architectural/compute constraints caused specific musical dimensions (pitch, rhythm, dynamics, instrumentation) to break down, being able to say something original about the gap between what these models output and what a real musician would find useful, and finally my brief wouldn't be just summarizing other people's papers, it can cite my own Spaces as primary evidence
