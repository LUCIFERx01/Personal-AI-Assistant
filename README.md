# Alpha: Local-First AI Personal Assistant

Alpha is a local-first AI assistant designed for explanation, coaching, and daily task support.
It combines chat, voice input/output, and image understanding in a single app experience.

## Why This Project Matters

- Demonstrates practical AI product design for real users (education + ADHD support).
- Implements security-first local architecture (single-user isolation by default).
- Shows full-stack delivery across backend APIs, frontend UX, and deployment scripts.

## Role-Relevant Skills Demonstrated

| Role | Evidence in this repo |
| --- | --- |
| Business Analyst | User-problem framing, feature prioritization, clear workflows, operational docs |
| Data Scientist | Structured data capture (`memory`, `tasks`, `checkins`) for future analytics |
| AI Engineer | Prompt design, multimodal flows (text + image), robust API fallback/error handling |
| Product/Platform Engineer | FastAPI backend, local persistence, launchd runtime, PWA-style frontend |

## Core Features

- Chat assistant with contextual memory.
- "Explain this simply" text workflow.
- Image explanation from upload/camera.
- Voice input + optional voice replies.
- ADHD-friendly check-in coaching.
- Local SQLite persistence.
- Strict single-user mode and local-only access controls.

## Security and Privacy Model

- Runs on `127.0.0.1` only.
- Rejects non-local client requests.
- CORS/trusted hosts restricted to localhost.
- No user-to-user or Alpha-to-Alpha communication layer in this build.
- `SINGLE_USER_MODE=true` enforced at startup.

See detailed security notes in [docs/SECURITY.md](docs/SECURITY.md).

## Tech Stack

- Backend: FastAPI, Pydantic, SQLite
- AI: OpenAI Responses API
- Frontend: HTML/CSS/Vanilla JS (mobile-friendly, chat-style UI)
- Runtime: macOS launchd script for always-on mode

## Quick Start (Mac)

```bash
cd backend
python3.13 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp -n .env.example .env
```

Update `backend/.env`:

```env
OPENAI_API_KEY=your_key_here
OPENAI_MODEL=gpt-4.1-mini
ASSISTANT_NAME=Alpha
SINGLE_USER_MODE=true
```

Run:

```bash
cd backend
source .venv/bin/activate
uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
```

Open in browser:

- `http://localhost:8000/app/`

## Health Check

```bash
curl -s http://127.0.0.1:8000/health
```

Expected fields:

- `"status": "ok"`
- `"single_user_mode": true`
- `"ai_enabled": true` (when API key is valid)

## Run Continuously (Optional)

```bash
./scripts/install_launchd.sh
```

Useful commands:

```bash
launchctl list | grep focusmate
launchctl unload ~/Library/LaunchAgents/com.focusmate.backend.plist
launchctl load ~/Library/LaunchAgents/com.focusmate.backend.plist
```

## CI

This repo includes a lightweight GitHub Actions workflow to run Python compile checks on push/PR.

## Troubleshooting

- `{"detail":"Not Found"}`:
  - Open `http://localhost:8000/app/` (include trailing slash).
- White screen/plain text:
  - Hard refresh in Safari with `Cmd + Option + R`.
- Service worker fetch errors:
  - Clear service workers from browser dev console and reload.
- `429 insufficient_quota`:
  - OpenAI billing/limits issue for the API key.

## Repository Docs

- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)
- [docs/EMPLOYER-OVERVIEW.md](docs/EMPLOYER-OVERVIEW.md)
- [docs/PROFILE-KIT.md](docs/PROFILE-KIT.md)
- [docs/ROADMAP.md](docs/ROADMAP.md)
- [docs/SECURITY.md](docs/SECURITY.md)

## Portfolio Positioning Tips

- Pin this repository on your GitHub profile.
- Add a short demo video/GIF in the README.
- Use consistent keywords in your profile: `AI assistant`, `FastAPI`, `LLM`, `data workflows`, `product analytics`.

## License

MIT. See [LICENSE](LICENSE).

## Employer Quick Links
- Project brief: `docs/PROJECT_BRIEF.md`
- Resume bullets: `docs/RESUME_BULLETS.md`
- Security policy: `SECURITY.md`
- Contribution guide: `CONTRIBUTING.md`
- Code of conduct: `CODE_OF_CONDUCT.md`
