# Ghost Tales (ভূতের কথা)

Learn English through ghost stories. A tiny local-first app for the Hacktoberfest 2026 "Ghost in the Machine" challenge.

A local open-weight model (Gemma, through [Ollama](https://ollama.com)) tells a short ghost story in simple English, then translates it into Bengali sentence by sentence. Learners can read both languages side by side, hide the English to test themselves, and listen to each sentence. Story writing and translation run on your own machine, with no accounts. The Listen button uses your browser's built-in speech voices, and I have not checked whether those voices run locally on every system.

## Who it is for

Bengali speakers who want to practice English with stories they actually enjoy, and anyone who would rather keep story writing and translation on their own computer.

## Run it

1. Install Ollama and pull a local open-weight model:

       ollama pull gemma4:12b

2. Serve this folder and open it in a browser:

       python3 -m http.server 8000

   Then visit http://localhost:8000

   (No Python? `npx serve -l 8000` works too.)

## How it works

- `index.html` is the whole app: no framework, no build step, no external fonts or scripts.
- It lists your installed models from the local Ollama API (`/api/tags`) and hides cloud-hosted ones, so generation stays on your machine.
- Step 1 writes the story in English (`/api/generate`, streamed so cards can appear as sentences complete). Step 2 translates each sentence into Bengali with a separate low-temperature request, which keeps small models more accurate than translating a whole story at once.
- "Listen" uses the browser's built-in speech synthesis. Bengali audio appears only if the computer has a Bengali voice installed.

## Design

"Ghost x Machine": a cold night scene lit by one warm candle, with a faint machine grid, monospace labels and cut-corner buttons. Everything is drawn and synthesized in the file itself, with no external fonts, images, audio or scripts.

- A staged intro (moon, fog, lantern, title) that you can skip at any moment, then a centered storybook layout.
- A layered scene with depth: moon and stars, silhouettes that change with the setting (bamboo grove, old mansion, river ghat, railway station, village school), foreground grass, fog, and faint circuit traces. The layers move with the pointer (parallax, rotation limited to about 4 degrees), and a soft "lantern light" follows the pointer on devices with a mouse.
- Scene tiles and a mood switch replace the plain form. The mood changes the whole palette: deep indigo and teal with fireflies for "spooky", warm amber with floating petals for "a memory ghost".
- The English story streams in and each card is typed out like a typewriter, then its Bengali fades in underneath. The last line gets a short breath, a slower pace and an italic candle colour. A **Pace** control (slow, normal, fast; slow is the default) sets the speed, and **Tap to continue** makes each card wait for a click, Space or Right Arrow. Clicking a card that is still typing finishes it at once. With Motion off or reduced motion there is no typing, only plain fades. Translation runs sentence by sentence while the cards appear. If streaming fails, the page falls back to a plain request.
- Typography uses system fonts only: a Didot-style display face for the title, a Palatino-style serif for the English story, and Kohinoor Bangla (or another installed Bengali font) for Bengali. Which fonts you see depends on what your computer has installed.
- The sentence being read aloud glows like candlelight. A cinema mode shows the finished story one large sentence at a time.
- Sound is synthesized with the Web Audio API: an ambient bed for each setting, and short effects triggered by words in the sentence being read. It is **off by default**, stays off until you press the Sound button, has a volume slider, and never starts before a click.
- Voice pickers (English and Bengali) under **Advanced** use the speech voices your browser lists (some browsers also list online voices), with a "Test voice" button for each. The default English voice is ranked by name (Premium, Enhanced, Siri, Natural first), and classic novelty voices are put last. Quality depends on which voices you have installed.
- Respects `prefers-reduced-motion`, and has a visible **Motion on/off** toggle. Particles are capped at 36 (plus 6 fog blobs) and the animation slows to about 30 fps while the model is working.
- `index.stable.html` is an earlier plain version of the page, kept as a fallback.

## Known limits

- Small models can make mistakes in Bengali. Review translations before relying on them for study.
- Not a replacement for a teacher; it is a fun practice companion.

## Demo narration

The narration in the demo video was generated with ElevenLabs text to speech (model: Eleven v4, voice: Wyman - The Storyteller), using audio tags for tone. ElevenLabs is not used inside the app: story writing and translation stay on the computer.

## Built with AI assistance

Developed during the Hacktoberfest 2026 Week 4 challenge window, with AI help for code and writing.

Submission snapshot: tag submission-v1. Any later commits are not part of the submission.

## License

MIT

## Measured on

Apple M4 Mac, 16 GB RAM, `gemma4:12b` (7.7 GB) through local Ollama, `think: false`. Three test stories (see [sample-stories.md](sample-stories.md)):

| Story | English story | Bengali (all sentences) | Whole story |
|---|---|---|---|
| Bamboo grove, spooky (12 sentences) | 15.4 s | 15.1 s | 30.5 s |
| Old mansion, memory ghost (13 sentences) | 10.3 s | 15.2 s | 25.5 s |
| Railway station, spooky (12 sentences) | 9.3 s | 13.7 s | 23.0 s |

Translating one sentence took about 1.1 to 1.3 s on average (0.74 to 2.04 s range). With thinking switched on, one sentence took about 25 s in earlier manual testing.
