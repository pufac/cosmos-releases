# Installing Cosmos (Linux)

Cosmos is a voice assistant that runs on your PC: you talk (or type), it answers out
loud, searches the web, remembers things you tell it, sets reminders on your phone and
opens apps. Speech recognition and the voices run on your computer.

## What you need

- Linux on a normal 64-bit PC (x86_64 or ARM64), any distribution
- ~2 GB of disk space (Cosmos with its libraries ~600 MB, voice models ~650 MB)
- For voice: a microphone and PipeWire (the default on current Fedora, Ubuntu, Mint,
  Arch, KDE and GNOME). Headphones work best.
- A language model, one of these (setup helps you choose):
  - **Local**: [Ollama](https://ollama.com/download) and a graphics card with ~8 GB
    (5 GB for the smaller one), or the tiny Liquid LFM3 1.7B for almost
    any PC (less reliable with tools). Private and free.
  - **Cloud**: a free API key from Groq or Google Gemini (no credit card), OpenRouter, or
    paid Claude credits. Works on any PC; your questions go to that company, your
    memories and notes don't.
- For web search: podman or docker (setup starts a private search engine, SearXNG).

## Install

Open a terminal and run:

```bash
curl -fsSL https://github.com/pufac/cosmos-releases/releases/latest/download/install.sh | sh
```

It needs no root/sudo. It installs [uv](https://docs.astral.sh/uv/) (a Python installer)
if you don't have it, downloads Cosmos, checks it, and installs it with its own Python in
your home folder. Then **`cosmos setup`** asks a few questions:

1. what Cosmos should call you, and your time zone
2. which model to use (and tests it)
3. voice on or off, which microphone, and the "hey Cosmos" wake word
4. reminders on your phone (the free ntfy app) — optional
5. web search (starts SearXNG with podman/docker if you say yes)

Then it downloads the voice models (one time) and adds Cosmos to your application menu.

## Use

Start **Cosmos** from the application menu (or `cosmos gui` in a terminal). Say
**"hey Cosmos"** and your question, or press **Enter** in the empty text box to talk,
or just type. Settings are behind the gear icon.

Things to try: "what's the weather in Budapest tomorrow?", "remind me in 20 minutes to
take the pizza out", "remember that my sister's birthday is on 3 May", "open Firefox",
"take a note: buy milk", "research the best budget e-readers".

## Updating

Cosmos looks for a new version once a day. When there is one, the app says so:
**Settings → About → Update**, then close Cosmos and start it again. Or in a terminal:

```bash
cosmos update
```

## When something doesn't work

```bash
cosmos check      # what works and what doesn't, with the command that fixes it
cosmos setup      # answer the questions again (your old settings are kept as .bak)
```

- **No answers / "model unreachable"**: local model: is Ollama running (`ollama list`)?
  Cloud: is the API key right? `cosmos models test` checks it.
- **Web search is down after a restart**: `podman start searxng` (or `docker start searxng`).
- **It doesn't hear you**: Settings → Audio, or `cosmos setup` to pick another microphone.
  The wake word works best within a couple of meters.
- **Log file**: `~/.local/state/cosmos/cosmos.log`

## Where things are

| What | Where |
|---|---|
| Settings | `~/.config/cosmos/config.toml` (all options: `config.example.toml` next to it) |
| Memories (Markdown, opens in Obsidian) | `~/Documents/cosmos-brain/` |
| Notes | `~/Documents/cosmos-notes/` |
| Voice models, preferences | `~/.local/share/cosmos/` |
| The program | `~/.local/share/uv/tools/cosmos/` |

## Uninstall

```bash
cosmos uninstall
```

Removes the program and the menu entry. Your settings, memories, notes and voice models
stay (it lists the folders, delete them if you want). SearXNG: `podman rm -f searxng`.

## Licenses

Cosmos is free for personal use. The bundled "hey Cosmos" and "hey Jarvis" wake word
models are CC BY-NC-SA 4.0 (non-commercial). The Piper voice engine is GPL-3.0, the
Kokoro voice Apache-2.0.
