# Alpha (Local Personal AI Assistant)

Alpha is a strict single-user local assistant for Mac/iPhone-style usage.  
Each installation has its own local data. No Alpha-to-Alpha communication is implemented.

## Privacy and Isolation

- Local SQLite storage per installation.
- Backend binds to `127.0.0.1` only.
- Non-local client requests are blocked.
- CORS and trusted hosts are localhost-only.
- Relay/peer code is removed.
- `SINGLE_USER_MODE=true` is enforced at startup.

## Requirements

- macOS
- Python `3.13`
- Optional: OpenAI API key for full AI responses

## Quick Start (Mac)

```bash
cd backend
python3.13 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp -n .env.example .env
```

Edit `backend/.env`:

```env
OPENAI_API_KEY=your_key_here
OPENAI_MODEL=gpt-4.1-mini
ASSISTANT_NAME=Alpha
SINGLE_USER_MODE=true
```

Run backend:

```bash
cd backend
source .venv/bin/activate
uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
```

Open in Safari:
- `http://localhost:8000/app/`

## Verify Health

```bash
curl -s http://127.0.0.1:8000/health
```

Expected:
- `"status": "ok"`
- `"single_user_mode": true`
- `"ai_enabled": true` when `OPENAI_API_KEY` is valid

## Run Continuously on Mac (Optional)

```bash
./scripts/install_launchd.sh
```

Useful commands:

```bash
launchctl list | grep focusmate
launchctl unload ~/Library/LaunchAgents/com.focusmate.backend.plist
launchctl load ~/Library/LaunchAgents/com.focusmate.backend.plist
```

Logs:
- `backend/data/launchd.out.log`
- `backend/data/launchd.err.log`

## Troubleshooting

- `{"detail":"Not Found"}`:
  - Use `http://localhost:8000/app/` (include trailing slash).
  - Restart backend.

- UI loads as plain white text:
  - Hard refresh in Safari: `Cmd + Option + R`.

- `FetchEvent.respondWith ... Returned response is null`:
  - Clear stale service worker in Safari console:
    ```js
    navigator.serviceWorker.getRegistrations().then(r => Promise.all(r.map(x => x.unregister()))).then(() => location.reload())
    ```

- Repeating fallback message / AI offline:
  - Confirm `backend/.env` has `OPENAI_API_KEY=...`.
  - Restart backend after editing `.env`.
  - Check health endpoint and confirm `"ai_enabled": true`.

- `429 insufficient_quota`:
  - API billing/quota issue on OpenAI account.
  - Check billing and limits in OpenAI platform settings.

## Notes for iPhone

This repo is local-first and single-user.  
If an iPhone build is created, it should run as a separate local installation with its own storage.
