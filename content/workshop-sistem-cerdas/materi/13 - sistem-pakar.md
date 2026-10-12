---
matkul: workshop-sistem-cerdas
topik: Sistem Pakar
urutan: 13
tanggal: 2026-10-12
tags: [workshop-sistem-cerdas, sistem-pakar, inference-engine, forward-chaining, backward-chaining]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas_Sistem_Pakar.pdf"
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas_Sistem_Pakar_dan_NLP.pdf"
---

# Sistem Pakar

## Ringkasan

Sistem pakar (Expert System, ES) adalah sistem berbasis AI yang memanfaatkan pengetahuan dan pengalaman seorang pakar untuk membantu diagnosis, konsultasi, dan pengambilan keputusan. Cikal bakalnya adalah General Purpose Problem Solver (GPS, Newell–Shaw–Simon) dari Logic Theorist — prosedur menyusun langkah dari keadaan awal ke state tujuan. Penalaran dilakukan mesin inferensi memakai fakta + aturan (forward atau backward chaining); ketidakpastian ditangani CF, fuzzy, Bayesian, atau Dempster–Shafer (lihat [[dempster-shafer]]).

![[Excalidraw/arsitektur-sistem-pakar.excalidraw]]

---

## Konsep Kunci

### 1. Sejarah singkat

- Pertengahan 1960-an, Artificial Intelligence Corporation: keyakinan nalar + komputer canggih → prestasi setara pakar.
- GPS (Allen Newell, Cliff Shaw, Herbert Simon) dikembangkan dari Logic Theorist; predecessor ES — menyusun langkah mengubah situasi awal menjadi state tujuan.

### 2. Tujuan sistem pakar

Membantu pengguna mengambil keputusan; konsultasi seperti pakar; menyimpan pengetahuan pakar; membantu diagnosis; mengurangi ketergantungan pada pakar manusia; membantu pembelajaran/pelatihan; rekomendasi berbasis pengetahuan.

### 3. Struktur: 7 komponen

1. **Penambahan pengetahuan** (knowledge acquisition): memasukkan/membangun/memperluas pengetahuan dari ahli, buku, gambar.
2. **Basis pengetahuan**: pengetahuan untuk memahami, memformulasi, menyelesaikan masalah.
3. **Mesin inferensi**: program metodologi penalaran atas basis pengetahuan + blackboard, memformulasi konklusi.
4. **Blackboard**: area memori untuk kejadian berjalan termasuk keputusan sementara.
5. **Antarmuka**: media komunikasi user–program.
6. **Fasilitas penjelasan**: melacak respons, memberi penjelasan interaktif.
7. **Perbaikan pengetahuan**: evaluasi kinerja — apakah pengetahuan masih cocok untuk masa mendatang.

### 4. Inference engine

Tugas: baca fakta → cocokkan fakta dengan aturan → tentukan aturan terpenuhi → hasilkan kesimpulan. Skema umum: Fakta + Rule → Proses Inferensi → Kesimpulan.

### 5. Forward chaining vs backward chaining

| Aspek | Forward Chaining | Backward Chaining |
|---|---|---|
| Arah | fakta → kesimpulan (data-driven) | tujuan → fakta pendukung (goal-driven) |
| Cocok untuk | diagnosis, monitoring, prediksi, banyak fakta awal | konsultasi, diagnosis, troubleshooting, tujuan tertentu |
| Skema | FAKTA → RULE → KESIMPULAN | TUJUAN → RULE → FAKTA |

**Kapan pilih:** fakta awal melimpah dan kesimpulan belum diketahui → forward; hipotesis/tujuan sudah ada dan perlu dibuktikan → backward.

### 6. Inferensi vs ketidakpastian (jangan tertukar)

Forward/backward chaining = strategi penelusuran inferensi. CF, fuzzy logic, Bayesian, Dempster–Shafer = representasi/pengolahan ketidakpastian yang dikombinasikan dengan strategi tersebut.

---

## Contoh (dengan perhitungan)

Fakta: G01 = laptop tidak menyala, G02 = lampu indikator mati.
Rule: IF G01 AND G02 THEN power supply bermasalah.

- **Forward:** sistem terima fakta G01 + G02 → rule terpenuhi → tampilkan "Power Supply Bermasalah".
- **Backward:** tujuan "apakah power supply bermasalah?" → sistem tanya "laptop tidak menyala?" (ya) → "lampu indikator mati?" (ya) → simpulkan "Power Supply Bermasalah".

---

## Keterkaitan

- Prasyarat: —
- Lanjutan: [[dempster-shafer]] (ketidakpastian pada sistem pakar)
