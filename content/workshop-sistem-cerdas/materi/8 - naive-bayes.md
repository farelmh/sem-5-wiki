---
matkul: workshop-sistem-cerdas
topik: Naive Bayes
urutan: 8
tanggal: 2026-10-12
tags: [workshop-sistem-cerdas, naive-bayes, klasifikasi, supervised, peluang-bersyarat, hmap]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WS_Sistem_Cerdas-Minggu_3_Kmeans_-_NB.pdf"
---

# Naive Bayes

## Ringkasan

Naive Bayes = klasifikasi supervised untuk data kategorikal (versi kontinu pakai Gaussian Naive Bayes). Fondasi: peluang bersyarat + teorema Bayes, disederhanakan lewat HMAP dengan asumsi independensi antar fitur. Supervised learning sendiri = belajar dari data berlabel (contoh: foto diberi tag "kucing"/"anjing"), output berupa klasifikasi (label) atau regresi (angka).

---

## Konsep Kunci

### 1. Peluang Bersyarat

$$P(X|Y) = \frac{P(X \cap Y)}{P(Y)}$$

Bacaan: proporsi $X$ di dalam $Y$.

### 2. Teorema Bayes + HMAP

Posterior dari prior:

$$P(X_k|Y) = \frac{P(Y|X_k)}{\sum_i P(Y|X_i)}$$

HMAP (Hypothesis Maximum A Priori Probability) — penyederhanaan Bayes yang dipakai ML: ambil hipotesis berprobabilitas posterior terbesar; penyebut $P(X)$ konstan sehingga bisa dibuang:

$$P(S|X) = \frac{\arg\max_{x} P(Y|X)\,P(X)}{P(X)} = \arg\max_{x} P(Y|X)\,P(X)$$

### 3. Asumsi Independensi ("Naive")

Fitur dianggap saling bebas bersyarat terhadap kelas, sehingga likelihood gabungan = produk likelihood tiap fitur:

$$P(X_1, \dots, X_n|Y) \propto P(Y) \prod_{i=1}^{n} P(X_i|Y)$$

Kelas pemenang = skor terbesar. Harga asumsi: abaikan korelasi antar fitur (tetap cukup akurat di praktik).

---

## Contoh (dengan perhitungan)

### A. Peluang bersyarat (6 baris olahraga)

| #   | Cuaca | Temperatur | Berolahraga |
| --- | ----- | ---------- | ----------- |
| 1   | Cerah | Normal     | Ya          |
| 2   | Cerah | Tinggi     | Ya          |
| 3   | Hujan | Tinggi     | Tidak       |
| 4   | Cerah | Tinggi     | Tidak       |
| 5   | Hujan | Normal     | Tidak       |
| 6   | Cerah | Normal     | Ya          |

$P(\text{Olahraga} = \text{Ya}) = 3/6$. $P(\text{cerah}, \text{normal} \mid \text{ya}) = (2/6)/(3/6) = 2/3$ (baris 1, 6).

### B. Keputusan dengan fitur angin (6 baris + Angin)

| #   | Cuaca | Temperatur | Kecepatan Angin | Berolahraga |
| --- | ----- | ---------- | --------------- | ----------- |
| 1   | Cerah | Normal     | Pelan           | Ya          |
| 2   | Cerah | Normal     | Pelan           | Ya          |
| 3   | Hujan | Tinggi     | Pelan           | Tidak       |
| 4   | Cerah | Normal     | Kencang         | Ya          |
| 5   | Hujan | Tinggi     | Kencang         | Tidak       |
| 6   | Cerah | Normal     | Pelan           | Ya          |

Fakta training: $P(\text{cerah}|\text{ya}) = 1$, $P(\text{cerah}|\text{tidak}) = 0$; $P(\text{kencang}|\text{ya}) = 1/4$, $P(\text{kencang}|\text{tidak}) = 1/2$; $P(\text{ya}) = 4/6$, $P(\text{tidak}) = 2/6$.

Tanya: cuaca cerah + angin kencang → olahraga?

- Skor Ya: $\{1 \cdot 1/4\} \cdot 4/6 = 1/6$.
- Skor Tidak: $\{0 \cdot 1/2\} \cdot 2/6 = 0$.

Keputusan: **Ya** (nol di pihak Tidak karena tak ada contoh cerah-tidak di training — contoh zero-probability).

### C. Latihan: kelulusan mahasiswa (15 train, 1 test)

Test: Perempuan, Bekerja, Belum menikah, IPK 3,50 → ?

Hitung kategorikal (Tepat = 8 baris, Terlambat = 7):

| $P(\cdot\|kelas)$ | Tepat (8) | Terlambat (7) |
|---|---|---|
| Perempuan | 3/8 | 4/7 |
| Bekerja | 3/8 | 4/7 |
| Belum | 4/8 | 4/7 |
| Prior | 8/15 | 7/15 |

- Skor Tepat: $(3/8)(3/8)(4/8)(8/15) = 9/128 \cdot 8/15 = 0{,}0375$.
- Skor Terlambat: $(4/7)^3(7/15) = 64/343 \cdot 7/15 \approx 0{,}0871$.

Dengan 3 fitur kategorikal: **Terlambat** menang. IPK (kontinu) tak bisa dihitung naive biasa — pakai Gaussian NB (mean IPK Tepat $\approx 3{,}37$ vs Terlambat $\approx 2{,}56$; $x = 3{,}50$ jauh lebih dekat ke Tepat) atau diskretisasi + Laplace smoothing bila frekuensi nol.

---

## Keterkaitan

- Prasyarat: [[5 - pra-pemrosesan-data|Pra-pemrosesan Data]] (encoding fitur kategorikal, tangani missing value sebelum training; train-test split untuk evaluasi akurasi/presisi/recall/F1).
- Lanjutan: bandingkan dengan [[7 - k-means-clustering|K-Means Clustering]] (unsupervised, tanpa label).
