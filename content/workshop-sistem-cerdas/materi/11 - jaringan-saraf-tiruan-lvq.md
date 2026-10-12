---
matkul: workshop-sistem-cerdas
topik: Jaringan Saraf Tiruan LVQ
urutan: 11
tanggal: 2026-10-12
tags: [workshop-sistem-cerdas, jst, lvq, klasifikasi]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas_JST__Prediksi.pdf"
---

# Jaringan Saraf Tiruan LVQ

## Ringkasan

Jaringan Saraf Tiruan (JST/ANN) adalah sistem pemroses informasi yang meniru cara kerja otak manusia, umum dipakai untuk klasifikasi: belajar dari data masa lalu lalu memberi keputusan atas data yang belum pernah dilihat. LVQ (Learning Vector Quantization) adalah salah satu jenis JST untuk klasifikasi pola — tiap unit output mewakili satu kategori, dan keputusan diambil dari prototype (vektor bobot) yang jaraknya terdekat ke vektor masukan. Selama pembelajaran terawasi, prototype pemenang (dan runner-up bila jaraknya hampir sama) digeser mendekat bila benar atau dijauhkan bila salah.

![[Excalidraw/arsitektur-lvq-prototype.excalidraw]]

---

## Konsep Kunci

### 1. Tiga lapis JST

1. **Lapis masukan (input layer)**: neuron penerima data input variabel X; bisa terhubung ke hidden layer atau langsung ke output bila tanpa hidden layer.
2. **Lapis tersembunyi (hidden layer)**: neuron penerima data dari lapis masukan.
3. **Lapis keluaran (output layer)**: neuron penerima data dari hidden (atau langsung dari input); nilai output melambangkan hasil kalkulasi X menjadi Y.

### 2. Jenis-jenis JST (dalam BKPM)

1. Perceptron, 2. Backpropagation (propagasi balik), 3. Learning Vector Quantization (LVQ), 4. dan lain-lain.

### 3. LVQ: prototype, winner, runner-up

- Tiap unit output mewakili satu kategori/kelompok.
- Pemrosesan tiap neuron: cari **jarak terdekat** antara vektor masukan dan vektor bobot (prototype) yang bersangkutan.
- Kelebihan: selain jarak terdekat, unit output diposisikan ulang lewat update bobot terawasi untuk mempertajam keputusan klasifikasi.
- Ide pengembangan algoritma: bila input berjarak hampir sama ke vektor **pemenang (winner)** dan **runner-up**, maka keduanya harus belajar (keduanya di-update).
- Catatan sumber: BKPM merujuk langkah algoritma pelatihan LVQ menurut **Fausett (1994)** dan menampilkan gambar step-by-step serta arsitektur, tetapi **teks hasil ekstraksi tidak memuat isi langkah-langkahnya** (halaman gambar), jadi detail iterasi per epoch tidak dikutip di sini.

### 4. Arsitektur + alur training vs testing

- Arsitektur: vektor input $x = (x_1, x_2, x_3, x_4)$ → dihitung jarak Euclidean ke tiap prototype $W_j$ → unit dengan $||x - W_j||$ minimum menang → kelas = kelas prototype pemenang.
- **Training** (terawasi, contoh BKPM: epoch 1, $\alpha = 0{,}1$, 2 kelas C1/C2): untuk tiap data latih, cari winner; bila kelas winner = kelas data, dekatkan prototype ($W \mathrel{+}= \alpha(x - W)$); bila salah, jauhkan ($W \mathrel{-}= \alpha(x - W)$).
- **Testing**: hitung jarak ke semua prototype final, pilih yang terdekat — tanpa update bobot.

---

## Contoh (dengan perhitungan)

### Data latih (10 baris Iris, dari BKPM)

| No | X1 | X2 | X3 | X4 | Kelas |
|----|-----|-----|-----|-----|---------------|
| 1 | 5,1 | 3,5 | 1,4 | 0,2 | Iris-setosa (C1) |
| 2 | 4,9 | 3,0 | 1,4 | 0,2 | Iris-setosa (C1) |
| 3 | 4,7 | 3,2 | 1,3 | 0,2 | Iris-setosa (C1) |
| 4 | 4,6 | 3,1 | 1,5 | 0,2 | Iris-setosa (C1) |
| 5 | 5,0 | 3,6 | 1,4 | 0,2 | Iris-setosa (C1) |
| 6 | 7,0 | 3,2 | 4,7 | 1,4 | Iris-versicolor (C2) |
| 7 | 6,4 | 3,2 | 4,5 | 1,5 | Iris-versicolor (C2) |
| 8 | 6,9 | 3,1 | 4,9 | 1,5 | Iris-versicolor (C2) |
| 9 | 5,5 | 2,3 | 4,0 | 1,3 | Iris-versicolor (C2) |
| 10 | 6,5 | 2,8 | 4,6 | 1,5 | Iris-versicolor (C2) |

### Data uji

| No | X1 | X2 | X3 | X4 | Kelas |
|----|-----|-----|-----|-----|-------|
| A | 5,6 | 2,5 | 3,9 | 1,1 | ?? |
| B | 5,1 | 3,8 | 1,6 | 0,2 | ?? |

### Cara mengerjakan (tugas training + testing)

1. Inisialisasi 2 prototype (misal $W_1$ dari data setosa, $W_2$ dari data versicolor), $\alpha = 0{,}1$, epoch = 1.
2. Tiap data latih: hitung $d_j = ||x - W_j|| = \sqrt{\sum_i (x_i - W_{ji})^2}$ untuk $j = 1, 2$; winner = jarak minimum.
3. Update winner: benar → $W_{baru} = W_{lama} + 0{,}1(x - W_{lama})$; salah → $W_{baru} = W_{lama} - 0{,}1(x - W_{lama})$.
4. Setelah epoch selesai, uji A dan B: hitung jarak ke $W_1, W_2$ final. Secara intuitif (ciri petal kecil vs besar): **B** (petal 1,6/0,2) dekat ke setosa → C1; **A** (petal 3,9/1,1) dekat ke versicolor → C2. Kepastian menunggu hitungan dengan bobot hasil training.

---

## Keterkaitan

- Mandiri: tidak bergantung pada halaman smoothing; keduanya dari satu sumber tetapi topik terpisah (klasifikasi vs prediksi deret waktu).
- Lanjutan: [[workshop-sistem-cerdas/materi/10 - support-vector-machine|Support Vector Machine]] (metode klasifikasi lain, terawasi).
