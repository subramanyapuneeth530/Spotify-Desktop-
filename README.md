# Spotify Desktop Control (FastAPI + PySide6)

A **work-in-progress desktop controller for Spotify**.

This project is split into:
- a **FastAPI backend** that authenticates with Spotify and exposes simple HTTP endpoints, and
- a **PySide6 desktop GUI** that calls those endpoints to control playback, devices, playlists, and queue.

> ⚠️ **Project status:** not final — expected to change and grow. The README is written to stay useful even as features are upgraded.

---

## What this project does

- Controls Spotify playback (play/pause/next/previous/seek/volume)
- Toggles shuffle + repeat
- Lists available Spotify devices and transfers playback to a selected device
- Lists your playlists and lets you play a playlist
- Shows playlist tracks and supports add/remove track operations
- Shows your current queue (note: clearing queue is limited by Spotify API)

---

## How it works (high level)

1. **Backend (`backend/`)** handles Spotify OAuth using **Spotipy** and keeps a token cache.
2. The backend exposes REST endpoints like `/playback/play`, `/devices`, `/playlists`, etc.
3. **GUI (`GUI/`)** uses an `api_client.py` module to call the backend and render controls in a PySide6 window.
4. Spotify actions affect your **currently active Spotify device** (desktop app/web player/mobile).

> If nothing is playing on Spotify, some actions may fail until you start playback on any device once.

---

## Repo structure (current)
```
├── backend/
│ ├── main.py # FastAPI routes (HTTP API)
│ ├── spotify_client.py # Spotify OAuth + Spotipy wrapper
│ ├── .env # (local) Spotify credentials ❌ don't commit
│ └── .spotify_token_cache # (local) OAuth token cache ❌ don't commit
│
└── GUI/
├── player.py # PySide6 desktop UI
└── api_client.py # HTTP client for backend
```

## Requirements

- **Python 3.9+**
- A Spotify account
- A Spotify Developer App (Client ID / Client Secret)

Python packages (installed via pip):
- `fastapi`
- `uvicorn`
- `pydantic`
- `spotipy`
- `python-dotenv`
- `requests`
- `PySide6`

---

## Spotify Developer App setup

1. Go to the Spotify Developer Dashboard and create an app.
2. Copy your:
   - `SPOTIFY_CLIENT_ID`
   - `SPOTIFY_CLIENT_SECRET`
3. In the app settings, add a Redirect URI matching your `.env` value, for example:
   - `http://localhost:8888/callback`

> Redirect URI **must match exactly** or login will fail.

---

## Environment variables

Create a file at:

`backend/.env`

Example:

```env
SPOTIFY_CLIENT_ID=your_client_id_here
SPOTIFY_CLIENT_SECRET=your_client_secret_here
SPOTIFY_REDIRECT_URI=http://localhost:8888/callback


<img width="799" height="749" alt="image" src="https://github.com/user-attachments/assets/94dc5418-9845-4f22-948d-bbadd8add803" />

preview video https://drive.google.com/file/d/1E66S_lMVRq_VChGsQv-voaRVJM5P58-x/view?usp=drive_link 
