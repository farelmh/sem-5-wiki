---
matkul: sistem-cerdas
topik: Pengantar Kecerdasan Buatan
urutan: 1
tanggal: 2026-10-12
minggu: 1
tags: [sistem-cerdas, ai, machine-learning, etika-ai]
status: selesai
sumber:
  - "sistem-cerdas/resource/kampus/Minggu_1_Pengantar_Kecerdasan_Buatan.pdf"
---

# Pengantar Kecerdasan Buatan

## Ringkasan

AI adalah cabang ilmu komputer yang membuat mesin mampu melakukan tugas yang biasanya butuh kecerdasan manusia: melihat, mendengar, memahami bahasa, bernalar, belajar dari pengalaman, lalu memutuskan dan bertindak untuk mencapai tujuan. AI tidak selalu robot fisik; banyak hadir sebagai perangkat lunak (rekomendasi, chatbot). Kerja AI = tujuan + data + model/aturan + komputasi. Hasil AI perlu dievaluasi manusia, terutama untuk keputusan berdampak besar.

---

## Konsep Kunci

### 1. Definisi AI

- AI meniru sebagian kemampuan kognitif manusia: terima informasi, olah data, kenali pola, susun prediksi, hasilkan keputusan/tindakan.
- Bukan sekadar program kaku; bisa belajar dan menyesuaikan keluaran dari data.

### 2. Enam Kemampuan Utama

- **Persepsi**: baca lingkungan via teks, gambar, suara, video, data sensor.
- **Pembelajaran**: temukan pola dalam data agar kinerja meningkat.
- **Penalaran**: hubungkan fakta, aturan, probabilitas untuk pilih solusi.
- **Bahasa**: pahami, ringkas, terjemahkan, hasilkan bahasa manusia.
- **Perencanaan**: susun urutan tindakan untuk capai target.
- **Aksi**: beri rekomendasi, kendalikan proses, gerakkan robot.

### 3. Ruang Lingkup AI

- **Machine Learning (ML)**: belajar dari contoh data untuk klasifikasi, prediksi, clustering.
- **Deep Learning (DL)**: bagian ML pakai jaringan saraf berlapis; efektif untuk data kompleks.
- **Natural Language Processing (NLP)**: pahami, analisis, hasilkan bahasa manusia.
- **Computer Vision (CV)**: kenali objek, wajah, teks, kejadian dari gambar/video.
- **Robotics**: gabungan AI + sensor + aktuator; rasakan lingkungan dan bertindak.
- **Sistem Pakar**: basis pengetahuan + aturan untuk saran pada domain sempit.

### 4. AI vs Otomasi Biasa

| Aspek | Otomasi biasa | AI |
|---|---|---|
| Cara kerja | Ikuti langkah/aturan tetap | Pakai data/model, kenali pola, sesuaikan keluaran |
| Contoh | Kirim email otomatis tiap Senin | Sarankan waktu kirim email paling mungkin dibuka |
| Perubahan | Butuh aturan baru bila kondisi berubah | Dilatih/diperbarui pakai data relevan |

- Praktik umum: AI buat prediksi/rekomendasi, otomasi jalankan tindakan rutin.

### 5. Sejarah Singkat

- **1950**: Alan Turing usul Turing Test untuk nilai kecerdasan mesin.
- **1956**: Istilah "Artificial Intelligence" populer di Dartmouth Workshop; awal formal bidang AI.
- **1960–1980**: Riset pemecahan masalah + sistem berbasis aturan; terhambat data dan komputasi.
- **1980-an**: Sistem pakar dipakai tiru pengetahuan spesialis domain sempit.
- **1990–2000-an**: ML naik karena data digital, algoritme, daya komputasi meningkat.
- **2010-an–kini**: Deep learning + AI generatif dorong lompatan visi, suara, bahasa, pembuatan konten.

### 6. Aplikasi di Berbagai Sektor

- **Kesehatan**: analisis citra medis, administrasi, pantau pasien, riset obat. Keputusan klinis tetap butuh tenaga profesional.
- **Pendidikan**: latihan adaptif, umpan balik, materi personal, asisten belajar.
- **Keuangan**: deteksi transaksi mencurigakan, kelola risiko, layanan pelanggan, analisis dokumen.
- **Transportasi**: navigasi, prediksi macet, optimasi rute, bantuan pengemudi.
- **Bisnis & layanan**: klasifikasi dokumen, peramalan permintaan, rekomendasi produk, chatbot.
- **Industri & pertanian**: deteksi cacat produk, pemeliharaan prediktif, pantau tanaman/mesin.

### 7. Contoh Sehari-hari

- **Rekomendasi konten**: video/belanja sarankan konten dari riwayat dan pola pengguna.
- **Peta digital**: estimasi waktu tempuh + rute dari data lalu lintas.
- **Penyaring spam**: nilai karakteristik pesan, pisahkan email mencurigakan.
- **Asisten virtual**: suara/chatbot pakai NLP untuk jawab pertanyaan.
- **Kamera ponsel**: pengenal wajah, mode malam, kelompok foto pakai CV.
- **Penerjemahan**: tebak padanan kalimat dari pola bahasa data latih.

### 8. Manfaat dan Keterbatasan

Manfaat:

- Olah informasi skala besar dengan cepat.
- Temukan pola sulit diamati manual.
- Konsisten pada tugas rutin berulang.
- Dukung kreativitas, analisis, pengambilan keputusan.

Keterbatasan/risiko:

- Hasil bisa keliru, tak lengkap, tak sesuai konteks.
- Bias data → keputusan tidak adil.
- Data pribadi/sensitif perlu dilindungi.
- Pengguna wajib cek sumber, fakta, dampak keputusan.

### 9. Prinsip Penggunaan Bertanggung Jawab

- **Manusia tetap mengawasi**: penilaian manusia untuk keputusan penting (kesehatan, pendidikan, keuangan, hak seseorang).
- **Lindungi data**: jangan masukkan data pribadi/rahasia ke sistem tanpa izin dan perlindungan memadai.
- **Periksa hasil**: verifikasi fakta, hitungan, sumber, konteks sebelum pakai keluaran AI.
- **Waspadai bias**: cek apakah hasil/data rugikan kelompok tertentu atau kuatkan stereotip.
- **Transparan**: jelaskan bila pekerjaan/keputusan dibantu AI, sesuai aturan berlaku.

---

## Contoh (dengan perhitungan)

Tidak ada perhitungan pada sumber ini. Contoh penalaran klasifikasi spam (ilustrasi konsep, bukan angka dari slide):

- Email masuk dengan kata "gratis", "menang", "klik link".
- Model nilai karakteristik → skor spam $0{,}85$ > ambang $0{,}5$ → masuk folder spam.
- Inti: persepsi (baca teks) → pembelajaran (pola dari data latih) → penalaran (banding ambang) → aksi (pisahkan email).

---

## Keterkaitan

- Prasyarat: tidak ada (halaman pertama matkul ini).
- Lanjutan: materi Pertemuan 4 (belum di-ingest; PDF hasil scan, butuh OCR).
