# RHClaw — RunningHub Skill for OpenClaw

[中文](./README_zh.md)

An [OpenClaw](https://github.com/openclaw/openclaw) skill that brings multimedia generation to conversational AI, powered by the [RunningHub](https://www.runninghub.cn) API. Generate images, videos, audio, 3D models, and more through natural language — backed by **179 standard API endpoints** plus **unlimited custom ComfyUI workflows** (AI Applications).

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](./LICENSE)
[![Python](https://img.shields.io/badge/python-3.x-blue.svg)](https://www.python.org/)
[![Dependencies](https://img.shields.io/badge/dependencies-none-brightgreen.svg)]()

---

## Overview

RHClaw connects OpenClaw's AI assistant to RunningHub's cloud multimedia services. Once installed, users can create rich media content through natural language without managing API calls, polling logic, or file handling — the skill handles all of that automatically.

It supports two operation modes:
- **Standard API** — 179 curated endpoints spanning images, video, audio, 3D, and text
- **AI Applications** — run any custom ComfyUI workflow hosted on RunningHub

---

## Capabilities

| Category | Endpoints | Tasks |
|----------|-----------|-------|
| **Image** | 44 | text-to-image, image-to-image, image upscale, Midjourney-style |
| **Video** | 101 | text-to-video, image-to-video, start+end frames, video extend/edit, motion control |
| **Audio** | 8 | text-to-speech, music generation, voice cloning |
| **3D** | 12 | text-to-3D, image-to-3D, multi-image-to-3D |
| **Text** | 14 | image understanding, video understanding, text processing |
| **AI Apps** | Unlimited | Run any RunningHub AI Application (custom ComfyUI workflow) |

---

## Quick Start

### Install

In your OpenClaw chat, send:

```
Install the RunningHub skill from https://github.com/HM-RunningHub/OpenClaw_RH_Skills
```

The assistant will clone the repo, copy files to the workspace, and guide you through API key setup.

### Update

When a new version is available:

```
Update from https://github.com/HM-RunningHub/OpenClaw_RH_Skills and re-read @runninghub/SKILL.md
```

The assistant will pull the latest code and reload the skill config. Your API key is preserved.

### Prerequisites

- **API Key** — Create one at [RunningHub API Management](https://www.runninghub.cn/enterprise-api/sharedApi) (click "新建")
- **Wallet balance** — [Recharge here](https://www.runninghub.cn/vip-rights/4) — API calls consume credits

---

## Usage

Once installed, talk to your OpenClaw assistant naturally:

```
Generate a picture of a dog playing in the park
Turn this photo into a video
Create background music for my video
Upscale this image to 4K
Convert this image to a 3D model
Run this AI app: https://www.runninghub.cn/ai-detail/1877265245566922800
What are the hottest AI apps right now?
```

The assistant selects the best RunningHub endpoint automatically. For AI Apps, it fetches node info, guides parameter setup, and runs the workflow.

### Image Model Selection

When generating images, the assistant presents 5 curated models:

| # | Model | Best For |
|---|-------|----------|
| 1 | 🎨 **RH Image PRO** | Best overall quality — recommended default |
| 2 | ⚡ **RH Image V2** | Fastest and most affordable |
| 3 | 🎭 **Youchuan v7** | Midjourney-style, cinematic look |
| 4 | 🌸 **Youchuan niji7** | Anime / illustration style |
| 5 | 📷 **Seedream v5** | ByteDance — strong photorealistic feel |

Pick a number or skip to use the default (RH Image PRO).

### Video Model Selection

When generating video, the assistant presents 8 curated models:

| # | Model | Best For |
|---|-------|----------|
| 1 | 🚀 **Google Veo 3.1 Fast** | Fast with great quality — best value |
| 2 | 🔥 **Grok Video** | Grok-powered, incredible imagination |
| 3 | 🎯 **Kling v3.0 Pro** | Natural motion, best for people |
| 4 | 🎬 **Google Veo 3.1 Pro** | Cinematic quality |
| 5 | ✨ **Vidu Q3 Pro** | Unique stylized look |
| 6 | ⭐ **Sora** | Sora-class engine |
| 7 | 🌊 **MiniMax Hailuo** | Fast with fine details |
| 8 | 🌱 **Seedance v1.5 Pro** | Smooth motion, great for dance |

Pick a number or skip to use the default (Google Veo 3.1 Fast).

---

## Architecture

```
runninghub/
├── SKILL.md                        # OpenClaw skill definition (routing, persona, rules)
├── scripts/
│   ├── runninghub.py               # Standard model API client (179 endpoints)
│   ├── runninghub_app.py           # AI Application client (custom ComfyUI workflows)
│   └── build_capabilities.py       # Generates capabilities.json from models_registry.json
├── data/
│   └── capabilities.json           # Full endpoint catalog (auto-generated)
└── references/
    ├── api-key-setup.md            # API key configuration guide
    ├── output-delivery.md          # Output handling rules
    ├── image-models.md             # Image model selection flow
    ├── video-models.md             # Video model selection flow
    └── ai-application.md           # AI app workflow guide
```

---

## Script Reference

### Standard Model API (`runninghub.py`)

| Mode | Command | Purpose |
|------|---------|---------|
| **Check** | `--check` | Verify API key + check wallet balance |
| **List** | `--list [--type T] [--task T]` | Browse available endpoints |
| **Info** | `--info ENDPOINT` | View endpoint parameters |
| **Execute** | `--endpoint EP --prompt "..." -o /tmp/out` | Run with a specific endpoint |
| **Auto** | `--task TASK --prompt "..." -o /tmp/out` | Auto-select the best endpoint |

```bash
# Examples
python3 scripts/runninghub.py --check
python3 scripts/runninghub.py --list --type image
python3 scripts/runninghub.py --info rhart-image-n-pro/text-to-image
python3 scripts/runninghub.py --task text-to-image --prompt "a cat on Mars" -o /tmp/cat.png
```

### AI Application (`runninghub_app.py`)

| Mode | Command | Purpose |
|------|---------|---------|
| **Check** | `--check` | Verify API key + check wallet balance |
| **Browse** | `--list [--sort S] [--size N] [--page N]` | Browse recommended/hottest/newest apps |
| **Nodes** | `--info WEBAPP_ID` | Show modifiable nodes for an AI app |
| **Execute** | `--run WEBAPP_ID --node ... --file ... -o /tmp/out` | Run an AI application |

```bash
# Examples
python3 scripts/runninghub_app.py --list --sort RECOMMEND --size 10
python3 scripts/runninghub_app.py --list --sort HOTTEST --days 7
python3 scripts/runninghub_app.py --info 1877265245566922800
python3 scripts/runninghub_app.py --run 1877265245566922800 --node "52:prompt=a sunset" -o /tmp/out.png
```

---

## Configuration

### API Key Setup

The API key is resolved in this order:

1. `--api-key` CLI flag
2. `RUNNINGHUB_API_KEY` environment variable
3. `~/.openclaw/openclaw.json` → `skills.entries.runninghub.apiKey`

See [`references/api-key-setup.md`](./runninghub/references/api-key-setup.md) for detailed instructions.

### Updating the Capabilities Catalog

When RunningHub adds new API endpoints, regenerate `capabilities.json`:

```bash
python3 scripts/build_capabilities.py \
  --registry /path/to/ComfyUI_RH_OpenAPI/models_registry.json \
  --output data/capabilities.json
```

---

## Requirements

| Requirement | Details |
|-------------|---------|
| Python | 3.x (standard library only — no pip installs) |
| curl | Must be available in `PATH` |
| RunningHub account | API key + wallet balance required |
| OpenClaw | Required to use as a conversational skill |

No third-party Python packages are required.

---

## License

[Apache-2.0](./LICENSE) — Copyright 2026 RunningHub
