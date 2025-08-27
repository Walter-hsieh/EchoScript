## EchoScript — AI Dialogue‑to‑Audio Converter

Turn written two‑person dialogues into lifelike, multi‑speaker audio using Google's Gemini TTS. Paste your script, click Generate, and listen instantly in your browser.

### Features
- **Multi‑speaker voices**: Select distinct voices for speakers 1 and 2 via dropdowns
- **Upload .txt**: Load your script from a `.txt` file directly into the editor
- **One‑file app**: Pure client‑side `dialogue-converter.html` — no backend required
- **Session‑only API key**: Prompts for your key and stores it in `sessionStorage` for the current tab only
- **WAV output**: Converts raw PCM to a playable `.wav` blob in‑browser
- **Retry logic**: Exponential backoff for transient API errors (429/5xx)

### Quick Start
1. Clone or download this repository.
2. Open `dialogue-converter.html` in a modern browser.
3. When prompted, paste your Google Generative Language API key.
4. Optionally click "Upload .txt Script" to load a text file, or paste/edit the sample dialogue.
5. Choose voices for Speaker 1 (Me) and Speaker 2 (Wife) from the dropdowns.
6. Click "Generate Audio".

If your browser blocks `fetch` on `file://` pages, serve locally instead:
- Python 3: `python -m http.server 8080`
- Node: `npx http-server -p 8080`
Then browse to `http://localhost:8080/dialogue-converter.html`.

### Getting an API Key
- Create a key for the Google Generative Language API (Gemini). See the official docs: [Get started with the Gemini API](https://ai.google.dev/)
- The app uses model `gemini-2.5-flash-preview-tts` and the endpoint `...:generateContent` with `responseModalities: ["AUDIO"]` and a `multiSpeakerVoiceConfig`.

### Usage Notes
- **Dialogue format**: Use lines starting with `Me:` and `Wife:`; these labels map to the selected voices.
- **Voice selection**: Pick voices from the two dropdowns. Defaults are Kore (Me) and Callirrhoe (Wife).
- **Supported voices**: Use lowercase voice names. Supported names include: `achernar`, `achird`, `algenib`, `algieba`, `alnilam`, `aoede`, `autonoe`, `callirrhoe`, `charon`, `despina`, `enceladus`, `erinome`, `fenrir`, `gacrux`, `iapetus`, `kore`, `laomedeia`, `leda`, `orus`, `puck`, `pulcherrima`, `rasalgethi`, `sadachbia`, `sadaltager`, `schedar`, `sulafat`, `umbriel`, `vindemiatrix`, `zephyr`, `zubenelgenubi`.
- Output is generated as a WAV blob and played via the built‑in `<audio>` element.

### File Overview
- `dialogue-converter.html`: Single‑page app (UI + logic). Includes Tailwind for styling and all TTS/PCM→WAV utilities.

### Privacy & Security
- Your API key is never hard‑coded or sent anywhere except the Gemini API request from your browser.
- The key is stored only in `sessionStorage` and cleared when the tab is closed.

### Troubleshooting
- **Unsupported voice name**: Ensure you selected a supported, lowercase name (see Supported voices above).
- **429 Too Many Requests**: The app automatically retries with exponential backoff. Try again after a short wait.
- **No audio playback**: Ensure the response contains audio data; check DevTools Console for errors. Verify your API key and model access.
- **CORS or file access issues**: Serve the file via a local web server (see Quick Start) instead of opening via `file://`.
- **Sample rate error**: The app parses the sample rate from the `mimeType`. If parsing fails, ensure your account has access to the specified TTS model.

### Customize Voices
Use the in‑app dropdowns to select voices at runtime. If you want to hard‑set defaults, edit `speakerVoiceConfigs` in `dialogue-converter.html` and use lowercase names, for example:
```js
{ speaker: "Me", voiceConfig: { prebuiltVoiceConfig: { voiceName: "kore" } } }
{ speaker: "Wife", voiceConfig: { prebuiltVoiceConfig: { voiceName: "callirrhoe" } } }
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
