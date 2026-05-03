# Scriptura

Handwriting → PDF converter. Free for everyone, no login needed.

Powered by **GLM-OCR** (Hugging Face free inference) via a **Vercel serverless function** that keeps the HF token secret.

## Project structure

```
scriptura/
├── index.html          ← frontend
├── css/style.css       ← styles
├── js/app.js           ← frontend logic
├── api/
│   └── transcribe.js   ← Vercel serverless function (calls HF API)
├── vercel.json         ← routing + cache headers
└── README.md
```

## Deploy to Vercel

### Step 1 — Push to GitHub
Create a new GitHub repo and push this folder to it.

### Step 2 — Connect to Vercel
1. Go to [vercel.com](https://vercel.com) → New Project
2. Import your GitHub repo
3. Leave all build settings as default (it's a static site + functions)
4. Click **Deploy**

### Step 3 — Add your HF token as an env variable
1. In your Vercel project → **Settings** → **Environment Variables**
2. Add:
   - **Name:** `HF_TOKEN`
   - **Value:** your Hugging Face token (get one free at huggingface.co/settings/tokens)
   - **Environments:** Production, Preview, Development
3. Click **Save** → then **Redeploy**

That's it. Users visit your site and convert for free — they never see the token.

## How it works

```
User uploads file
    ↓
Browser renders PDF pages to JPEG (pdf.js)
    ↓
POST /api/transcribe  { imageBase64, mimeType, lang }
    ↓
Vercel fn adds HF_TOKEN → calls HF router (GLM-OCR)
    ↓
Returns transcribed text
    ↓
Browser builds PDF (jsPDF) → user downloads
```

## Model: GLM-OCR
- Built specifically for OCR (not a general VLM)
- Supports: English, Chinese, French, Spanish, Russian, German, Japanese, Korean
- Free on HF inference API (rate limited but usable)
- 8.4M+ downloads, MIT license
