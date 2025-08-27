## EchoScript — AI Dialogue‑to‑Audio Converter

Turn written two‑person dialogues into lifelike, multi‑speaker audio using Google's Gemini TTS. Paste your script, click Generate, and listen instantly in your browser.

### Features
- **Multi‑speaker voices**: Distinct preset voices for speakers "Me" and "Wife" via Gemini multi‑speaker TTS
- **One‑file app**: Pure client‑side `dialogue-converter.html` — no backend required
- **Session‑only API key**: Prompts for your key and stores it in `sessionStorage` for the current tab only
- **WAV output**: Converts raw PCM to a playable `.wav` blob in‑browser
- **Retry logic**: Exponential backoff for transient API errors (429/5xx)

### Quick Start
1. Clone or download this repository.
2. Open `dialogue-converter.html` in a modern browser.
3. When prompted, paste your Google Generative Language API key.
4. Paste or edit the sample dialogue and click "Generate Audio".

If your browser blocks `fetch` on `file://` pages, serve locally instead:
- Python 3: `python -m http.server 8080`
- Node: `npx http-server -p 8080`
Then browse to `http://localhost:8080/dialogue-converter.html`.

### Getting an API Key
- Create a key for the Google Generative Language API (Gemini). See the official docs: [Get started with the Gemini API](https://ai.google.dev/)
- The app uses model `gemini-2.5-flash-preview-tts` and the endpoint `...:generateContent` with `responseModalities: ["AUDIO"]` and a `multiSpeakerVoiceConfig`.

### Usage Notes
- Use the format `Me: ...` and `Wife: ...` in the script; these labels map to the configured voices.
- Voices are currently set to `Kore` (Me) and `Callirrhoe` (Wife). You can change them in the script section inside `dialogue-converter.html`.
- Output is generated as a WAV blob and played via the built‑in `<audio>` element.

### File Overview
- `dialogue-converter.html`: Single‑page app (UI + logic). Includes Tailwind for styling and all TTS/PCM→WAV utilities.

### Privacy & Security
- Your API key is never hard‑coded or sent anywhere except the Gemini API request from your browser.
- The key is stored only in `sessionStorage` and cleared when the tab is closed.

### Troubleshooting
- **429 Too Many Requests**: The app automatically retries with exponential backoff. Try again after a short wait.
- **No audio playback**: Ensure the response contains audio data; check DevTools Console for errors. Verify your API key and model access.
- **CORS or file access issues**: Serve the file via a local web server (see Quick Start) instead of opening via `file://`.
- **Sample rate error**: The app parses the sample rate from the `mimeType`. If parsing fails, ensure your account has access to the specified TTS model.

### Customize Voices
Inside `dialogue-converter.html`, adjust the voice names in `speakerVoiceConfigs`:
```js
{ speaker: "Me", voiceConfig: { prebuiltVoiceConfig: { voiceName: "Kore" } } }
{ speaker: "Wife", voiceConfig: { prebuiltVoiceConfig: { voiceName: "Callirrhoe" } } }
```
Refer to Gemini TTS voice options in the official docs.

### Deploy to GitHub Pages
- Commit and push to GitHub
- Enable GitHub Pages (Settings → Pages → Source: `main`/`docs` as appropriate)
- Access `dialogue-converter.html` from your Pages URL

### Tech Stack
- **Frontend**: Vanilla HTML/CSS/JS with Tailwind CDN
- **TTS**: Google Gemini multi‑speaker TTS (`gemini-2.5-flash-preview-tts`)
- **Audio**: In‑browser PCM→WAV conversion with `DataView`

### License
MIT

### Credits
Inspired by Google Gemini API capabilities for text‑to‑speech and multi‑speaker rendering.
