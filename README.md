# Live Urdu → English Captions (One Host Mic, Many Listeners)

This demo shows:
- A Host device (near the loudspeaker) captures mic audio.
- Server performs live Urdu transcription and English translation using Whisper (faster-whisper).
- Listeners join by room code or QR and read captions in real time.
- Glossary controls to preserve religious terms.
- Transcript export.

## Quick start

Server:
```bash
cd server
python -m venv .venv
# Windows: .venv\Scripts\activate
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app:app --host 0.0.0.0 --port 8000
```

Client (static web):
```bash
cd web
python -m http.server 8080
# open http://localhost:8080 in browser
```

Usage:
1) Host:
   - Go to the web app, select "Host", enter a room code (e.g., MOSQUE1), and Start.
   - Allow microphone access.
   - Share the QR code or the room code with others.
   - Optional: configure glossary and track settings before/after starting.
2) Listener:
   - Open the web app, select "Listener", enter the same room code or scan the QR.
   - Toggle Urdu/English tracks as desired.
   - Optional: Download transcript.

Notes:
- CPU-only will work with smaller models; for best quality, run on a GPU (set device="cuda" in `server/app.py`).
- The first run will download a Whisper model; subsequent runs are faster.
- Some environments require `ffmpeg` installed system-wide (Dockerfile includes it).

Deployment:
- Use the included `Dockerfile` to containerize the server.
- Expose port 8000 and ensure listeners can reach it.
- For production at scale, consider LiveKit for media transport and Redis/Firebase for fan-out.

Security/Privacy:
- This MVP does not store audio; transcript is in-memory for the session and downloadable while the room is active.
- Add TLS (reverse proxy, e.g., Nginx/Caddy) in production.
