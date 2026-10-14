# Copy-paste prompt — for your Muse

Copy everything below, paste it to your Muse, and replace the prompt in
brackets with what you want to generate.

---

Bukain notebook `notebook/qwen-image-2.1-colab.ipynb` dari repo ini di
Google Colab-ku (akun Google-ku sendiri, runtime T4 GPU).

Baca `MUSE.md` di repo ini dan ikutin langkahnya persis:
1. Upload notebook ke Colab, save a copy in Drive, set runtime T4 GPU.
2. Run cells 1–4, verifikasi tiap print-nya.
3. Run contoh Cell 5 sekali buat buktiin pipeline jalan.
4. Generate gambar ini:
   `generate("[TULIS PROMPT KAMU DI SINI]", seed=42, steps=30)`
   Kalau mau portrait: tambah `width=768, height=1024`.
5. Download file hasil `saved:`-nya pakai `files.download()` dan kirim
   file-nya ke aku — bukan screenshot.

Aturan: jangan reload / force save / restart runtime / run ulang cells 1–4
tanpa izin aku dulu. Kalau runtime disconnect atau generate timeout,
laporin error persisnya, jangan ngarang hasil.

---

## Contoh prompt (tinggal ganti)

```text
Bukain notebook `notebook/qwen-image-2.1-colab.ipynb` dari repo ini di
Google Colab-ku (akun Google-ku sendiri, runtime T4 GPU).

Baca `MUSE.md` di repo ini dan ikutin langkahnya persis.
Generate gambar ini:
generate("a portrait of a cyberpunk warrior, neon city at night, highly detailed", seed=7)
Download file hasil saved:-nya dan kirim file-nya ke aku — bukan screenshot.
```

## Kenapa ini bekerja

Muse lain tidak butuh "dilatih" untuk repo ini. Dia cukup membaca
`MUSE.md` — file instruksi yang dibaca Muse di awal sesi, sama seperti
`AGENTS.md` di repo coding. Semua aturan (urutan cell, apa yang boleh /
tidak boleh dilakukan, cara download file) sudah tertulis di sana, jadi
Muse lain bisa mengulang persis apa yang dilakukan di sini, di Colab-nya
sendiri.
