# AGENTS.md — Wiki Kuliah (pola llm-wiki)

Schema untuk agent yang memelihara wiki kuliahku di Obsidian.
> Catatan: kata 'aku'/'-ku' di file ini selalu merujuk ke pemilik wiki (manusia), bukan kamu (agent). 
Diadaptasi dari ide "llm-wiki" (gist karpathy).

> Peran: KAMU (agent) adalah programmer-nya wiki — kamu yang menulis dan memelihara semua file .md. 
Obsidian adalah IDE tempat aku membaca hasilnya. 
Aku TIDAK menulis wiki secara manual. 
Tugasku: menyediakan sumber, memberi perintah, dan mengajukan pertanyaan.

## 1. Ide inti

Jangan seperti RAG (mencari potongan dokumen setiap ada pertanyaan, lalu melupakan).
Bangun wiki PERSISTEN: kumpulan markdown terstruktur dan saling terhubung yang duduk
di antara aku dan sumber mentah. Setiap sumber baru: baca → ekstrak info kunci →
integrasikan ke wiki yang sudah ada (update halaman terkait, revisi ringkasan, catat
kontradiksi dengan klaim lama). Wiki harus compounding: makin lama makin kaya, bukan
ditemukan ulang setiap ditanya.

## 2. Struktur folder

```
<vault>/
├── AGENTS.md              ← file ini (dibaca tiap sesi; jangan diubah tanpa izin)
├── index.md               ← katalog isi wiki (dipelihara agent, lihat §5)
├── log.md                 ← catatan kronologis append-only (dipelihara agent)
└── <nama-matkul>/         ← satu folder per mata kuliah, huruf kecil tanpa spasi
    ├── resource/          ← PDF & sumber mentah. IMMUTABLE: hanya dibaca, TIDAK PERNAH diubah
    │   ├── kampus/        ← materi resmi dari kampus (modul, slide dosen)
    │   └── web/           ← artikel, PDF, dan sumber lain dari internet
    └── materi/            ← bebas per TOPIK, bukan per minggu
        ├── 1 - limit.md
        ├── 2 - aturan-rantai.md
        └── ...
```

Contoh: `kalkulus/resource/kampus/Modul-03-Limit.pdf`, `kalkulus/materi/3 - limit.md`

## 3. Konvensi penulisan

- Bahasa: Bahasa Indonesia.
- Nama file & folder: huruf kecil, kata dipisah strip. File materi WAJIB
  berformat `<urutan> - <topik>.md`, contoh: `3 - limit.md`. Nomor `urutan`
  sama dengan field `urutan` di frontmatter.
- Setiap halaman materi WAJIB punya frontmatter YAML. Field `minggu` opsional —
  isi hanya kalau materi terikat minggu kuliah tertentu. Field `urutan` WAJIB —
  nomor urut belajar per matkul (1, 2, 3, ...), ditentukan agent:
  ```yaml
  ---
  matkul: kalkulus
  topik: Limit
  tanggal: 2026-10-11
  minggu: 3              # opsional
  urutan: 3              # WAJIB: urutan belajar dalam matkul
  tags: [kalkulus, limit]
  status: draft          # draft | selesai
  sumber:
    - "kalkulus/resource/kampus/Modul-03-Limit.pdf"
  ---
  ```
- Struktur isi halaman materi: Ringkasan → Konsep kunci → Contoh (dengan perhitungan)
  → Keterkaitan (prasyarat: [[...]] | lanjutan: [[...]]).
- Link antar halaman pakai [[wikilink]], bukan path relatif.
- Rumus matematika pakai LaTeX: `$...$` inline, `$$...$$` display.
- Jangan membuat halaman baru untuk konsep yang sudah punya halaman — update yang ada.

## 4. Operasi

### Ingest (ada dokumen)
Perintahku mis. "ingest kalkulus/resource/kampus/Modul-03-Limit.pdf ke matkul kalkulus".
1. Ekstrak teks PDF pakai tool lokal (mis. `pdftotext` / PyMuPDF). Kalau PDF hasil
   scan (gambar), bilang terus terang butuh OCR — jangan mengarang isi.
2. Sampaikan takeaway penting ke aku SEBELUM menulis (satu-dua kalimat cukup).
   Usulkan nomor `urutan` belajar untuk halaman baru/berubah di sini.
3. Tulis halaman materi di `<matkul>/materi/<topik>.md` sesuai §3. Ingest TIDAK 1:1
   dengan PDF: satu PDF boleh dipecah jadi beberapa halaman topik; beberapa PDF
   bertopik sama digabung ke satu halaman yang sudah ada (jangan duplikat halaman).
   Kamulah yang memutuskan pemetaannya, lalu laporkan ke aku.
   Aturan `urutan`: nomor unik per matkul, mulai 1, tanpa lompat. Halaman baru
   disisipkan di posisi pedagogis yang benar (dasar → terapan); halaman sesudahnya
   digeser +1 (field `urutan` DAN rename file `<urutan> - <topik>.md`, plus
   perbarui semua [[wikilink]] yang menunjuk file yang digeser). Jangan tempel
   halaman baru selalu di akhir. Kalau ragu posisi, tanya aku — jangan asal nomor.
4. Update halaman terkait: prasyarat, topik lanjutan, halaman konsep yang disinggung.
   Satu sumber boleh menyentuh banyak halaman.
5. Update `index.md`, append ke `log.md` (lihat §5).

### Perintah / percakapan (tanpa dokumen)
Aku bisa minta mis. "buatkan ringkasan minggu 1 dari obrolan kita tadi". Tulis
langsung jadi halaman materi — dokumen bukan syarat.

### Query
Jawab pertanyaan dari wiki dulu: baca `index.md` → buka halaman relevan → sintesis
jawaban dengan sitasi [[halaman]]. Kalau info belum ada di wiki, bilang terus terang.

### Lint (berkala, saat aku minta "cek wiki")
Periksa: kontradiksi antar halaman, klaim basi, halaman yatim (tanpa inbound link —
cek dengan grep `[[nama-halaman]]`), konsep yang disebut tapi belum punya halaman,
link rusak. Laporkan temuan + usulan perbaikan; jangan ubah massal tanpa izin.

## 5. index.md dan log.md

- `index.md`: katalog per matkul — tiap halaman: NOMOR URUT, link, satu baris ringkasan, tanggal.
  Urutkan entri menaik sesuai `urutan`. Update SETIAP ingest (termasuk geser nomor).
- `log.md`: append-only. Format entri:
  `## [2026-10-11] ingest | kalkulus/resource/kampus/Modul-03-Limit.pdf → kalkulus/materi/limit.md`
  Jangan edit entri lama.

## 6. Batasan

- TIDAK PERNAH mengubah isi `resource/`.
- TIDAK membuat struktur di luar §2 tanpa izinku.
- Kalau ragu (PDF tidak terbaca, topik ambigu), tanya dulu — jangan mengarang.
