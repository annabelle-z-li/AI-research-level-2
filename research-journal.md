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
