# speech-synthsizer
TODO: The specifications of the project have to be reworked into something that is better and more specific so that I can start programming it. In the meantime, I'll make a simpler version of the speech synthesizer that actually produces a result.

A fully parametric speech synthesizer where one can understand what each and every component does to achieve the minimum required to synthesize speech that sounds good.
In short, this is a "fully parametric virtual talkbox" with its own "virtual vocal chords and lungs".
Generally made just using whatever stuff I want and whatever sounds good. It's not a rigorous, formal project.
It's also not a simulation of a mouth, but rather just a collection of generators, filters and parameters each tuned to create good (or good enough) output.
This could technically be remade in some kind of modular synthesizer software like Max, or it could be made into a VST plugin for use in DAWs. The core idea and implementation matter the most.
(In fact, after the Python version, I may make a C++ version once I get there. Maybe even a Web version.)

## General notes about the project

NOTE: The general ideas of the project were laid down through my own, limited knowledge and ideas, as well as conversations with ChatGPT on the topic. The text written down below is written by me though. This is a human project, made with AI help (I'm not a professor of linguistics). If AI just wrote everything, of course I wouldn't even feel like I've achieved a lot.
I mostly watched a lot of linguistics content on YouTube before this project. I was (and somewhat still is) a Vocaloid fan, which was a huge motivator for the project. I also read a bit of Wikipedia. But other than that, I mostly just coined my own little theories about how our mouths produce sound and rolled with it.
ALSO NOTE: The goal of this project is NOT to make an AI TTS. Instead, it's the complete opposite: a TTS which has all its parameters exposed and *labelled*. One can *understand* how the TTS works, from the ground up. But the parameters may be set differently to create different characters or voices, and the parameter settings can be based on statistical data collection from a real human's speech, because setting everything by hand in a way that makes things coherent would be a pain. For that purpose, I'll use Praat and general audio editing software.

NOTE: The parameters open to the user don't have any illegal inputs by default. The user can input any number. This can open up the door to generating "impossible sounds", ao. mathematical extensions of the virtual mouth based on the formulas used. It also makes the whole thing more artistically aligned.
IDEA: This whole thing is like a talkbox. It gets an input (the throat sound and the noise source), modifies it similar to how our mouths modify the sound, but with functions and filters and not a 3d acoustic simulation, and then returns the output. This could be made into a VST plugin that can take arbitrary sound input and have its parameters modified in a time-varying manner. More art!

## General stucture of the synthesizer

### The sources / generators
Generally, the synthesizer works by modifying already existing audio sources/generators with filters.
The two (basic) sound sources:
- "Throat source": A sound that comes out from the nonexistent vocal cords, and which is meant to be shaped by the nonexistent mouth later. It could be called a "featureless vowel".
- Noise source: A noise sound generator (e.g. white noise) that forms the basis of all things non-vocal-chordy (e.g. the voiceless component of consonants, /h/ sounds etc.). It could be called a "featureless voiceless consonant".
The sources are literally repeating waves of some kind.

### Phones and their audio (aka. the reasoning behind the filters)
Every single phone's (linguistic phone's) acoustic realisation is generalized into two audio components: a voiced and a voiceless component
The voiced component's audio is based on a filtered throat source.
The voiceless component's audio is based on a filtered noise source.

Vowels generally consist of only a voiced component. (NOT IMPLEMENTED YET: with some exceptions, like breathy voice)
Voiceless consonants consist only of a voiceless component.
Voiced consonants consist of both a voiceless and a voiced component (like a voiceless consonant × vowel together)

The "lazy mouth": Since the tongue, the lips etc. are lazy and "cannot teleport into given coordinates", they must have a sort of easing effect. This effectively means that the parameters of the mouth cannot snap in place over time, they must have acceleration and deceleration. This kind of system could model the fact that many sounds influence the ones around them in general ways, in ways that are common amongst the words languages (like the sound of a /s/ changing based on roundedness). (TODO: The amount of "lazyness" could be its own parameter, or a set of parameters for different parts of the mouth.) See "coarticulation" and "articulatory intertia" for more academic info.

