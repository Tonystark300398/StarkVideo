# StarkVideo

**Turn a sentence into a video.** StarkVideo is a small web app that makes short AI video clips from a text prompt, right in your browser. It's free, needs no sign-up, and has no build step.

- **Free engine:** [LTX-Video](https://huggingface.co/spaces/Lightricks/ltx-video-distilled) by Lightricks, running on Hugging Face's free GPUs
- **Optional paid engine:** Google Veo 3.1 through the Gemini API (better quality, with sound, up to 4K)

---

## Features

- **Text to video:** describe a scene and get a 2–5 second clip, usually in under a minute.
- **Photo to video:** start from a photo. Pick a file, drag one onto the form, or paste it from the clipboard.
- **Prompt helpers:** sample prompts, plus one-click detail chips (slow pan, drone shot, golden hour…) that add or remove phrases. You can undo a prompt you replaced by mistake, and your draft is saved automatically.
- **Live viewfinder:** shows your place in the queue, a progress bar, an elapsed-time clock, and a Cancel button.
- **Results:** play, download, share, or click **Edit prompt** to reuse a prompt. The clips you made this visit are listed in a history strip.
- **Shape, length and look:** wide (16:9) or tall (9:16), clip length, and six styles (realistic, cinematic, anime, 3D, painterly, cartoon).
- **Light and dark themes:** follows your system setting, or pick one with the toggle in the header.
- **Accessible:** keyboard shortcuts (<kbd>Ctrl</kbd> / <kbd>⌘</kbd> + <kbd>Enter</kbd> to generate), focus trapping in the settings dialog, screen-reader status updates, and support for reduced motion and Windows high-contrast mode.
- **Demo mode:** plays sample clips without calling any engine, so you can try the UI for free.

## Quick start

There's nothing to install. It's plain HTML, CSS and JavaScript.

1. Clone the repo:
   ```bash
   git clone https://github.com/Tonystark300398/My-first-website-demo.git
   cd My-first-website-demo
   ```
2. Open `index.html` in a browser. A local server works best, for example the VS Code **Live Server** extension, or:
   ```bash
   npx serve .
   ```
3. Type a prompt and click **Make video**.

## Engines

| Engine | Cost | Length | Sound | Max quality | Needs |
|---|---|---|---|---|---|
| LTX-Video (default) | Free | 2–5 s | No | 704×480 | Nothing (optional free Hugging Face token) |
| Google Veo 3.1 Lite | Paid | 4–8 s | Yes | 1080p | Gemini API key with billing |
| Google Veo 3.1 Fast | Paid | 4–8 s | Yes | 4K | Gemini API key with billing |
| Google Veo 3.1 | Paid | 4–8 s | Yes | 4K | Gemini API key with billing |

To switch engines or add keys, click the engine button (the gear icon) in the header.
See **[API_SETUP.md](docs/API_SETUP.md)** for step-by-step setup, the API flow, and troubleshooting.

## Project structure

```
.
├── index.html     # Page markup: studio, viewfinder, help sections, settings dialog
├── assets/
│   ├── css/
│   │   └── style.css       # Design tokens, layout and components
│   └── js/
│       └── script.js       # Engines, viewfinder, history, settings and prompt helpers
├── docs/
│   └── API_SETUP.md        # Engine setup and troubleshooting guide
├── .vscode/
│   └── launch.json         # Local preview configuration
├── README.md
└── .gitignore
```

`assets/js/script.js` is a single dependency-free script. The only external library, [`@gradio/client`](https://www.npmjs.com/package/@gradio/client), is loaded from a CDN, and only when the free engine is used.

## Privacy and security

- Prompts and photos are sent only to the engine you choose (Hugging Face or Google).
- Tokens, keys, your theme choice and your prompt draft are stored only in your browser's `localStorage`.
- With Google Veo, the browser calls the Gemini API directly. That's fine for personal use, but **before you publish the site with your own key, move the Veo call to a small backend** so visitors can't see the key. Never commit keys to Git.

## Credits

- Video models: [LTX-Video](https://github.com/Lightricks/LTX-Video) by Lightricks · [Google Veo](https://ai.google.dev/gemini-api/docs/veo)
- Fonts: [Bricolage Grotesque](https://fonts.google.com/specimen/Bricolage+Grotesque) and [Instrument Sans](https://fonts.google.com/specimen/Instrument+Sans)
