# Gemini Live Audio Investigation

## Objective
Document the efforts to diagnose and eliminate the distorted audio returned by `models/gemini-2.5-flash-preview-native-audio-dialog` in the A-EYE project.

---

## 1. Initial Problem Statement
- **Symptom:** Model responses played through the iOS client include a high-pitched “radio static” overlay.
- **Environment:** iOS Swift client interacting with a FastAPI backend that brokers the Google Gemini Live API.
- **Expectation:** Clean, intelligible audio similar to the quality observed during early desktop experiments.

---

## 2. Attempts Within The iOS Client

### 2.1 Playback Pipeline Review
- Ensured incoming PCM chunks were enqueued into `AudioPlaybackEngine.enqueue`, replicating the native sample rate and channel metadata supplied by the backend.
- Adjusted buffer handling to schedule only frames actually produced by `AVAudioConverter`, eliminating uninitialised frame scheduling.

### 2.2 AVAudioSession Tuning
- Experimented with customised session categories (e.g. `.spokenAudio`, forced speaker overrides, sub-10 ms IO buffers).
- Outcome: Build errors (unsupported converter APIs) and new glitches without resolving the static. Changes were reverted to the neutral `.playAndRecord` / `.playback` defaults.

### 2.3 Bluetooth / AirPods Behaviour
- Verified route switching and session activation when AirPods connected mid-session.
- Result: Audio still distorted, indicating the issue is not device specific.

---

## 3. Backend Diagnostics

### 3.1 Media Pipeline Comparison
- Compared current FastAPI WebSocket relays with the earlier desktop `WebInterface/main.py` script sourced from the Google quickstart.
- Both send `audio/pcm` payloads encoded as 16-bit, 16 kHz mono, and expect 24 kHz mono responses. No practical difference uncovered.

### 3.2 Raw Audio Capture
- Implemented `debug_audio/model_response_<timestamp>.wav` creation server-side by writing raw chunks before they reach the iOS client.
- WAV files opened in tooling (Audacity) using 16-bit PCM, 24 kHz mono already contain the static overlay.
- Conclusion: The corruption originates upstream, before the audio touches the mobile app.

### 3.3 Additional Checks
- Verified that silence gating, base64 encoding/decoding, and JSON payload structure match the quickstart reference implementation.
- Tested different playback devices (built-in speaker vs AirPods) to rule out local hardware issues.

---

## 4. Differences From Earlier Working Setup
- The original “good” experience used the desktop script (`WebInterface/main.py`) with the same model and voice (`Zephyr`).
- Current backend mirrors that request/response flow but routes audio to iOS; the only notable additions are camera streaming and the silence gate.
- The new WAV captures demonstrate the distortion is present immediately after the Gemini Live API response, suggesting a regression or configuration change on Google’s side.

---

## 5. Final Conclusion & Next Actions
- **Root Cause:** Model responses from Gemini Live arrive already distorted. All client-side pipelines faithfully reproduce the supplied PCM data.
- **Evidence:** Captured WAV files created before iOS playback exhibit the same static when played externally (e.g., Audacity).
- **Recommendation:** Escalate to Google support with the recorded samples, detailing the endpoint, model, voice, and timestamps. Mention that earlier runs (via desktop script) produced clean audio, implying a recent regression.
- Optional mitigation: apply a backend filter (e.g. low-pass or noise reduction) to soften the artifacts while awaiting an upstream fix.

---

### Attachments / References
- `backend/debug_audio/model_response_*.wav` – reproducible evidence of distorted output straight from the API.
- `WebInterface/main.py` – Google quickstart implementation used as a baseline for comparison.
- `backend/app/routers/media.py` – backend forwarding logic with WAV capture instrumentation.

