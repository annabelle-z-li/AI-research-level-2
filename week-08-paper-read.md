# Week 08 Paper Read

## Paper Metadata

**Title:** An AI Approach to Automatic Natural Music Transcription
**Authors:** Michael Bereket, Karey Shi
**Year:** 2017
**URL:** https://cs229.stanford.edu/proj2017/final-reports/5244388.pdf

## I. Field orientation (AI-assisted, on the abstract only)

### 1. Field + Sibling Fields
The paper lives in Music Information Retrieval (MIR) — specifically the AMT subproblem. It draws on:

Machine learning / deep learning (CNNs, RNNs, HMMs borrowed from CS229-style ML)
Signal processing / acoustics (time-frequency representations of audio)

### 2. Five Terms to Know

Polyphonic audio — audio where multiple notes sound simultaneously, as opposed to monophonic (one note at a time), which makes pitch detection much harder.
Piano roll — a grid representation of music where the x-axis is time and the y-axis is pitch, showing exactly when each note starts and stops, like a scrolling player-piano scroll.
Acoustic modeling — the step of figuring out which pitches are present in a given slice of audio, before worrying about rhythm or notation.
Hidden Markov Model (HMM) — a probabilistic model that infers a hidden sequence of states (here: the "intended" written rhythms) from noisy observed data (here: the irregular performed rhythms).
Tempo bucketing — grouping note durations into discrete categories (quarter note, eighth note, etc.) relative to an estimated tempo, so messy real-world timing gets snapped to clean notational values.

### 3. Kind of Evidence Being Promised
The authors are presenting a new end-to-end pipeline / method — not a large dataset study or a purely observational analysis. Specifically they're proposing two linked systems built and tested by them: a CNN for acoustic modeling and a two-phase score generation module (tempo selection + HMM smoothing). The evidence will be experimental, evaluating their own pipeline's outputs, likely on classical piano audio.

## II. Methods walkthrough (AI-assisted, on the methods section only)

### Step 1: Represent audio as an image

The authors convert raw piano audio into a CQT (Constant-Q Transform) — a time-frequency grid where the x-axis is time (frames) and the y-axis is pitch frequency, producing something visually similar to a spectrogram. This is what gets fed into the CNN as if it were an image.

### Step 2: Build a context window
Rather than feeding the network one frame of audio at a time, they group several neighboring frames into a "context window" and ask the CNN to predict what notes are active in the center frame. This lets the model use surrounding musical context to make better predictions.

### Step 3: Design the CNN architecture

They stack layers in this order:

- Conv layer 1 — 50 filters, large kernel (5 frames × 25 frequency bins), meant to catch broad harmonic patterns
- Tanh activation — a non-linearity that squashes values into a curve, helping the network learn complex patterns
- MaxPooling (frequency axis only) — compresses the frequency dimension by taking the maximum value in each small region, reducing data size
- Dropout (30%) — randomly zeros out 30% of neurons during training to prevent the model from over-relying on any single feature -(overfitting prevention technique)
- Conv layer 2 — 50 filters, smaller kernel (3×5), refining the features from layer 1, same pooling and dropout
- Two fully connected layers — 1,000 then 200 hidden neurons with sigmoid activations, synthesizing all the learned features
- Output layer — exactly 88 neurons, one per piano key, each outputting a probability that key is "on" during that frame

### Step 4: Initialize weights carefully

They use He normal initialization — a method of setting the CNN's starting random weights so that signals neither explode nor vanish as they pass through deep layers, which helps training converge faster.

### Step 5: Define the loss function

Each of the 88 output neurons is trained independently using binary cross-entropy — a loss function that penalizes the model based on how far its predicted probability is from the true 0/1 label (key off or on). This treats the problem as 88 simultaneous yes/no questions.

### Step 6: Train the network

They use stochastic gradient descent (SGD) with 0.9 momentum — an optimizer that updates the model's weights in small steps, where momentum means it carries some "velocity" from prior updates to avoid getting stuck. They start with a learning rate of 0.01 and iteratively adjust a decay schedule over up to 40 training epochs (passes through the full dataset).

### Step 7: Produce a piano roll

After training, the model runs over every time frame in a song and outputs a probability per key. Aggregating these frame-by-frame predictions across the full audio produces a complete piano roll — which then feeds into their separate score generation module.

## III. Read the results section YOURSELF

### Summary

The authors developed their CNN acoustic model through three iterative phases: fixing underfitting on a small dataset by reducing dropout and scaling up to 138 songs on a GPU, then tuning dropout rate and hidden unit count to reach a ~44% F1 baseline. A training plateau led them to experiment with learning rate decay schedules, where starting at 0.05 and halving every 10 epochs proved most effective on the validation set. A gap between their training F1 (~74%) and validation F1 (~44%) indicated overfitting, which they attribute to limited data diversity across only 6 composers and 138 songs. They also identified a note-fading artifact: the model tends to cut notes short because real piano audio naturally decays in volume before the note ends in the MIDI ground truth. Their proposed remedies include acquiring more MIDI data across a wider range of composers and generating synthetic training data to broaden the model's exposure to varied note patterns.

### Notes

- Dropout is a regularization tool that intentionally makes the network learn less precisely during training to prevent memorization. When you're already underfitting (the model hasn't learned enough), too much dropout makes the problem worse. Lowering it gave the model more capacity to actually pick up patterns.
- F1-score is a single number that combines two metrics — precision and recall — into one balanced measure of a model's accuracy.
F1-score is a single number that combines two metrics — precision and recall — into one balanced measure of a model's accuracy.

Precision = of all the notes the model predicted were on, what fraction were actually on? (Are your positives trustworthy?)

Recall = of all the notes that actually were on, what fraction did the model catch? (Are you missing things?)

## IV. Read the limitations YOURSELF

### Summary

The main limitations of this paper fall into data, modeling, and scope concerns. The dataset is narrow, only 138 songs across 6 composers, and the resulting 30-point gap between training and validation F1 suggests the model overfit to those specific patterns rather than learning to generalize. Structurally, a mismatch between real piano audio (which fades in volume) and MIDI ground truth (which marks full note duration) causes the model to systematically cut notes short, and the entire pipeline is scoped exclusively to classical piano with no accommodation for other instruments or mixed ensembles. Finally, the lack of a direct benchmark against Sigtia et al. makes it difficult to assess how much the authors' approach actually advances the state of the art.

### What claim from this paper would you actually cite in your brief, and what would it support?

Bereket and Shi (2017) is a Stanford CS229 project that builds an end-to-end pipeline for automatic music transcription of classical piano audio, combining a CNN-based acoustic model with an HMM-based score generation module. The authors iteratively tuned their CNN through three phases: fixing underfitting, scaling to a full dataset on a GPU, and experimenting with learning rate decay schedules. This ultimately achieving ~44% F1 on their validation set before overfitting became a ceiling. Their most citable finding for your brief is a structural one: even a reasonably well-performing model systematically truncates note durations because real piano audio fades in volume before the MIDI ground truth ends, a mismatch that no amount of architectural tuning can fully resolve. The paper's main limitations are its narrow 138-song dataset, piano-only scope, and no direct benchmark against prior work. This means its results should be treated as a proof-of-concept rather than a definitive performance claim, but the note-fading observation concretely illustrates how the gap between acoustic signal and symbolic notation is a fundamental constraint on AMT usability, not just an engineering problem.
