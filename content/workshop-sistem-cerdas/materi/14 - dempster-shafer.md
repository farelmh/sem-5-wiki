---
matkul: workshop-sistem-cerdas
topik: Dempster-Shafer
urutan: 14
tanggal: 2026-10-12
tags: [workshop-sistem-cerdas, dempster-shafer, ketidakpastian, belief, plausibility]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas_Sistem_Pakar.pdf"
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas_Sistem_Pakar_dan_NLP.pdf"
---

# Dempster–Shafer

## Ringkasan

Teori Dempster–Shafer adalah teori matematika pembuktian berbasis belief function dan plausible reasoning untuk mengkombinasikan potongan evidence terpisah dan menghitung kemungkinan suatu peristiwa. Keyakinan ditulis sebagai interval $[\text{Bel}, \text{Pl}]$: Bel = kekuatan evidence yang mendukung, Pl = sejauh mana hipotesis masih mungkin (tidak terbantahkan). Evidence baru digabung lewat aturan kombinasi Dempster; studi kasus BKPM (Abi: panas → hidung buntu → makan udang) berakhir pada kesimpulan Flu + Demam dengan $m_5\{F,D\} = 0{,}554$.

---

## Konsep Kunci

### 1. Frame of discernment $\theta$

$\theta$ = semesta hipotesis. Kasus BKPM: $\theta = \{A, F, D, B\}$ dengan $A$ = Alergi, $F$ = Flu, $D$ = Demam, $B$ = Bronkitis. Tanpa informasi apapun: $m\{\theta\} = 1{,}0$ (seluruh massa pada ketidaktahuan).

### 2. Mass function $m$

$m$ = fungsi densitas kepercayaan yang dilekatkan pada tiap himpunan bagian $\theta$; sisa yang tak teralokasi selalu kembali ke $m\{\theta\}$. Contoh: panas gejala $\{F,D,B\}$ dengan $m = 0{,}8$ → $m\{F,D,B\} = 0{,}8$, $m\{\theta\} = 1 - 0{,}8 = 0{,}2$.

### 3. Belief dan plausibility

- **Belief** $\text{Bel}(S)$: ukuran kekuatan evidence yang mendukung $S$; $0$ = tak ada evidence.
- **Plausibility**: $\text{Pl}(S) = 1 - \text{Bel}(\bar{S})$; $1$ = kepastian penuh. (BKPM menulis $\text{Pl}(s) = 1 - \text{Bel}(s)$.)
- Interval $[\text{Bel}, \text{Pl}]$: batas bawah keyakinan vs batas atas kemungkinan.

### 4. Aturan kombinasi

Evidence $m_1, m_2$ digabung menjadi $m_3$ dengan mengkombinasikan tiap himpunan bagian berdasarkan **irisan** kedua himpunan, lalu menormalisasi dengan membagi $1 - K$ ($K$ = massa konflik, irisan kosong):

$$m_3(Z) = \frac{\sum_{X \cap Y = Z} m_1(X)\, m_2(Y)}{1 - K}, \quad K = \sum_{X \cap Y = \emptyset} m_1(X)\, m_2(Y)$$

Praktik BKPM: kolom = himpunan gejala 1 ($m_1$), baris = himpunan gejala 2 ($m_2$), sel = irisan × perkalian massa.

---

## Contoh (dengan perhitungan)

### Gejala 1 — panas: $m_1\{F,D,B\} = 0{,}8$, $m_1\{\theta\} = 0{,}2$

### Gejala 2 — hidung buntu: $m_2\{A,F,D\} = 0{,}9$, $m_2\{\theta\} = 0{,}1$

Kombinasi $m_3$ (tak ada konflik, $K = 0$):

| $\cap$ | $\{A,F,D\}$ (0,9) | $\theta$ (0,1) |
|---|---|---|
| $\{F,D,B\}$ (0,8) | $\{F,D\}$: 0,72 | $\{F,D,B\}$: 0,08 |
| $\theta$ (0,2) | $\{A,F,D\}$: 0,18 | $\theta$: 0,02 |

Hasil: $m_3\{F,D\} = 0{,}72$, $m_3\{F,D,B\} = 0{,}08$, $m_3\{A,F,D\} = 0{,}18$, $m_3\{\theta\} = 0{,}02$. Densitas terkuat: $\{F,D\}$.

### Gejala 3 — makan udang: $m_4\{A\} = 0{,}6$, $m_4\{\theta\} = 0{,}4$

Kombinasi $m_3 \oplus m_4 \to m_5$; konflik $K = 0{,}72 \cdot 0{,}6 + 0{,}08 \cdot 0{,}6 = 0{,}48$ (irisan dengan $\{A\}$ yang kosong), penyebut $1 - K = 0{,}52$. Massa $\{F,D\}$ hanya dari $\{F,D\} \cap \theta$: $0{,}72 \cdot 0{,}4 = 0{,}288$, sehingga:

$$m_5\{F,D\} = \frac{0{,}288}{0{,}52} \approx 0{,}554$$

Nilai paling bertahan. **Kesimpulan: Abi kemungkinan paling kuat terkena Flu dan Demam.**

---

## Keterkaitan

- Prasyarat: [[sistem-pakar]] (mesin inferensi + strategi forward/backward yang digabung metode ini)
- Lanjutan: —
