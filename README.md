# 🎬 youtube-autopilot

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![YouTube Data API](https://img.shields.io/badge/YouTube%20Data%20API-FF0000?style=flat-square&logo=youtube&logoColor=white)
![Stable Diffusion](https://img.shields.io/badge/Stable%20Diffusion-000000?style=flat-square&logo=stabilityai&logoColor=white)
![FFmpeg](https://img.shields.io/badge/FFmpeg-007808?style=flat-square&logo=ffmpeg&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)

An **end-to-end automated YouTube content pipeline**. You give it a topic; it produces a finished, upload-ready video — no manual intervention at any stage.

The system is broken into **22 independent modules** that hand work down a chain: script generation → image sourcing → audio narration → video assembly → publishing metadata. Each module reads from and writes to a shared run directory, so any stage can be re-run or swapped out in isolation.

---

## ✨ Features

- **Fully automated, zero-touch runs** — kick off a job and walk away
- **22-module pipeline** with clear separation of concerns (each module does one thing)
- **AI script generation** from a topic prompt or a queue of topics
- **Automated image sourcing** via API and **Stable Diffusion** generation for custom visuals
- **Text-to-speech narration** with configurable voices and pacing
- **Automated video assembly** — image sequencing, transitions, audio sync, and rendering via FFmpeg/MoviePy
- **JSON-driven configuration** — change behaviour without touching code
- **Structured logging** per run for debugging and auditing
- **Resumable** — re-run a single stage without restarting the whole pipeline

---

## 🧱 Architecture overview

```
topic / queue
     │
     ▼
┌──────────────────┐   ┌──────────────────┐   ┌──────────────────┐
│  Script stage    │──▶│  Visuals stage   │──▶│  Audio stage     │
│  (LLM scripting, │   │  (image sourcing,│   │  (TTS narration, │
│   scene splits)  │   │  SD generation)  │   │   timing)        │
└──────────────────┘   └──────────────────┘   └──────────────────┘
                                                        │
                                                        ▼
                                            ┌──────────────────┐
                                            │  Assembly stage  │
                                            │  (FFmpeg render, │
                                            │   metadata, SEO) │
                                            └──────────────────┘
                                                        │
                                                        ▼
                                                  output/ + upload
```

Every module is invoked by the orchestrator, reads its inputs from the run directory, and writes its outputs back for the next stage to pick up.

### Project structure

```
youtube-autopilot/
├── modules/        # The 22 pipeline modules (script, images, audio, assembly, etc.)
├── config/         # settings.json and environment configuration
├── output/         # Rendered videos and intermediate run artifacts
├── logs/           # Per-run structured logs
├── requirements.txt
├── .gitignore
└── LICENSE
```

---

## 🛠️ Tech stack

| Layer | Technology |
| --- | --- |
| Language | Python 3.10+ |
| Publishing | YouTube Data API v3 (`google-api-python-client`) |
| Image generation | Stable Diffusion |
| Image handling | Pillow |
| Video assembly | MoviePy / FFmpeg |
| HTTP / APIs | `requests` |
| Config & secrets | JSON config + `python-dotenv` |

---

## ⚙️ Setup

> ⚠️ Setup instructions are a placeholder and will be expanded.

```bash
# 1. Clone the repo
git clone https://github.com/portal-source/youtube-autopilot.git
cd youtube-autopilot

# 2. Create a virtual environment
python -m venv .venv
source .venv/bin/activate   # On Windows: .venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Configure
cp config/settings.json config/settings.local.json   # then edit your copy
# Add API keys to a .env file (see config/settings.json for required fields)

# 5. Run the pipeline
python -m modules.orchestrator --topic "your topic here"
```

You'll need: a **YouTube Data API** credential, a Stable Diffusion endpoint or local install, and FFmpeg available on your `PATH`.

---

## 📄 License

Released under the [MIT License](LICENSE).
