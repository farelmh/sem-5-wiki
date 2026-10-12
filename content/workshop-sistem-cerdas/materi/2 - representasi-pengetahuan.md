---
matkul: workshop-sistem-cerdas
topik: Representasi Pengetahuan
urutan: 2
tanggal: 2026-10-12
minggu: 2
tags: [workshop-sistem-cerdas, representasi-pengetahuan, aturan-produksi, semantic-network, frame, graf]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas-Minggu_2.pdf"
  - "workshop-sistem-cerdas/resource/kampus/Knowledge_Representation_-_Graph_Dijkstra_-_Share.pdf"
---

# Representasi Pengetahuan

## Ringkasan

Representasi pengetahuan (KR) = cara sajikan fakta domain agar komputer bisa olah, bernalar, putuskan. Logika = proses bentuk kesimpulan dari premis benar lewat penalaran. Materi: 4 syarat KR baik, logika + aturan produksi, forward/backward reasoning, semantic net, frame, tree vs graph, matriks adjacency. Studi kasus: aturan kredit, frame mobil sedan, semantic net kenari, klasifikasi hewan (NetworkX opsional).

---

## Konsep Kunci

### 1. Empat syarat KR baik

- **Representational Adequacy**: mampu wakili semua jenis pengetahuan domain.
- **Inferential Adequacy**: mampu manipulasi struktur hasilkan pengetahuan baru (inferensi).
- **Inferential Efficiency**: info tambahan bikin inferensi lebih efisien.
- **Acquisitional Efficiency**: mudah tambah pengetahuan baru.

Bentuk umum: logika (proposisi & predikat), aturan produksi, jaringan semantik, frame, pohon, graf.

### 2. Logika dan aturan produksi

Logika nyatakan pengetahuan proposisi/predikat benar-salah. Aturan produksi: IF (antecedent/premis) THEN (konsekuen/tindakan). Konsekuen benar hanya jika IF benar. Bentuk tulis:

- `IF premis THEN kesimpulan` — "Jika pendapatan tinggi MAKA pajak tinggi".
- `Kesimpulan IF premis` — "Pajak tinggi JIKA pendapatan tinggi".
- `IF ... OR ... THEN ... ELSE ...` — pendapatan tinggi OR pengeluaran tinggi THEN pajak tinggi ELSE pajak rendah.
- Kompleks: `IF rating kredit tinggi AND gaji > Rp 3 Jt OR aset > Rp 75 Jt AND riwayat bayar tidak buruk THEN pinjaman > Rp 10 Jt disetujui AND kategori "B"`.

Deklaratif, mudah dibaca; lemah untuk relasi hierarkis banyak.

### 3. Forward vs backward reasoning

- **Forward**: dari fakta awal → cocokkan → ke tujuan. Fakta-baru picu penyelesaian.
- **Backward**: dari tujuan/hipotesa → cocokkan mundur ke fakta. Query picu penyelesaian.

Faktor pilih: jumlah fakta awal vs tujuan (kecil → mulai dari sisi kecil; fakta awal sedikit pakai forward, tujuan banyak pakai backward), rata-rata cabang node (pilih cabang sedikit), butuh tanya user (pilih arah mudah), pemicu (fakta baru = forward, query = backward).

### 4. Jaringan semantik

Graf node (objek/konsep/situasi) + arc/edge (relasi). Kuat di pewarisan sifat (inheritance). Relasi umum: **Is-a** (kelas, Canary is-a Bird) dan **Has** (bagian, Bird has Wings).

### 5. Frame

Struktur slot-filler per objek, mirip OOP. Frame = objek/konsep, slot = atribut, filler = nilai.

### 6. Tree vs graph

- **Tree**: hierarki root-cabang-leaf, tanpa siklus. Guna: organisasi, taksonomi, decision tree, pohon pelacakan search.
- **Graph**: $G = (V, E)$, $V$ simpul, $E$ sisi. Boleh siklus, banyak jalur antar dua simpul. Guna: ruang keadaan (state) search, simpul = keadaan, sisi = aksi/transisi + cost. Fondasi algoritma A*.

### 7. Matriks adjacency

Matriks $n \times n$ ($n$ = jumlah simpul). Elemen $A_{i,j}$ = ada sisi $i$-$j$ (1 atau bobot; 0 bila tak ada). Undirected: simetris ($A_{i,j} = A_{j,i}$). Directed: tak selalu simetris.

---

## Contoh (dengan perhitungan)

### Aturan kredit (forward chaining)

Fakta: rating = tinggi, gaji = Rp 4 Jt, aset = Rp 80 Jt, riwayat = baik. Aturan: `IF rating tinggi AND (gaji > 3 Jt OR aset > 75 Jt) AND riwayat tidak buruk THEN setuju > 10 Jt kategori B`. Evaluasi: tinggi $\land$ (4 > 3 $\lor$ 80 > 75) $\land$ baik = benar $\land$ benar $\land$ benar = **benar** → pinjaman disetujui kategori B.

### Frame mobil sedan

- Nama frame: Mobil_Sedan. Slot-filler: Merk Toyota, Model Vios, Warna Hitam, Jumlah_Pintu 4, Bahan_Bakar Bensin, Kecepatan_Maksimum 180 km/jam.

### Semantic net kenari

Canary —Is-a→ Bird —Is-a→ Animal. Bird —Has→ Wings. Canary —Has-color→ Yellow. Inferensi warisan: Canary dapat sifat Bird (punya sayap) dan Animal (hewan) tanpa tulis ulang.

### Adjacency graf $V = \{A,B,C\}$, $E = \{AB, BC\}$

$$A = \begin{pmatrix} 0 & 1 & 0 \\ 1 & 0 & 1 \\ 0 & 1 & 0 \end{pmatrix}$$

Baris B: terhubung A dan C. Simetris karena undirected.

---

## Keterkaitan

- Prasyarat: [[workshop-sistem-cerdas/materi/1 - pengenalan-sistem-cerdas|Pengenalan Sistem Cerdas]]
- Lanjutan: [[workshop-sistem-cerdas/materi/3 - graf-dan-dijkstra|Graf dan Dijkstra]], [[workshop-sistem-cerdas/materi/4 - pencarian-a-star|Pencarian A-Star]]
