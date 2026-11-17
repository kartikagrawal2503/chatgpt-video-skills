---
name: lemo-opuscar
description: Direct and produce a short film made entirely in code, in one of the styles of the Lemo-Opuscar library (e.g. Impasto Oil Painting 油画厚涂, Watercolor Brush 水彩笔刷, Ukiyo-e 浮世绘, Satellite Briefing 卫星简报, Exploded Teardown 产品拆解). Use when the user asks for a video, film, short, promo, explainer or animation (视频、短片、动画、宣传片) in a named style, including explainers built from real material such as satellite imagery, laser scans, CAD files or archive scans (数据解说、科普、产品拆解), and new films cut from real archive footage and documents (剪辑纪实, e.g. 用档案影像剪一支新闻片); when they want a film about their own topic and haven't chosen a style (help them choose); when they want to change a style into their own, make their own style from a blank page, or use a style they saved (改风格、从空白开始、我的风格); when they start the skill without saying what they want; or when they ask which film styles there are. Not for plain editing, trimming or converting the user's existing video files.
---

# Lemo-Opuscar

The styles, guides and tools live in the Lemo-Opuscar library (github.com/lemomo-ai/lemo-opuscar). This skill fetches it and hands you over to it. `<skill dir>` below is the folder that holds this `SKILL.md`.

1. **Get the library.** Run `sh "<skill dir>/scripts/setup.sh"`. It clones or updates the library (default `~/lemo-opuscar`; a clone the user is standing in is used as it is), lists any missing system tools (Node, ffmpeg, Python) and prints `LIB=<path>` on its last line. It needs network access: if the sandbox blocks it, ask for approval to run it outside the sandbox.
2. **Install the core tools early.** Start `sh "<skill dir>/scripts/setup.sh" deps` right away, detached if your shell allows it, otherwise in the foreground with a long timeout: it takes a few minutes the first time. Add `deps voice` or `deps music` later, only if the film needs them (`$LIB/TECHNIQUE.md` §1). Report missing system tools (Node, ffmpeg, Python) in your one round of questions; don't install system software yourself. A style marked ✦ also needs an image generator (see the table below). Packages a style needs (its `STYLE.md` lists them) you install yourself, into the project folder.
3. **Follow the library.** Read `$LIB/AGENTS.md` and do what it says, starting with **Start** (the user's own styles in `~/lemo-my-styles/`; when they started the skill without writing anything, the language question first, then the ways in): finding the style or making their own, the one round of questions, the treatment, production and delivery.

What changes in skill mode:

| | |
|---|---|
| Project folder | `<name>/` in the folder the user started from (lowercase letters, digits, hyphens; avoid `#` and `?` in the path). Wherever the guides say `films/<name>/`, read this folder |
| Running tools | from `$LIB`, with the project's absolute path: `cd "$LIB" && node core/render/still.mjs "<project>" 1.5 3` |
| Library files in pages | absolute URLs: `/core/lib.js`, `/node_modules/three/…` (the project is served at `/@film/`) |
| Library files in scripts and `build.sh` | `$LIB/…`, with `LIB=<path>` at the top of `build.sh`; never `../..` |
| Demo source | not downloaded. `sh "<skill dir>/scripts/setup.sh" demo <slug>`, only after your `TREATMENT.md` is written, to read its techniques; it is not meant to be rendered. An Edit & Documentary style is the exception: fetch it before the treatment to use the material tools its `STYLE.md` names in `demo/tools/` (`$LIB/AGENTS.md`, "Treatment first") |

What changes in Codex / ChatGPT (the library's guides were written for Claude Code):

| The guides say | Do this |
|---|---|
| the `AskUserQuestion` tool | use your multiple-choice question tool if you have one (e.g. `request_user_input`); otherwise ask in plain text: the question, then 2–4 numbered options, the recommended one first and marked (Recommended), one line each, and a last line saying they can also type their own answer. Then stop and wait for the reply. The rules about *when* to ask and *what* to offer stay as written |
| `sh …` on Windows | the scripts need a POSIX `sh` (Git for Windows ships one). From PowerShell call it as `sh "<path>/setup.sh"`, or through `bash -lc '…'`; pass paths with forward slashes |
| image generator for ✦ styles (`core/image/gen_image.py`, which drives the Codex CLI) | if you have a built-in image tool (`image_gen`), that is the user's image generator: `gen_image.py --check` may fail only because the `codex` command isn't on PATH, and that doesn't block the film. Generate each image with your tool from the prompts in the style's `STYLE.md`, save it under `<project>/gen/` with the file name the style's scripts expect, and check its aspect ratio (5 % tolerance) before you use it. Tell the user how many images and of what before you start. With no image tool at all, follow `$LIB/core/README.md`, "Image generation" |
| look at a frame, still or contact sheet | open the rendered PNG with your image-viewing tool (e.g. `view_image`); never judge a frame you haven't seen |
| `/effort high` | use high reasoning effort for this skill: the styles are detailed and easy to get subtly wrong |

Deliver what `$LIB/DIRECTOR.md` §11 lists, in the project folder, and tell the user where it is.
