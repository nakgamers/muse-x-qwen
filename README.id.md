# Muse × Qwen-Image 2.1 UNCENSORED

Jalankan **Qwen-Image 2.1 Uncensored** di **Google Colab-mu sendiri**, digerakkan oleh **Muse-mu**.
Tanpa sewa GPU. Tanpa server yang harus dirawat. Colab-mu, kuotamu, prompt-mu.
Tanpa safety filter. Tanpa prompt yang di-block. **NSFW boleh — khusus 18+.**

Muse-mu yang membukakan notebook di Colab-mu, menjalankannya, men-generate
gambarnya, dan mengirimkan file-nya ke kamu. Kamu cukup menjelaskan maumu apa — SFW atau NSFW.

> 📄 Baca dalam bahasa Inggris: [README.md](README.md)

## Cara kerjanya

```
Kamu → Muse: "generate X"
Muse → Colab-mu (GPU T4): upload notebook → jalankan ComfyUI → generate
Muse → kamu: file .png-nya
```

Stack: **ComfyUI** + **ComfyUI-GGUF** (fork leejet) + **Qwen-Image 2.1 Uncensored GGUF Q4_K_M**.
File model (~15 GB per sesi) diunduh dari Hugging Face saat pertama kali jalan.

## Uncensored — maksudnya apa

Ini **bukan** rilis resmi Qwen yang ada safety filter-nya. Ini build GGUF
komunitas (`Qwen-Image-2.1-Uncensored-Quantized`) yang **tanpa safety checker /
tanpa content filter**. Prompt yang bakal ditolak model resmi — termasuk NSFW —
jalan di sini.

- Khusus 18+. Kamu bertanggung jawab atas apa yang kamu generate di Colab-mu sendiri.
- Jangan pernah generate orang beneran, anak di bawah umur, atau konten non-konsensual.
- Semua jalan lokal di sesi Colab-mu sendiri. Repo ini tidak mengirim apa pun
  ke server pihak ketiga.

## Mulai cepat (2 menit)

### Opsi A — Pakai Muse (disarankan)

1. Kasih repo ini ke Muse-mu (clone, atau paste link-nya).
2. Copy prompt dari [`PROMPT.md`](PROMPT.md), ganti teks prompt-nya dengan
   yang kamu mau.
3. Paste ke Muse-mu. Beres — dia baca [`MUSE.md`](MUSE.md) dan menjalankan
   seluruh alurnya di Colab-mu: upload → run → generate → download → kirim file.

### Opsi B — Manual (tanpa Muse)

1. Buka [Google Colab](https://colab.research.google.com), set runtime ke **GPU T4**
   (Runtime → Change runtime type → GPU).
2. Upload `notebook/qwen-image-2.1-colab.ipynb` (File → Upload notebook),
   lalu File → Save a copy in Drive.
3. Jalankan cell 1–4 berurutan. Harusnya muncul: `install OK` → `models OK` →
   `ComfyUI READY` → `helper OK`.
4. Jalankan Cell 5 (contoh). Harusnya muncul `saved: /content/qwen-uc_00001_.png`.
5. Di cell baru, jalankan prompt-mu sendiri:
   ```python
   generate("prompt kamu di sini", seed=42, steps=30)
   ```
6. Download hasilnya:
   ```python
   from google.colab import files
   files.download('/content/qwen-uc_00002_.png')  # pakai path saved: yang persis
   ```

## Contoh

| Prompt | Hasil |
|---|---|
| `a portrait of a cyberpunk warrior, neon city at night, highly detailed` (seed 7) | Contoh Cell 5 — harusnya langsung jalan di run pertama |
| Portrait | `generate("...", width=768, height=1024)` |
| Landscape | `generate("...", width=1024, height=768)` |

Sweet spot: **25–35 steps**, cfg 2.5 (sudah diatur di helper).
Generate pertama lambat (load model). Generate berikutnya jauh lebih cepat.

## Kenapa Muse butuh `MUSE.md`

Muse lain tidak perlu "dilatih" untuk repo ini. Mereka membaca `MUSE.md`
di awal sesi — pola yang sama seperti `AGENTS.md` di repo coding. File itu
berisi urutan run yang persis, print verifikasi tiap cell, langkah download,
dan aturan keras hasil dari run beneran:

- Jangan reload / force-save / restart runtime tanpa izin.
- Jangan klaim berhasil tanpa baris `saved:`.
- Selalu kirim file-nya, bukan screenshot.
- Kalau runtime disconnect, generate yang sedang jalan hilang — laporkan.

Copy `MUSE.md` + `PROMPT.md` ke sesi Muse mana pun dan dia bisa mengulang
alur ini di Colab-nya sendiri.

## Struktur repo

```
muse-x-qwen/
├── MUSE.md                    # instruksi yang dibaca Muse-mu pertama kali
├── PROMPT.md                  # prompt copy-paste buat Muse-mu
├── README.md                  # versi bahasa Inggris
├── README.id.md               # file ini (versi bahasa Indonesia)
├── notebook/
│   └── qwen-image-2.1-colab.ipynb   # satu-satunya notebook yang dibutuhkan
└── docs/
    └── TROUBLESHOOTING.md     # error yang beneran kita kena + cara betulin
```

## Troubleshooting

Lihat [`docs/TROUBLESHOOTING.md`](docs/TROUBLESHOOTING.md) untuk daftar lengkap.
Tiga yang paling sering menggigit:

- **Halaman stuck di "Loading..."** — jangan reload tanpa tanya user.
  Bilang halamannya stuck dan minta izin reload sekali.
- **Runtime disconnect** — generate yang sedang jalan hilang. Reconnect,
  lalu run ulang hanya cell generate-nya (cells 1–4 tetap beres kalau kernel
  selamat; kalau tidak, run ulang dari cell 1).
- **`render timeout`** — cek `/content/comfyui.log` untuk error aslinya.

## Catatan

- Colab tier gratis bisa (T4). Download model ~15 GB per sesi —
  diunduh ulang setiap sesi baru.
- Jangan jalankan ini di Colab / akun orang lain. Masing-masing pakai punya sendiri.
- Repo ini adalah wrapper workflow. Bobot model milik pemiliknya masing-masing
  di Hugging Face. Gunakan sesuai lisensinya.

## DYWOR — Do Your Own Research

Ingat ya, DYWOR. Baca README-nya, pahami apa yang kamu jalankan di Colab-mu
sendiri, dan pakai dengan tanggung jawab. Repo ini cuma ngasih workflow-nya —
apa yang kamu generate adalah tanggung jawabmu. Jangan pernah generate orang
beneran, anak di bawah umur, atau konten non-konsensual.

## Kasih ⭐ kalau ini menghemat tagihan GPU-mu

Kalau repo ini bikin kamu bisa generate uncensored di Colab tanpa sewa GPU, kasih ⭐.
Issues dan PR dipersilakan — terutama prompt yang lebih bagus, default yang
lebih enak, dan error handling yang lebih rapi.
