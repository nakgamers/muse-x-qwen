# Troubleshooting — errors we actually hit

## 1. Page stuck at "Loading..."

**Symptom:** Notebook page never renders cells.
**Fix:** Don't reload without the user's permission. Tell them it's stuck
and ask: "boleh reload sekali?" A reload does not stop a server-side
generate, but the user must approve it first.

## 2. Runtime disconnected / "Connect to a new runtime"

**Symptom:** Cell shows "not executed in this session", or Colab says
the runtime is gone.
**What happened:** The in-flight generate is lost. The `queued ...` line
was printed but the run never finished.
**Fix:** Reconnect, then re-run only the generate cell. If cells 1–4 also
lost their state (kernel did not survive), re-run from cell 1. Ask the user
before re-running.

## 3. `TimeoutError: render timeout`

**Symptom:** `generate()` waited ~20 minutes, no `saved:` line.
**Fix:** Check `/content/comfyui.log` for the real error. Common cause:
Colab runtime hiccup mid-run. Re-run the generate cell once.

## 4. `Unknown model architecture!`

**Symptom:** ComfyUI rejects the GGUF file.
**Cause:** Wrong GGUF node fork (old `city96` fork doesn't support
Qwen-Image 2.1).
**Fix:** Use the notebook in this repo. It uses `leejet/ComfyUI-GGUF`.

## 5. `FqnToConfig` / torchao import error

**Symptom:** `ImportError: cannot import name 'FqnToConfig' from
'torchao.quantization'`.
**Cause:** Old Turbo/diffusers notebook with mismatched `torchao`.
**Fix:** Don't use the old Turbo notebook. This repo's notebook uses
ComfyUI + GGUF and does not hit this.

## 6. "Automatic saving failed / file updated remotely"

**Symptom:** Banner at the top of Colab.
**Fix:** Do nothing while a cell is running. Don't force-save, don't
reload. Report it. It does not stop the kernel-side generate.

## 7. Only `queued ...` line, no `saved:` line

**Symptom:** Cell 7 output shows only `queued <id> | seed ...`.
**Meaning:** Generate started but hasn't finished yet (or was lost on
disconnect). Wait for the `saved:` line. If the runtime disconnected,
see #2. Never claim the image exists without a `saved:` line.

## 8. Download gives the wrong file

**Fix:** Copy the filename from the `saved:` line exactly
(`/content/qwen-uc_00002_.png`), don't guess. Use:
```python
from google.colab import files
files.download('/content/qwen-uc_00002_.png')
```
