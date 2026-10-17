# 🎙️ Free AI Voiceover Studio

A **100% free, unlimited** AI voiceover studio that runs entirely in your browser. No backend, no API keys, no sign-ups, no usage limits, no cost — ever.

**🌐 Live demo:** https://obaidasiaone-free-ai-voiceover-studio.static.hf.space

Generate natural-sounding voiceovers in **English, اردو (Urdu), हिन्दी (Hindi), and العربية (Arabic)**, clone your own voice from a short sample, and download studio-quality audio as **WAV or MP3**.

---

## ✨ Features

| Feature | Details |
|---|---|
| 🗣️ 4 Languages | English, Urdu, Hindi, Arabic — each with a native neural voice engine |
| 🎭 18 English voices | US & UK accents, male & female (Kokoro-82M) |
| 🧬 Voice cloning | Record, upload, or link an audio sample → get a reusable voice fingerprint |
| 📝 Two modes | Single voiceover + multi-voice script mode (dialogues, podcasts) |
| 💾 WAV / MP3 export | Choose your format on every download |
| 👁️ Voice previews | Hear any voice before generating |
| 🔒 Private by design | Everything runs locally in your browser — your text and voice never leave your device |
| 💰 $0 forever | No APIs, no keys, no subscriptions, no rate limits |

## 🧠 How it works

All AI models run **in-browser** via ONNX/WebAssembly — the page is a static site with no server:

- **English** → [Kokoro-82M](https://huggingface.co/hexgrad/Kokoro-82M) via `kokoro-js` (18 high-quality voices)
- **اردو / हिन्दी / العربية** → MMS neural TTS via [Transformers.js](https://huggingface.co/docs/transformers.js) (Meta's Massively Multilingual Speech models; the Urdu model was converted to ONNX specially for this project)
- **Voice cloning** → a TDNN x-vector speaker encoder builds a 512-dim voice fingerprint from your sample; [SpeechT5](https://huggingface.co/microsoft/speecht5_tts) then speaks any English text in that voice

Models download once (~90–115 MB per language) and are cached in the browser afterwards.

## 🚀 Use it

### Option 1 — Open the file
Just open `index.html` in any modern browser (Chrome/Edge recommended). That's it.

> **Note:** `index.html` loads the ONNX runtime from a CDN (`cdn.jsdelivr.net`), so you need internet access on first load. All voice AI models also download from Hugging Face on first use per language, then work offline from cache.

### Option 2 — GitHub Pages (free hosting)
1. Fork or push this repo to GitHub.
2. Go to **Settings → Pages → Deploy from a branch**, select `main` and `/ (root)`.
3. Your studio is live at `https://<username>.github.io/free-ai-voiceover-studio/` — free, forever.

### Option 3 — Any static host
Upload `index.html` + `lame.min.js` to Netlify, Vercel, Cloudflare Pages, or Hugging Face Spaces (Static). No build step needed.

## 🧬 Cloning your voice

1. Open the **🧬 My Voices** tab.
2. Add a sample: 🎤 **record** (mic), 📁 **upload** an audio/video file, or 🔗 **paste a direct link** to a media file.
3. Give it a name → **Save voice**.
4. Select it in the voice grid and hit **Generate**.

**Tips for the closest match:**
- Use **15–30 seconds** of clear, close-mic speech — just you talking naturally.
- No background noise, music, or other speakers.
- The studio analyses only the speech-active parts of your sample automatically.
- If it warns you that little clear speech was detected, re-record in a quieter spot.

**Honest limits:** cloned voices speak **English only** and capture the *character* of your voice rather than a perfect replica — that's the ceiling of free in-browser cloning today. Cloned voices are stored **only in your browser** (localStorage). For the most human-sounding output, Kokoro's built-in English voices are the best free option.

## 📁 Project structure

```
free-ai-voiceover-studio/
├── index.html      # The entire app — UI + all logic (single file)
├── README.md       # This file
└── LICENSE         # MIT
```

There is no build step, no `node_modules`, no backend. `index.html` is the app.
The ONNX runtime and MP3 encoder load from CDN (jsDelivr); all voice AI models
download from Hugging Face on first use and are cached by the browser.

## 🛠️ Technical notes

- **ONNX Runtime Web 1.14.0** (via CDN) powers the voice-cloning encoder. The encoder model (`obaidasiaone/xvect-tdnn-voxceleb-onnx`) is a cleaned single-file ONNX export of a SpeechBrain TDNN x-vector model: custom `aten` ops were replaced with standard ONNX ops and the IR version set to 8 for wasm compatibility.
- **Urdu model** (`obaidasiaone/mms-tts-urd-onnx`): Meta's `mms-tts-urd-script_arabic` converted from PyTorch to ONNX with a byte-exact hand-built tokenizer, since no public ONNX Urdu TTS existed.
- **MP3 export** uses a vendored `lamejs` build — encoding happens locally, no server.
- **Privacy:** no analytics, no cookies, no network calls except downloading the AI models from Hugging Face / jsDelivr CDNs.

## 🙏 Credits

- [Kokoro](https://github.com/hexgrad/kokoro) (HexGrad) & [kokoro-js](https://github.com/xenova/kokoro-js) — English voices
- [Meta MMS](https://github.com/facebookresearch/fairseq/tree/main/examples/mms) — Urdu/Hindi/Arabic voices
- [Microsoft SpeechT5](https://github.com/microsoft/SpeechT5) — cloned-voice synthesis
- [SpeechBrain](https://speechbrain.github.io/) — speaker encoder architecture
- [Transformers.js](https://huggingface.co/docs/transformers.js) & [ONNX Runtime Web](https://onnxruntime.ai/) — in-browser AI
- [lamejs](https://github.com/zhuker/lamejs) — MP3 encoding

## 📄 License

MIT — do whatever you want with it. See [LICENSE](LICENSE).
