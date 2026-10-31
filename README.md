# Ghost Tales (ভূতের কথা)

Learn English through ghost stories. A tiny offline app for the Hacktoberfest 2026 "Ghost in the Machine" challenge.

A local open-weight model (Gemma, through [Ollama](https://ollama.com)) tells a short ghost story in simple English, then translates it into Bengali sentence by sentence. Learners can read both languages side by side, hide the English to test themselves, and listen to each sentence. Everything runs on your own machine, with no accounts and no data leaving the computer.

## Who it is for

Bengali speakers who want to practice English with stories they actually enjoy, and anyone who would rather keep a language-learning tool private and offline.

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
- Step 1 writes the story in English (`/api/generate`). Step 2 translates each sentence into Bengali with a separate low-temperature request, which keeps small models more accurate than translating a whole story at once.
- "Listen" uses the browser's built-in speech synthesis. Bengali audio appears only if the computer has a Bengali voice installed.

## Design

The page is a haunted storybook by lamplight. All of it is drawn and synthesized in the file itself, with no external fonts, images, audio or scripts.

- A layered scene with depth: moon and stars, silhouettes that change with the setting (bamboo grove, old mansion, river ghat, railway station, village school), foreground grass, and drifting fog. The layers move with the pointer (parallax, rotation limited to about 4 degrees).
- The mood changes the whole palette: deep indigo and teal with fireflies for "spooky", warm amber with floating marigold petals for "a memory ghost".
- Story cards rise in one after another, tilt slightly under the pointer, and the sentence being read aloud glows like candlelight.
- Sound is synthesized with the Web Audio API: an ambient bed for each setting, and short effects triggered by words in the sentence being read (wind, whispers, footsteps, a bell, a heartbeat and so on). It is **off by default**, has a volume slider, and never starts before a click.
- Respects `prefers-reduced-motion`, and has a visible **Motion on/off** toggle. Particles are capped at 36 (plus 6 fog blobs) and the animation slows to about 30 fps while the model is working.
- `index.stable.html` is the earlier plain version of the page, kept as a fallback.

## Known limits

- Small models can make mistakes in Bengali. Review translations before relying on them for study.
- Not a replacement for a teacher; it is a fun practice companion.

## Built with AI assistance

Developed during the Hacktoberfest 2026 Week 4 challenge window, with AI help for code and writing.

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
