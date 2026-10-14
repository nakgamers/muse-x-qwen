# Muse × Qwen-Image 2.1

> 🌐 Read in Indonesian: [README.id.md](README.id.md)

Run Qwen-Image 2.1 on **your own Google Colab**, driven by **your Muse**.
No GPU to rent. No server to maintain. Your Colab, your quota, your prompts.

Your Muse opens the notebook in your Colab, runs it, generates the image,
and sends you the file. You just describe what you want.

## How it works

```
You → Muse: "generate X"
Muse → your Colab (T4 GPU): upload notebook → run ComfyUI → generate
Muse → you: the .png file
```

Stack: **ComfyUI** + **ComfyUI-GGUF** (leejet fork) + **Qwen-Image 2.1 GGUF Q4_K_M**.
Model files (~15 GB per session) are downloaded from Hugging Face on first run.
No safety checker in this GGUF build — you are responsible for what you
generate on your own Colab.

## Quickstart (2 minutes)

### Option A — With Muse (recommended)

1. Give your Muse this repo (clone it, or paste the link).
2. Copy the prompt from [`PROMPT.md`](PROMPT.md), replace the prompt text
   with what you want.
3. Paste it to your Muse. Done — it reads [`MUSE.md`](MUSE.md) and runs
   the whole flow in your Colab: upload → run → generate → download → send file.

### Option B — Manual (no Muse)

1. Open [Google Colab](https://colab.research.google.com), set runtime to **T4 GPU**
   (Runtime → Change runtime type → GPU).
2. Upload `notebook/qwen-image-2.1-colab.ipynb` (File → Upload notebook),
   then File → Save a copy in Drive.
3. Run cells 1–4 in order. Expect: `install OK` → `models OK` →
   `ComfyUI READY` → `helper OK`.
4. Run Cell 5 (example). Expect `saved: /content/qwen-uc_00001_.png`.
5. In a new cell, run your own prompt:
   ```python
   generate("your prompt here", seed=42, steps=30)
   ```
6. Download the result:
   ```python
   from google.colab import files
   files.download('/content/qwen-uc_00002_.png')  # use the exact saved: path
   ```

## Examples

| Prompt | Result |
|---|---|
| `a portrait of a cyberpunk warrior, neon city at night, highly detailed` (seed 7) | Cell 5 example — should work on first run |
| Portrait | `generate("...", width=768, height=1024)` |
| Landscape | `generate("...", width=1024, height=768)` |

Sweet spot: **25–35 steps**, cfg 2.5 (already set in the helper).
First generate is slow (model load). Later ones are much faster.

## Why Muse needs `MUSE.md`

Other Muse instances don't need to be trained for this repo. They read
`MUSE.md` at the start of the session — the same pattern as `AGENTS.md`
in coding repos. That file contains the exact run order, the verification
prints for each cell, the download step, and the hard rules learned from
real runs:

- Never reload / force-save / restart runtime without permission.
- Never claim success without a `saved:` line.
- Always deliver the file, not a screenshot.
- If the runtime disconnects, the in-flight generate is lost — report it.

Copy `MUSE.md` + `PROMPT.md` into any Muse session and it can repeat this
flow on its own Colab.

## Repo layout

```
muse-x-qwen/
├── MUSE.md                    # instructions your Muse reads first
├── PROMPT.md                  # copy-paste prompt for your Muse
├── README.md                  # this file (English)
├── README.id.md               # Indonesian version
├── notebook/
│   └── qwen-image-2.1-colab.ipynb   # the only notebook you need
└── docs/
    └── TROUBLESHOOTING.md     # errors we actually hit + fixes
```

## Troubleshooting

See [`docs/TROUBLESHOOTING.md`](docs/TROUBLESHOOTING.md) for the full list.
The three that bite most:

- **Page stuck at "Loading..."** — don't reload without asking the user.
  Tell them it's stuck and ask for permission to reload once.
- **Runtime disconnected** — the running generate is lost. Reconnect,
  then re-run only the generate cell (cells 1–4 stay done if the kernel
  survived; if not, re-run from cell 1).
- **`render timeout`** — check `/content/comfyui.log` for the real error.

## Notes

- Colab free tier works (T4). Model download is ~15 GB per session —
  it re-downloads every new session.
- Don't run this on someone else's Colab / account. Everyone uses their own.
- This repo is a workflow wrapper. The model weights belong to their
  respective owners on Hugging Face. Use them per their licenses.

## Star this if it saved you a GPU bill

If this repo got you generating on Colab without renting a GPU, give it a ⭐.
Issues and PRs welcome — especially better prompts, better defaults,
and cleaner error handling.
