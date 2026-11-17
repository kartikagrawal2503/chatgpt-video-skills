# Lemo-Opuscar for ChatGPT (Codex)

A port of the [Lemo-Opuscar](https://github.com/lemomo-ai/lemo-opuscar) Claude Code plugin (by Lemomo) to OpenAI Codex, the coding agent in the ChatGPT desktop app, CLI and IDE extension. Pick one of 99 film styles, give a topic, and the agent directs, draws, scores, mixes and renders a short film entirely in code.

## What's here

```
.agents/plugins/marketplace.json          Codex marketplace listing this plugin (local source)
plugin/.codex-plugin/plugin.json          Codex plugin manifest (was .claude-plugin/plugin.json)
plugin/skills/lemo-opuscar/SKILL.md       the skill, with a Codex/ChatGPT section added
plugin/skills/lemo-opuscar/agents/openai.yaml   display name and default prompt in the Codex UI
plugin/skills/lemo-opuscar/scripts/setup.sh     unchanged: clones the library, installs packages
```

The style library itself (guides, `core/` tools, every `STYLE.md`) isn't copied here. On first use, `setup.sh` clones it from upstream into `~/lemo-opuscar`, as the original plugin does. Its guides are written for Claude Code, so `SKILL.md` tells Codex what to do in their place:

| Claude Code | Codex |
|---|---|
| `AskUserQuestion` tool | `request_user_input` if available, else a numbered plain-text question |
| `<skill base directory>` | the folder holding `SKILL.md` |
| `gen_image.py` drives the `codex` CLI for ✦ styles | Codex's built-in `image_gen` is used directly |
| reading frames with the Read tool | `view_image` |
| `/effort high` | high reasoning effort |

## Install

Skill only (simplest):

```bash
cp -r plugin/skills/lemo-opuscar ~/.agents/skills/
```

Or as a plugin: add this folder as a local marketplace in Codex and install **lemo-opuscar** from it.

Then restart Codex and ask, for example, "Which film styles are there?"

## Requirements

Node 20+, ffmpeg 7+, Python 3.11+ (or `uv`), git, and a POSIX `sh` (on Windows, the one from Git for Windows). The first `setup.sh deps` downloads a headless Chromium and Python packages, so it needs network access outside the Codex sandbox.

## Caveats

- Upstream says the styles were tuned for Claude models and other models may reproduce them less faithfully.
- Licence: code MIT; the docs, including `SKILL.md`, are CC BY-NC 4.0 (non-commercial only, with attribution). See `LICENSE`. This port changes `SKILL.md` and the manifests as described above.
