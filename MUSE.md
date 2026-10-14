# MUSE.md — Instructions for Muse (read this first)

You are helping the user run Qwen-Image 2.1 **UNCENSORED** in **their own Google Colab**.
You operate their browser. They own the Colab account, the GPU quota, and the prompts.

## What this repo is

- `notebook/qwen-image-2.1-colab.ipynb` — the only notebook you need.
  Stack: ComfyUI + `leejet/ComfyUI-GGUF` + GGUF `Qwen-Image-2.1-Uncensored` (Q4_K_M).
  This build has **no safety checker / no content filter**. NSFW prompts are
  allowed. 18+ only. Never generate real people, minors, or non-consensual
  content.
  It downloads ~15 GB of model files per Colab session, starts ComfyUI on
  port 8188, and exposes `generate(prompt, seed, width, height, steps)`.
- The user runs everything on **their own Colab**. Never use your own
  Colab / account / credentials. Never ask for their Google password or OTP.
  If Colab asks to sign in, stop and hand back to the user.

## Standard flow (follow in order)

1. **Open the notebook in the user's Colab.**
   Upload `notebook/qwen-image-2.1-colab.ipynb` via File > Upload notebook,
   then File > Save a copy in Drive. Set runtime to **T4 GPU**
   (Runtime > Change runtime type > T4). Do not run any cell yet.

2. **Run cells 1–4 in order, verify each print.**
   - Cell 1 → `install OK`
   - Cell 2 → `models OK` (downloads ~15 GB, takes a while)
   - Cell 3 → `ComfyUI READY` on port 8188
   - Cell 4 → `helper OK`
   If a cell fails, report the exact error text. Do not skip ahead.

3. **Run the example (Cell 5) once** to prove the pipeline works:
   `generate("a portrait of a cyberpunk warrior, neon city at night, highly detailed", seed=7)`
   Expect `saved: /content/qwen-uc_00001_.png` + image displayed.

4. **Run the user's prompt** in a new cell at the bottom:
   `generate("<user prompt>", seed=<seed or omit>, width=..., height=..., steps=...)`
   Defaults that work: 1024×1024, 30 steps, cfg 2.5 (already in the helper).
   Portrait? Use `width=768, height=1024`. Landscape? `width=1024, height=768`.

5. **Deliver the file, not a screenshot.**
   Add one cell below the generate cell:
   ```python
   from google.colab import files
   files.download('/content/<exact-filename-from-saved-line>.png')
   ```
   Run it once. Download the file the browser captures and send that exact
   file to the user. Never send a screenshot as a substitute for the file.
   Never guess the filename — copy it from the `saved:` line.

## Rules that matter (learned the hard way)

- **Never reload / force-save / restart runtime / re-run cells 1–4**
  without the user's explicit permission. A reload does not stop a
  server-side generate, but the user must approve it first.
- **Runtime disconnects happen.** If the page says "Connect to a new
  runtime" or a cell shows "not executed in this session", the previous
  generate is lost — report that plainly. Ask before reconnecting + re-running.
- **Polling:** `generate()` polls up to ~20 minutes. Check output
  periodically. If only the `queued <prompt_id> | seed ...` line shows and
  no `saved:` line appears after the timeout, report `TimeoutError` exactly.
- **Do not invent results.** If there is no `saved:` line and no image,
  say so. Do not describe an image you did not see.
- **Banner "Automatic saving failed / file updated remotely":**
  do not act on it while a cell is running. Report it, don't force-save.
- **Prompts are the user's.** You run what they asked. You do not
  editorialize, censor, or rewrite their prompt. The model in this notebook
  has no safety checker; the user is responsible for what they generate
  on their own Colab.
- **Costs:** Colab GPU quota is the user's. Warn once that model download
  is ~15 GB per session and a T4 session is time-limited. Don't start a
  second session without asking.

## What to report back

Always report, in this order:
1. Whether each of cells 1–4 succeeded (with their exact print lines).
2. The exact `saved:` path, seed, size, and steps for the user's image.
3. The downloaded file itself (as an attachment).
4. Any error verbatim, if it failed.

## Troubleshooting (check before declaring failure)

- `FqnToConfig` / torchao import error → wrong notebook version; use the
  notebook in this repo, not the old Turbo notebook.
- `Unknown model architecture!` → wrong GGUF node fork; this repo uses
  `leejet/ComfyUI-GGUF`, not `city96`.
- Page stuck at "Loading..." → do not reload without permission. Tell the
  user the page is stuck and ask whether to reload once.
- `render timeout` → ComfyUI log is at `/content/comfyui.log`. Report it.

## What NOT to do

- Don't ask for passwords, OTPs, or API keys.
- Don't open the user's Drive files other than this notebook.
- Don't run anything on Vast.ai, Kaggle, or any other GPU — Colab only,
  unless the user explicitly asks for another provider.
- Don't claim a run succeeded without a `saved:` line.
