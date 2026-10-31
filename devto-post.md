---
title: "Ghost Tales: Learn English Through Ghost Stories, Fully Offline with Gemma"
tags: devchallenge, hf26challenge, ai, opensource
published: false
---

*This is a submission for the [Hacktoberfest Open-Source AI Challenge Week 4: Ghost in the Machine](https://dev.to/challenges/hacktoberfest-week4-2026-10-26).*

## What I Built

Ghost Tales (ভূতের কথা in Bengali) is a small web app that teaches English through ghost stories, with Bengali beside every sentence. It runs fully offline on my own computer.

You pick a place, like an old zamindar mansion or a river ghat at dusk. You pick a mood: spooky, or a gentle "memory ghost". You can type a name for the hero. Then a local AI model tells a short story in simple English. After that, it translates the story into Bengali, one sentence at a time. The two languages sit side by side.

The ghost in my machine is the model itself. It lives in my own computer and tells stories by lamplight. The story writing and translation never leave this computer.

I built it for people like me. I live in Bangladesh. Many learners here want to practice English, but good tools often need accounts, data and a connection. A story you enjoy is easier to remember than a list of words. And a tool that is free, private and offline is easier to trust.

[[ADD: screenshot of the app with a finished story]]

## Demo

[[ADD: demo video link]]

The app has three learning helpers:

- **Side by side:** English on one side, Bengali on the other.
- **Learn mode:** the English is blurred. You read the Bengali, guess the English, then click to reveal it.
- **Listen:** the browser reads each sentence aloud with its built-in speech voices. I have not checked whether the voices on my system are local or online.

Here is one real pair from a story it wrote for me (unedited model output):

> I feel her hand on my shoulder when the house grows cold.
> ঘর যখন ঠান্ডা হয়ে যায়, তখন আমি আমার কাঁধে তার হাতের স্পর্শ অনুভব করি।

## Code

https://github.com/AnamulAI/ghost-tales

It is one `index.html` file plus a README (and `index.stable.html`, an earlier plain copy of the page). No framework. No build step. No external fonts or scripts. MIT licensed.

To run it: `ollama pull gemma4:12b`, then `python3 -m http.server 8000` in the folder, and open `http://localhost:8000`.

## The look and the sound

The page is a haunted storybook by lamplight. You light a lantern to enter. Behind the text is a layered night scene: a moon, silhouettes that change with the place you pick (bamboo grove, old mansion, river ghat, railway station, village school), drifting fog and foreground grass. The layers shift a little as you move the mouse. The mood changes the palette: indigo and teal with fireflies for spooky, warm amber with floating petals for a memory ghost. The sentence being read aloud glows like candlelight.

Sound is synthesized in the browser with the Web Audio API. There are no audio files. Each place has a quiet ambient bed, such as crickets, wind, water, a distant train or light rain. Short effects, like a whisper, footsteps, a bell or a heartbeat, are triggered by words in the sentence being read. Sound is off by default and stays off until you press the Sound button. Lighting the lantern or skipping the intro does not turn it on. There is also a volume slider. Motion can be switched off too, and the page respects the reduced-motion setting.

The visuals and sound have only been tried on my own Mac, in one browser.

## How I Built It

**Gemma, locally, through Ollama.** The page calls Ollama on `http://localhost:11434` and uses `gemma4:12b` (a 7.7 GB download). I tested on an Apple M4 Mac with 16 GB of RAM. In my own testing, gemma4:12b writes natural Bengali, which is the main reason I chose it.

**Two steps, not one.** Step one asks for an English story of about 12 short sentences, at level A2 to B1. Step two sends each sentence to the model on its own and asks for the Bengali only. I did this because small models tend to be more accurate on short inputs. A whole-story translation has more room to drift, skip lines or add notes. One sentence per request is also easy to show as it arrives. The page fills in line by line.

**The think:false finding.** Gemma 4 can "think" before it answers. With thinking on, one sentence took about 25 seconds to translate in my first tests. That is far too slow when a story has a dozen sentences. So the app sends `think: false`. If an Ollama version rejects that field with HTTP 400, the app retries without it.

With thinking off, I measured three full stories on the same Mac:

| Story | English | Bengali | Whole story |
|---|---|---|---|
| Bamboo grove, spooky (12 sentences) | 15.4 s | 15.1 s | 30.5 s |
| Old mansion, memory ghost (13 sentences) | 10.3 s | 15.2 s | 25.5 s |
| Railway station, spooky (12 sentences) | 9.3 s | 13.7 s | 23.0 s |

A single sentence took about 1.1 to 1.3 seconds on average (0.74 to 2.04 seconds at the extremes). To be fair: the 25-second figure came from earlier manual testing, not from the same sentences, so it is not a controlled comparison. But the gap is large enough that turning thinking off was an easy call. The raw outputs are in `sample-stories.md` in the repo.

**Local only, on purpose.** The model picker reads your installed models from Ollama and hides any with "cloud" in the name. Some Ollama models run on remote servers. For a privacy-first tool, that would break the promise. The filter keeps every request on the machine.

**Honest limits.**

- Translation mistakes happen. My AI assistant checked the three test stories and found a few clear defects: one Bengali line began with a stray Russian word, one had a garbled phrase, and one had a stray Latin letter inside a word. Most lines looked fine in that check, but a small model's Bengali still needs review by a person. The app says so on the page.
- Bengali speech needs a Bengali voice installed on your computer. If there is none, the app reads English only.
- I have not run a user study. I do not know yet how much it helps people learn. I only know it runs, it is quick, and the stories are fun to read.

**What comes next.** I would like to test it with real learners, add a "check my translation" mode, and try more story levels. Reviewing the Bengali with native speakers is first on the list.

AI tools helped build this project. I used an AI coding assistant for code, tests and drafts.

## Why Does Open Innovation Matter?

A model that runs on your own machine can't be switched off, repriced or turned into a data pipeline. That matters most for learners with limited money or a patchy connection. Open weights also let me inspect, swap and test the model. When I wanted a faster setting, I could just change it and measure.

## The Spirit of It

Ghost stories are how many of us first heard how language can pull you in. It felt right that a ghost story be the thing a machine tells, in two languages, with no one listening but you.

## Prize Categories

- **Best Use of Gemma:** the whole app is built around Gemma (`gemma4:12b`) running locally through Ollama, for both story writing and Bengali translation.
