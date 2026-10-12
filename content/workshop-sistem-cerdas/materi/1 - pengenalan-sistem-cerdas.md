---
matkul: workshop-sistem-cerdas
topik: Pengenalan Sistem Cerdas
urutan: 1
tanggal: 2026-10-12
minggu: 1
tags: [workshop-sistem-cerdas, sistem-cerdas, kecerdasan-buatan, turing-test, heuristik]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas-Minggu_1.pdf"
---

# Pengenalan Sistem Cerdas

## Ringkasan

Sistem cerdas meniru perilaku cerdas manusia lewat komputer: belajar dari pengalaman, paham pesan ambigu, respons cepat ke situasi baru, bernalar pecahkan masalah. Mata kuliah praktik (0-4 SKS); minggu 1 fondasi definisi, tujuan, bidang, konsep inti (Turing, simbolik, heuristik, inferensi, pattern matching), 10 penerapan sehari-hari, kebutuhan data, target proyek akhir.

---

## Konsep Kunci

### 1. Definisi menurut ahli

- **Minsky (1989)**: ilmu membuat komputer lakukan hal seperti manusia.
- **Simon (1987)**: kawasan riset, aplikasi, instruksi terkait pemrograman komputer lakukan hal yang dalam pandangan manusia cerdas.
- **Rich dan Knight (1991)**: studi cara membuat komputer lakukan hal yang saat ini lebih baik dikerjakan manusia.

### 2. Tiga tujuan AI

Buat komputer lebih cerdas, pahami kecerdasan, buat mesin lebih berguna. Kecerdasan = kemampuan belajar dari pengalaman, pahami pesan kontradiktif/ambigu, respons cepat ke situasi baru, bernalar pecahkan masalah efektif (Winston dan Prendergast, 1994).

### 3. Bidang penelitian

1. **Formal tasks**: matematika, games.
2. **Mundane tasks**: perception, robotics, natural language, common sense, reasoning.
3. **Expert tasks**: financial analysis, medical diagnostics, engineering, scientific analysis.

### 4. AI vs pemrograman konvensional

Konvensional: algoritmik, numerik, input-proses-output pasti. AI: simbolik dan non-algoritmik, toleran ambiguitas, cari solusi lewat search + heuristik + inferensi, butuh data sebagai bahan belajar.

### 5. Lima konsep inti

- **Turing Test**: penanya (manusia) bedakan jawaban manusia vs mesin. Tak bisa bedakan → mesin diasumsikan cerdas.
- **Pemrosesan simbolik**: komputer dirancang numerik; manusia berpikir simbolik. AI proses simbolik/non-algoritmik.
- **Heuristik**: strategi search ruang problem selektif, pandu pencarian di jalur kemungkinan sukses terbesar. Biaya search turun dari eksplorasi buta $O(b^d)$ ke terarah.
- **Inferensi**: penarikan kesimpulan dari fakta + aturan pakai heuristik/search. Bentuk: fakta $P$, aturan $P \rightarrow Q$, simpulkan $Q$.
- **Pattern matching**: jelaskan objek/event/proses dalam relasi logik/komputasional; mesin kerja dengan cocokkan pola input ke pola tersimpan.

### 6. Sepuluh penerapan sehari-hari

1. **Streaming video/musik**: rekomendasi dari riwayat, kualitas adaptif sinyal.
2. **Search engine**: pelajari kebiasaan pengguna, personalisasi hasil.
3. **Fitur selfie**: deteksi wajah, filter, beauty, aging.
4. **GPS**: rute tercepat hindari macet, peta 3D real, data mobil kamera 360°.
5. **Ojek online**: matching driver acak berkriteria, layanan antar/makanan, anti-fraud.
6. **Video game**: NPC baca teknik pemain, sesuaikan level lawan, grafis 3D.
7. **Internet marketing**: iklan tersegmentasi otomatis dari rekam jejak.
8. **Media sosial**: saran pertemanan dari kontak, kurasi feed foto/video.
9. **Translator**: voice record dan kamera translate real-time.
10. **Belanja online**: rekomendasi produk di beranda dari kebiasaan cari/beli.

---

## Contoh (dengan perhitungan)

Heuristik pangkas search: ruang problem faktor cabang $b = 3$, kedalaman $d = 4$. Blind search kunjungi sampai $1 + 3 + 9 + 27 + 81 = 121$ simpul. Heuristik bagus hanya ikuti 1 cabang tiap level: $1 + 1 + 1 + 1 + 1 = 5$ simpul. Penghematan $\frac{121-5}{121} \approx 96\%$.

---

## Keterkaitan

- Prasyarat: [[sistem-cerdas/materi/1 - pengantar-kecerdasan-buatan|Pengantar Kecerdasan Buatan]]
- Lanjutan: [[workshop-sistem-cerdas/materi/2 - representasi-pengetahuan|Representasi Pengetahuan]]
