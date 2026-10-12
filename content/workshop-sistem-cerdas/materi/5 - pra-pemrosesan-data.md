---
matkul: workshop-sistem-cerdas
topik: Pra-pemrosesan Data
urutan: 5
tanggal: 2026-10-12
tags: [workshop-sistem-cerdas, pra-pemrosesan-data, cleaning, encoding, scaling, train-test-split]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WS_Sistem_Cerdas-Minggu_3_Kmeans_-_NB.pdf"
---

# Pra-pemrosesan Data

## Ringkasan

Pra-pemrosesan data (data preprocessing) adalah tahap dalam Knowledge Discovery in Database (KDD) sebelum data dipakai membangun model Machine Learning. Data dunia nyata umumnya mentah: tidak konsisten, ada nilai kosong, duplikasi, outlier. Prinsipnya **"garbage in, garbage out"** — kualitas input menentukan kualitas prediksi. Empat tahap: data cleaning, integrasi, transformasi, reduksi.

---

## Konsep Kunci

### 1. Data Cleaning

- **Missing value** — dua strategi:
  - *Deletion*: hapus baris/kolom kosong; cocok bila data hilang sedikit.
  - *Imputation*: isi dengan mean, median, modus, atau prediksi dari atribut lain.
- **Data duplikat** — hapus agar tidak membiaskan model.
- **Noise/outlier** — data pencilan ditangani dulu (detail: [[6 - outlier-detection]]).

### 2. Data Integration

Menggabungkan data dari beberapa sumber menjadi satu kesatuan yang konsisten (skema, satuan, format diseragamkan).

### 3. Encoding Data Kategorikal

Kebanyakan algoritma ML hanya makan data numerik, jadi atribut kategorikal harus di-encode:

- **Label Encoding**: tiap kategori → angka (0, 1, 2, ...). Cocok untuk **ordinal** (ada urutan, mis. SD < SMP < SMA).
- **One-Hot Encoding**: tiap kategori → kolom biner baru (0/1). Cocok untuk **nominal** (tanpa urutan, mis. jenis kelamin, jenis produk). Hindari label encoding di data nominal karena angka menimbulkan urutan palsu.

### 4. Normalisasi dan Standardisasi

Atribut numerik beda skala bisa saling mendominasi saat training. Dua cara:

- **Min-Max Scaling** — tekan data ke rentang tertentu (umumnya 0–1):

$$x' = \frac{x - \min(x)}{\max(x) - \min(x)}$$

- **Standardisasi (Z-score)** — buat data ber-rata-rata 0, standar deviasi 1:

$$z = \frac{x - \mu}{\sigma}$$

Min-max cocok bila butuh rentang terbatas (mis. input neural network); z-score cocok bila data mendekati normal atau algoritma berbasis jarak/varians.

### 5. Pembagian Data (Data Splitting)

Data bersih dibagi jadi **training set** (bangun model) dan **testing set** (evaluasi ke data yang belum pernah dilihat), proporsi mis. 80:20 atau 70:30.

### 6. Data Reduction

Sederhanakan jumlah atribut/data (mis. seleksi fitur) tanpa buang informasi penting.

---

## Contoh (dengan perhitungan)

Dataset pelanggan: atribut usia, pendapatan (juta Rp), jenis kelamin, status berlangganan. Ada nilai kosong di usia/pendapatan, 1 baris duplikat, 1 pendapatan outlier (mis. 500 juta vs mayoritas 5–15 juta).

Langkah pra-pemrosesan (Python: Pandas, NumPy, Scikit-learn):

1. **Eksplorasi**: jumlah baris/kolom, tipe data, jumlah missing value per kolom.
2. **Cleaning**: imputasi usia kosong dengan median, hapus baris duplikat, tandai outlier pendapatan via IQR/Z-score.
3. **Transformasi**:
   - Jenis kelamin (nominal) → one-hot: kolom `JK_L`, `JK_P` berisi 0/1.
   - Contoh min-max: pendapatan $x = 10$, $\min = 5$, $\max = 15$ → $x' = (10-5)/(15-5) = 0{,}5$.
   - Contoh z-score: usia $x = 40$, $\mu = 30$, $\sigma = 5$ → $z = (40-30)/5 = 2$.
4. **Split**: 80% latih, 20% uji.

---

## Keterkaitan

- Prasyarat: tidak ada (fondasi awal alur ML).
- Lanjutan: [[6 - outlier-detection|Outlier Detection]] (pendalaman tahap cleaning), [[7 - k-means-clustering|K-Means Clustering]], [[8 - naive-bayes|Naive Bayes]] (model yang memakai data hasil pra-pemrosesan).
