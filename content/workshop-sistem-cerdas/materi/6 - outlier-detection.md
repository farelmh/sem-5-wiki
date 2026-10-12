---
matkul: workshop-sistem-cerdas
topik: Outlier Detection
urutan: 6
tanggal: 2026-10-12
tags: [workshop-sistem-cerdas, outlier, z-score, iqr, isolation-forest, boxplot]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_M3_AP.pdf"
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WS_Sistem_Cerdas-Minggu_3_Kmeans_-_NB.pdf"
---

# Outlier Detection

## Ringkasan

Outlier = data yang nilainya menyimpang signifikan dari mayoritas (contoh: tinggi mahasiswa 160–180 cm, satu data 250 cm). Outlier Detection membersihkan error/noise dan menaikkan performa model — tapi outlier kadang justru informasi penting (fraud, penyakit langka). Penyebab: measurement error, human error, variasi alami ekstrem, kondisi langka. Dampak: statistik (mean, std) bergeser, model tidak akurat. Tidak ada satu metode yang "paling benar" — pilihan tergantung konteks dataset.

![[Excalidraw/outlier-boxplot-isolation.excalidraw]]

---

## Konsep Kunci

### 1. Taksonomi 4 Metode

1. **Statistical** — pakai distribusi statistik (Z-Score, IQR).
2. **Distance-Based** — ukur jarak antar data (KNN, Mahalanobis Distance).
3. **Density-Based** — ukur kepadatan (Local Outlier Factor / LOF).
4. **Machine Learning** — model deteksi (Isolation Forest, One-Class SVM, DBSCAN).

Visual bantu: boxplot, scatterplot, histogram, heatmap korelasi.

### 2. Z-Score

Ukur jarak nilai ke rata-rata dalam satuan standar deviasi:

$$Z = \frac{X - \mu}{\sigma}$$

Aturan: $|Z| > 3$ → outlier. Contoh: $\mu = 70$, $\sigma = 10$, nilai $110$ → $Z = (110-70)/10 = 4$ → outlier.

### 3. IQR (Interquartile Range)

Langkah: hitung Q1 (25%) dan Q3 (75%) → $IQR = Q3 - Q1$ → batas:

$$\text{Lower} = Q1 - 1{,}5 \times IQR \qquad \text{Upper} = Q3 + 1{,}5 \times IQR$$

Data di luar $[\text{Lower}, \text{Upper}]$ → outlier. Sering dipakai boxplot. Kelebihan: tidak sensitif ke distribusi non-normal. Batas bisa diperketat ke $3 \times IQR$.

### 4. Isolation Forest

Pohon biner dengan split acak pada fitur. Prinsip: data normal butuh **banyak** split untuk terisolasi; outlier butuh **sedikit** split (cepat terpisah karena jauh dari kerumunan). Cocok untuk dataset besar dan berdimensi tinggi; tanpa asumsi distribusi.

Skor anomali dari rata-rata panjang path $E[h(x)]$:

$$s(x, n) = 2^{-\frac{E[h(x)]}{c(n)}}$$

$c(n)$ = normalisasi panjang path rata-rata pohon biner acak ukuran $n$. Interpretasi: $s \approx 1$ → outlier; $s \approx 0{,}5$ → ambigu; $s \approx 0{,}2$–$0{,}4$ → normal. (Di sklearn tanda dibalik: skor negatif → outlier, positif → normal.)

Intuisi contoh: dataset $X = [9, 10, 11, 12, 13, 200]$, root split acak $= 50$ → kanan $= [200]$ langsung terisolasi (path $= 1$), kiri masih 5 data butuh split berlapis (path 3–4). Jadi 200 dapat skor $\approx 1$, sisanya $\approx 0{,}2$–$0{,}4$.

### 5. Threshold Tiap Metode

- Z-score: ambang $3{,}0$ (bisa diubah).
- IQR: $1{,}5 \times IQR$ (bisa diperketat $3 \times IQR$).
- Isolation Forest: parameter **contamination** = proporsi data yang dipaksa jadi outlier (mis. 0,1 = 10%).

### 6. Tabel Perbandingan

