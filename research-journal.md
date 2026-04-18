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
