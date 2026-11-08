# StarkVideo: Setup Guide

## Free engine (default): no key, no credit card

The site uses **LTX-Video** by Lightricks, through its free demo server on Hugging Face
(`Lightricks/ltx-video-distilled`). Just type a prompt and click **Make video**.

- Videos are 2–5 seconds, 704×480 (16:9) or 480×704 (9:16), with no audio.
- It usually takes 20–90 seconds, plus a wait if other people are in the queue.
- Free GPU time is limited per visitor. If you see "Free GPU time has run out", wait a while, or
  create a free account at huggingface.co and paste a **Read** token from
  https://huggingface.co/settings/tokens into **Settings → Hugging Face token** for more free time.
- You can add a starting photo to animate it: pick a file, drag it onto the form, or paste it into the prompt box.
- The code is in `generateWithHF()` in `assets/js/script.js`. It uses the official `@gradio/client` library from a CDN.

---

## Optional: Google Veo (paid, best quality)

In **Settings → Video Engine**, choose a Google Veo model and paste a Gemini API key.
The key must be from a project with **billing enabled**; free-tier keys get a 429 "quota" error.

## 1. Get a Gemini API key

1. Go to [Google AI Studio → API keys](https://aistudio.google.com/apikey).
2. Click **Create API key** and copy it (it starts with `AIza...`).
3. **Turn on billing** for the key's Google Cloud project. Veo is a paid model; with a free-tier key, requests fail with a quota (429) error.

## 2. Add the key

1. Open `index.html` in your browser (double-click it or use VS Code Live Server).
2. Click the engine button (gear icon) in the top bar to open **Settings**.
3. Choose a Veo engine, paste your key and click **Save settings**.

The key is stored only in your browser's `localStorage`.

| Model | ID | Notes |
|---|---|---|
| Veo 3.1 | `veo-3.1-generate-preview` | Best quality, with audio |
| Veo 3.1 Fast | `veo-3.1-fast-generate-preview` | Faster and cheaper |
| Veo 3.1 Lite | `veo-3.1-lite-generate-preview` | Cheapest, no 4K |

**Demo Mode** (in Settings) plays sample videos without calling the API, so you can test the UI for free.

## 3. Generate a video

1. Describe the video in English for best results (subject, action, setting, camera and sound).
2. Pick the style, length (4, 6 or 8 s), aspect ratio (16:9 or 9:16) and resolution (720p, 1080p or 4K; 1080p and 4K are 8 s only).
3. You can add a **starting image** to animate a photo (image-to-video).
4. Click **Make video**. It usually takes 1–6 minutes. Click **Cancel** in the preview to stop.
5. Download the MP4. **Google deletes generated videos after 2 days**, so save the ones you want.

## How it works (code)

The flow lives in `assets/js/script.js`, in the `generateWithVeo()` function:

1. `POST https://generativelanguage.googleapis.com/v1beta/models/{model}:predictLongRunning`
   with header `x-goog-api-key` and this body:
   ```json
   {
     "instances": [{ "prompt": "...", "image": { "inlineData": { "mimeType": "image/png", "data": "<base64>" } } }],
     "parameters": { "aspectRatio": "16:9", "resolution": "720p", "durationSeconds": 8 }
   }
   ```
2. Poll `GET /v1beta/{operation.name}` every 10 seconds until `done: true`.
3. Read `response.generateVideoResponse.generatedSamples[0].video.uri` and download it with your API key.

## Troubleshooting

| Error | Fix |
|---|---|
| "That Gemini API key isn't valid" | Re-copy the key from AI Studio into Settings. |
| "Your Gemini key has no Veo quota" (429) | Turn on billing for the project, or wait and retry. |
| "Google didn't find that Veo model" (404) | Pick another Veo model in Settings; preview model names change over time. |
| "Google's safety filter blocked this video" | Reword the prompt. Avoid real people, violence and so on. |
| Nothing happens | Press F12 and check the Console tab for errors. |

## ⚠️ Security note

The browser calls the Gemini API directly, so this setup is fine for **personal or local use**. Before you publish the site publicly, move the API call to a small backend (for example a Cloudflare Worker, Vercel function or Node server) so visitors can't see or use your key. Never commit your key to Git.

Docs: https://ai.google.dev/gemini-api/docs/veo
