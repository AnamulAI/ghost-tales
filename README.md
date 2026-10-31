# Bhooter Kotha (ভূতের কথা)

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

## Known limits

- Small models can make mistakes in Bengali. Review translations before relying on them for study.
- Not a replacement for a teacher; it is a fun practice companion.

## Built with AI assistance

Developed during the Hacktoberfest 2026 Week 4 challenge window, with AI help for code and writing.

## License

MIT
