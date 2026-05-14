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
