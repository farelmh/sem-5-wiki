---
matkul: workshop-sistem-cerdas
topik: Support Vector Machine
urutan: 10
tanggal: 2026-10-12
tags: [workshop-sistem-cerdas, svm, klasifikasi, supervised-learning, kernel]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas__SVM__Decision_Tree_Rev.pdf"
---

# Support Vector Machine

## Ringkasan

Support Vector Machine (SVM) adalah algoritma supervised learning untuk klasifikasi (dan regresi) yang dikembangkan Guyon, Boser, Vapnik dan diperkenalkan 1992. Prinsipnya: Structural Risk Minimization (SRM) — cari hyperplane terbaik (margin terbesar) yang memisahkan dua kelas; berupa linear classifier yang diperluas lewat kernel trick agar mampu menangani data non-linear. SVM dirancang untuk klasifikasi biner ($y_i \in \{+1, -1\}$); untuk multi-kelas dipakai strategi One-vs-One atau One-vs-Rest.

---

## Konsep Kunci

### 1. SRM dan hyperplane

- Teori berangkat dari kasus linear separable: data $\{x_1, \dots, x_n\}$ dengan label $y_i \in \{+1, -1\}$; kelas data uji $x$ ditentukan lewat fungsi keputusan.
- Tujuan SRM: hyperplane yang memisahkan ruang input dua kelas dengan generalisasi terbaik (margin maksimal, bukan sekadar memisahkan).
- Persamaan hyperplane: $$w \cdot x + b = 0$$
- Fungsi keputusan: $$f(x) = \sum_{i=1,\,x_i \in SV}^{n} \alpha_i y_i K(x, x_i) + b$$
- Optimasi (kasus linear): minimalkan $$\min \frac{1}{2}\|w\|^2 = \frac{1}{2}(w_1^2 + w_2^2)$$ dengan syarat $$y_i(w \cdot x_i + b) \geq 1$$ untuk semua data latih.

### 2. Kernel trick

- Bila data tak separable linear, petakan $x$ ke ruang berdimensi lebih tinggi via $\Phi(x)$ sehingga hyperplane bisa dibangun di sana; perhitungan hanya butuh dot product di ruang baru.
- Karena $\Phi$ sulit dicari eksplisit, menurut Mercer dot product diganti fungsi kernel: $$K(x_i, x_j) = \Phi(x_i) \cdot \Phi(x_j)$$
- Tabel BKPM menyebut 7 kernel beserta penjelasan parameternya, tetapi baris rumus kosong di ekstraksi teks (hilang saat `pdftotext`). Rumus di bawah dilengkapi dari sumber web tepercaya [web1][web2][web3] untuk melengkapi materi kampus:

| No | Kernel | Rumus |
|---|---|---|
| 1 | Linear | $K(x_i, x_j) = \langle x_i, x_j \rangle = x_i^T x_j$ — untuk data separable linear [web1] |
| 2 | Polinomial derajat $d$ | $K(x_i, x_j) = (\gamma \langle x_i, x_j \rangle + r)^d$ dengan $d$ = derajat, $r$ ($c$ di BKPM) = bias [web1][web3] |
| 3 | Polinomial hingga derajat $d$ | $K(x_i, x_j) = \sum_{k=0}^{d} (\langle x_i, x_j \rangle + c)^k$ — memuat semua suku sampai derajat maksimum $d$ |
| 4 | RBF / Gaussian | $K(x_i, x_j) = \exp(-\gamma \|x_i - x_j\|^2) = \exp(-\|x_i - x_j\|^2 / (2\sigma^2))$ dengan $\|x_i - x_j\|^2$ jarak Euclidean, $\sigma$ lebar Gaussian ($\gamma = 1/(2\sigma^2) > 0$) [web1][web3] |
| 5 | Sigmoid | $K(x_i, x_j) = \tanh(\gamma \langle x_i, x_j \rangle + r)$ dengan $\gamma$ ($\alpha$ di BKPM) skala, $r$ ($c$) bias — mirip aktivasi jaringan saraf [web1][web3] |
| 6 | Inverse Multiquadratic | $K(x_i, x_j) = 1 / \sqrt{\|x_i - x_j\|^2 + c^2}$ dengan $c$ konstanta bebas pengontrol kelengkungan [web2] |
| 7 | Additive | $K(u, v) = \sum_{k=1}^{n} K_k(u_k, v_k)$ dengan $n$ jumlah fitur, $K_k$ kernel untuk fitur $k$ [web2] |

### 3. Multi-kelas: OvO vs OvR

- **One-vs-One (OvO)**: untuk kelas A, B, C buat 3 model biner (A–B, A–C, B–C); tiap model hanya tangani 2 kelas → biasanya lebih efisien untuk dataset besar.
- **One-vs-Rest (OvR / One-vs-All)**: tiap kelas dilawan gabungan semua kelas lain (A vs BC, B vs AC, C vs AB); lebih sederhana diimplementasikan, kadang kurang akurat dibanding OvO.

### 4. Alur praktikum BKPM

- Diberi dataset 2 fitur ($x_1, x_2$) → tulis program Python yang menggambar hyperplane → hyperplane awal program dinyatakan SALAH (arah maupun posisi) → hitung manual $w_1, w_2, b$ lewat formulasi optimasi + syarat, lalu perbaiki program dan gambar ulang dengan support vector ditandai (data merah (3,4), data biru (2,3)).

---

## Contoh (dengan perhitungan)

### Hitung $w_1 = w_2 = 1$, $b = -6$ → garis $x_2 = 6 - x_1$

Misal $w_1 = 1$, $w_2 = 1$. Ambil persamaan support vector (4): $3w_1 + 4w_2 + b \geq 1$:

$$3(1) + 4(1) + b \geq 1 \implies 7 + b \geq 1 \implies b \geq -6$$

Ambil persamaan (7): $-2w_1 - 3w_2 - b \geq 1$:

$$-2(1) - 3(1) - b \geq 1 \implies -5 - b \geq 1 \implies b \geq -6$$

Dengan support vector data ke-(4)/(7): $b = -6$. Persamaan hyperplane:

$$w_1 x_1 + w_2 x_2 + b = 0 \implies x_1 + x_2 - 6 = 0 \implies x_2 = 6 - x_1$$

### Tugas Python (dari BKPM)

Jalankan program SVM yang diberikan; bila kode benar, luaran awal muncul (dengan hyperplane yang salah). Lanjutkan menggambar grafik: support vector merah (3,4) dan biru (2,3), lalu betulkan garis sesuai $x_2 = 6 - x_1$ dan dokumentasikan hasil latihan.

---

## Sources

[web1] https://scikit-learn.org/stable/modules/svm.html — Kernel functions (linear, polynomial, RBF, sigmoid)
[web2] https://cse.msu.edu/~cse802/S17/slides/Lec_17_Mar27_SVM.pdf — Support Vector Machine and Kernel Methods (MSU)
[web3] https://apple.github.io/coremltools/mlmodel/Format/SVM.html — SVM kernel formulas (linear, RBF, poly, sigmoid)

---

## Keterkaitan

- Prasyarat klasifikasi: [[workshop-sistem-cerdas/materi/7 - k-means-clustering|K-Means Clustering]] (pengelompokan tak terawasi sebagai pembanding paradigma klasifikasi).
- Terkait representasi: [[workshop-sistem-cerdas/materi/2 - representasi-pengetahuan|Representasi Pengetahuan]].
- Mandiri terhadap: [[workshop-sistem-cerdas/materi/9 - decision-tree-id3-c45|Decision Tree ID3 & C4.5]] (satu sumber, topik klasifikasi berbeda).