| Metode | Konsep dasar | Kelebihan | Kekurangan | Cocok untuk |
|---|---|---|---|---|
| Z-Score | Jarak ke rata-rata (satuan std); outlier bila $\|Z\| > 3$ | Mudah dipahami & diimplementasikan; cepat, dataset kecil | Kurang efektif bila data sangat besar/kompleks | Data berdistribusi normal |
| IQR | Rentang kuartil; outlier di luar $Q1 \pm 1{,}5 \times IQR$ | Tak bergantung distribusi normal; sensitif ke ekor distribusi; sederhana & populer | Bisa terlalu ketat untuk dataset kecil | Distribusi miring, data ordinal/rasio |
| Isolation Forest | Isolasi via pohon acak; outlier = cepat terisolasi | Dataset besar & multidimensi; tanpa asumsi distribusi; fleksibel | Lebih kompleks dijelaskan ke pemula; hasil dipengaruhi contamination; "memaksa" jumlah outlier | Data besar, high-dimensional, real-world (fraud, anomali sensor) |

---

## Contoh (dengan perhitungan)

### A. Dataset X1 (10 baris, BKPM M3 AP)

| ID | X1 | X2 | Kelas |
|---|---|---|---|
| 1 | 10 | 12 | A |
| 2 | 11 | 15 | A |
| 3 | 9 | 14 | A |
| 4 | 10 | 13 | A |
| 5 | 12 | 16 | A |
| 6 | 200 | 220 | A |
| 7 | 30 | 28 | B |
| 8 | 32 | 29 | B |
| 9 | 28 | 31 | B |
| 10 | 29 | 27 | B |

**Z-score** ($\mu = 37{,}1$, $\sigma \approx 57{,}38$):

| ID | X1 | Z | ID | X1 | Z |
|---|---|---|---|---|---|
| 1 | 10 | −0,47 | 6 | 200 | **+2,84** |
| 2 | 11 | −0,45 | 7 | 30 | −0,12 |
| 3 | 9 | −0,49 | 8 | 32 | −0,09 |
| 4 | 10 | −0,47 | 9 | 28 | −0,16 |
| 5 | 12 | −0,44 | 10 | 29 | −0,14 |

ID 6: $Z = (200 - 37{,}1)/57{,}38 \approx 2{,}84$ — di bawah ambang 3 tapi jelas menyimpang; Z-score hampir meleset karena outlier sendiri menggelembungkan $\mu$ dan $\sigma$.

**IQR**: urutkan X1 $= [9, 10, 10, 11, 12, 28, 29, 30, 32, 200]$ → median bawah $[9,10,10,11,12]$ → $Q1 = 10$; median atas $[28,29,30,32,200]$ → $Q3 = 30$; $IQR = 20$. Batas bawah $= 10 - 1{,}5 \times 20 = -21{,}5$; batas atas $= 30 + 1{,}5 \times 20 = 60$. ID 1–5 dan 7–10 normal; ID 6 (200 > 60) outlier.

**Isolation Forest**: ID 6 $(200, 220)$ jauh dari kerumunan 9–32 → terisolasi dalam 1–2 split, skor $\approx 1$; data lain $\approx 0{,}2$–$0{,}4$.

### B. Dataset Nilai (perbandingan 3 metode)

- **Z-score** ($|Z| > 3): 5 outlier (12, 29, 22, 25, 12). Nilai 125 ($Z = 2{,}52 < 3$) dan 53 lolos — distribusi lebar menutupi outlier atas.
- **IQR**: 7 outlier (12, 29, 22, 25, 12, **125, 53**) — lebih sensitif ke ekor distribusi.
- **Isolation Forest** (contamination $= 0{,}1$ → 10% data ≈ 10 baris): menandai 12, 29, 22, 12, 125, 53 (+ sisanya). 125 tetap kena meski lolos Z-score karena mudah diisolasi.

---

## Keterkaitan

- Prasyarat: [[5 - pra-pemrosesan-data|Pra-pemrosesan Data]] (outlier detection = bagian data cleaning).
- Lanjutan: [[7 - k-means-clustering|K-Means Clustering]], [[8 - naive-bayes|Naive Bayes]] (model dilatih setelah data bersih).
