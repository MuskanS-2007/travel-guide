# AI Travel Guide

An AI-powered travel companion. Pick a famous Indian landmark, choose a summary or detailed guide, a language and a voice, and the app generates a written guide plus a spoken audio version.

## How it works

1. The frontend (`index.html` + `index.js`) sends the place, detail level, language, voice and locale to the backend.
2. The backend asks **Google Gemini** to write a tourist-guide style description in the chosen language.
3. The text is sent to **Murf AI** to be converted into speech (MP3).
4. The backend returns the text and the audio (base64) and the frontend plays it.

## Features

- Six landmark cards: Taj Mahal, Red Fort, Gateway of India, Hawa Mahal, Golden Temple, Mysore Palace
- Summarized (~1 min) or Detailed (~3 min) guides
- Languages: English, Hindi, Tamil, Telugu
- Male or female voice
- Readable transcript alongside the audio player

## Tech stack

- **Frontend:** HTML, Tailwind CSS (CDN), vanilla JavaScript
- **Backend:** Python, Flask, Flask-CORS
- **AI:** Google Gemini (`google-genai`)
- **Text to speech:** Murf AI API

## Project structure

```
AI_TRAVEL GUIDE_FINAL_CODE/
  Backend/
    app.py              Flask API
    requirements.txt    Python dependencies
    .gitignore          keeps .env out of Git
    .env                your API keys (never commit this)
  Frontend/
    index.html
    index.js
```

## Setup and run locally

### 1. Install dependencies

```bash
cd Backend
pip install -r requirements.txt
```

### 2. Add your API keys

Create a file named `.env` inside `Backend/`:

```
MURF_API_KEY=your_murf_api_key
GEMINI_API_KEY=your_gemini_api_key
```

No quotes and no spaces around `=`. Never commit this file.

### 3. Start the backend

```bash
python app.py
```

It runs at `http://127.0.0.1:5000`.

### 4. Start the frontend

In a second terminal:

```bash
cd Frontend
python -m http.server 8000
```

Open `http://localhost:8000` in your browser.

## API

### `POST /generate-audio-guide`

Request body (JSON):

| Field        | Example                | Description                    |
|--------------|------------------------|--------------------------------|
| `place`      | `"Taj Mahal"`          | Landmark name                  |
| `answerType` | `"Summary"`/`"Detailed"` | Length of the guide          |
| `language`   | `"English"`            | English, Hindi, Tamil, Telugu  |
| `voiceId`    | `"Matthew"`            | Murf voice name                |
| `locale`     | `"en-US"`              | Murf locale code               |

Response (JSON):

```json
{
  "description": "Generated guide text...",
  "audioBase64": "base64-encoded MP3"
}
```

Errors return JSON like `{"error": "Gemini failed: ..."}` with status 400 or 502.

## Deployment

- **Backend:** deployed on [Render](https://render.com) as a Python web service.
  - Build command: `pip install -r requirements.txt`
  - Start command: `gunicorn app:app --timeout 120`
  - Environment variables: `MURF_API_KEY`, `GEMINI_API_KEY`
- **Frontend:** deployed on [Netlify](https://netlify.com). In `index.js`, set `GENERATE_AUDIO_GUIDE_API_URL` to your Render URL followed by `/generate-audio-guide`.

Note: on Render's free plan the server sleeps when idle, so the first request after a break can take about a minute.

## Security

- API keys live in `.env` locally and in Render's environment variables when deployed.
- `.env` is listed in `.gitignore` and must never be pushed to GitHub.
- If a key is ever exposed, revoke it in the provider dashboard and create a new one.

## Live demo

- Frontend: _add your Netlify link here_
- Backend: _add your Render link here_