### The nonexistent mouth and its filtering
The nonexistent mouth has a few parameters which are used in the process of filtering.

#### Vowels
The mouth has the following properties for vowels:
- A tongue with frontness-backness (x axis) and highness-lowness (y axis).
- Roundedness (width and height of lips)
- Nasality (preferably percentage that can go out of bounds, where 0% is normal speech, 100% is fully nasal (all air escapes through the nose))
TODO: I plan on devising or finding simple formulas for the connections between tongue position, roundedness and formants. Using these simple formulas, I could, in theory, generate any vowel (and since functions could be defined outside of the scope of sensibility, "illegal" values could be entered too.) Even if the mapping of parameters to formants is non-linear, maybe approximating it with the appropriate functions could still prove functional.

The filtering is pretty simple. It's like an EQ. The plan is that the sources ***ALREADY CONTAIN EVERY POSSILBE FORMANT THAT COULD BE REQUIRED AT FULL VOLUME***. (The other method is with formants and anti-formants.) With this, the EQ acts as some kind of "subtractive synthesis", only keeping formants where it makes sense to generate the given vowel.

(*If I truly cannot derive any way to have a function return formants based on mouth parameters, I could use a lookup table but that's... not as great, especially for the "lazy mouth" aspect, which requires gliding between vowels.*)

#### Consonants
The mouth has the following properties for consonants:
- Everything that was already present for vowels
TODO: Creating functions modelling how consonant formants and stuff are formed may prove to be too difficult, especially without the necessary knowledge. I will probably, at least first, create a lookup table that tells the program how to shape and filter the noise and throat sources to generate pre-determined consonants. Note that the "lazy mouth" concept applies here too.

#### Syllables
Each syllable and each phone has its own length. This depends on language, and is also a unique identifier of the person who is talking.
This means that each phone's general pronunciation length (maybe also based on phonotactics) has to be configured parametrically.
Coarticulation habits (how a character's "lazy mouth" works) also have to be configured spearately.

TODO: Expand the parameter space.

AS FOR NOW, the project only has very few parameters for these.    ***TODO: CONTINUE WRITING DOWN IDEAS.***

### Prosody

TODO: Let's just leave this for later. It's easier to make this into a singing speech synthesizer first.

### Extras

According to ChatGPT, I left out a few things (which may already be included in the above stuff anyways):
- "Radiation model": Sound changes when exiting the mouth: mild high-fequency boost. (I would guess this could be related to roundedness and generally be part of the above procedure anyways, as a result of me not caring to differentiate between what comes out "inside the mouth" and what truly comes out the mouth. This could be an easier method anyways, since microhpone recordings of my own voice contain the data after the sound came out of my mouth, so collecting emperical data of the fully "filtered" sound is easier.)
- Nasility control. Fair enough, I need that, included above.
- NOT IMPLEMENTED: Breathy, creaky, etc. Yeah, I need those. (At least eventually.)


## Goal, target languages and input

The most basic and most reachable goal is to get this synthesizer to sound like me on a recording, without using any recordings of me directly.
After a bit of thought, the more concrete goal is to create a nice-sounding (even if somewhat robotic, that could be part of the vibe) 

The target languages are Japanese, Hungarian, English, maybe German. Evenhally, I want this to be a general synthesizer though, capable of reading IPA out aloud.

### Inputting language and IPA

The basic input system of the software will be the IPA and the language's writing system (or its romanization). There are multiple input methods still: (more info at ¹[Wikipedia](https://en.wikipedia.org/wiki/International_Phonetic_Alphabet#Brackets_and_transcription_delimiters)
- Broad transcription (uses the generally accepted `/.../` slashes): A general transcription of the utterance, "which note only features that are distinctive in the language"¹. This can only be used if the software knows which language to read the transcription as.
- Narrow transcription (uses the generally accepted `[...]` square brackets): Depends on setting, this could be normal narrow transcription or "especially precise"¹ narrow transcription. If a language is given, it should be treater as the former, if not, then the latter.
- Text (uses `|...|` or nothing sorrounding the text, as seen in link ¹): Writing in the language's orthology or its romanization. Using an NLP package, this could potentially be made into IPA.
