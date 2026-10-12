---
matkul: workshop-sistem-cerdas
topik: K-Means Clustering
urutan: 7
tanggal: 2026-10-12
tags: [workshop-sistem-cerdas, k-means, clustering, unsupervised, euclidean, elbow, wcss]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WS_Sistem_Cerdas-Minggu_3_Kmeans_-_NB.pdf"
---

# K-Means Clustering

## Ringkasan

K-Means = clustering non-hierarchical (partitional), unsupervised: mengelompokkan data tanpa label berdasar kemiripan, minimalkan variasi dalam klaster dan maksimalkan variasi antar klaster. Tantangan utama: nilai $K$ harus ditentukan di awal (dibantu Elbow Method via WCSS).

---

## Konsep Kunci

### 1. Klasifikasi vs Klastering

| Aspek | Klasifikasi | Klastering |
|---|---|---|
| Pembelajaran | Supervised (data berlabel) | Unsupervised (tanpa label) |
| Tujuan | Prediksi kelas data baru | Kelompokkan data berdasar kemiripan |
| Proses | Latih dari data berlabel → uji | Kelompokkan langsung via jarak/kemiripan |
| Output | Label/kelas prediksi | Nomor klaster tiap data |
| Evaluasi | Akurasi, presisi, recall, F1 (vs label asli) | WCSS, Silhouette Score (tanpa label) |
| Algoritma | Naive Bayes, Decision Tree, KNN, SVM, Neural Network | K-Means, Hierarchical, DBSCAN |

### 2. Langkah Algoritma K-Means

a) Tentukan jumlah klaster $K$; b) alokasi data ke klaster acak; c) hitung centroid (rata-rata) tiap klaster; d) alokasikan tiap data ke centroid terdekat; e) ulangi c–d sampai tak ada data pindah, perubahan centroid di bawah threshold, atau objective function konvergen.

### 3. Euclidean Distance

Jarak $L2$ dua titik, metrik kemiripan utama K-Means:

$$d(A, B) = \sqrt{\sum_{i=1}^{n} (A_i - B_i)^2}$$

Untuk 2 dimensi:

$$d(A, B) = \sqrt{(A_x - B_x)^2 + (A_y - B_y)^2}$$

### 4. Elbow Method + WCSS

Jalankan K-Means untuk $K = 1$ s.d. $10$, hitung WCSS (Within-Cluster Sum of Squares / inertia) tiap $K$:

$$WCSS = \sum_{k=1}^{K} \sum_{x \in C_k} \|x - \mu_k\|^2$$

$K$ optimal = titik penurunan WCSS mulai melandai seperti siku (elbow).

---

## Contoh (dengan perhitungan)

Clustering bunga Iris (fitur SL = sepal length, SW = sepal width), $K = 2$. Centroid awal acak: $C_1 = (4{,}5;\ 3{,}5)$, $C_2 = (5{,}5;\ 2{,}6)$.

| No | SL | SW | Species |
|---|---|---|---|
| 1 | 5 | 3,5 | Setosa |
| 2 | 4,5 | 2,3 | Setosa |
| 3 | 4,4 | 3,2 | Setosa |
| 4 | 5 | 3,5 | Setosa |
| 5 | 5,1 | 3,8 | Setosa |
| 6 | 5,5 | 2,6 | Versicolor |
| 7 | 6,1 | 3 | Versicolor |
| 8 | 5,8 | 2,6 | Versicolor |
| 9 | 5 | 2,3 | Versicolor |
| 10 | 5,6 | 2,7 | Versicolor |

Data 1: $d(1,1) = \sqrt{(5-4{,}5)^2 + (3{,}5-3{,}5)^2} = 0{,}5$; $d(1,2) = \sqrt{(5-5{,}5)^2 + (3{,}5-2{,}6)^2} \approx 1{,}03$ → $C_1$. Hasil iterasi 1 (terhitung):

| No | d ke $C_1$ | d ke $C_2$ | Klaster |
|---|---|---|---|
| 1 | 0,5 | 1,03 | $C_1$ |
| 2 | 1,2 | 1,044 | $C_2$ |
| 3 | 0,316 | 1,253 | $C_1$ |
| 4 | 0,5 | 1,03 | $C_1$ |
| 5 | 0,671 | 1,265 | $C_1$ |
| 6 | 1,345 | 0 | $C_2$ |
| 7 | 1,676 | 0,721 | $C_2$ |
| 8 | 1,581 | 0,3 | $C_2$ |
| 9 | 1,3 | 0,583 | $C_2$ |
| 10 | 1,36 | 0,141 | $C_2$ |

Catatan: data 2 (label asli Setosa) jatuh ke $C_2$ — wajar, K-Means ikut jarak bukan label.

Update centroid dari anggota klaster: $C_1 = \{1,3,4,5\}$ → $SL = (5+4{,}4+5+5{,}1)/4 = 4{,}875$, $SW = (3{,}5+3{,}2+3{,}5+3{,}8)/4 = 3{,}5$. $C_2 = \{2,6,7,8,9,10\}$ → $SL = 32{,}5/6 \approx 5{,}417$, $SW = 15{,}5/6 \approx 2{,}583$. Ulangi hitung jarak; bila hasil klaster iterasi 2 sama dengan iterasi 1 → konvergen, tetapkan hasil akhir.

---

## Keterkaitan

- Prasyarat: [[5 - pra-pemrosesan-data|Pra-pemrosesan Data]] (fitur numerik + scaling sebelum hitung jarak; outlier menggeser centroid).
- Lanjutan: [[8 - naive-bayes|Naive Bayes]] (lawan supervised-nya: klasifikasi berlabel).
