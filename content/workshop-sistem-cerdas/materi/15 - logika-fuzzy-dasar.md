---
matkul: workshop-sistem-cerdas
topik: Logika Fuzzy Dasar
urutan: 15
tanggal: 2026-10-12
tags: [workshop-sistem-cerdas, fuzzy, logika-fuzzy, membership-function, kontrol-fuzzy]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas_Fuzzy.pdf"
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas_Fuzzy_dan_Decision_Support_System.pdf"
---

# Logika Fuzzy Dasar

## Ringkasan

Logika fuzzy adalah metodologi pemecahan masalah untuk informasi yang ambigu, samar, atau tidak tepat — meniru cara manusia mengambil keputusan dari data yang ditafsirkan. Bedanya dengan logika tegas: logika tegas hanya mengenal tidak $= 0$ dan ya $= 1$, sedangkan fuzzy memakai derajat keanggotaan $0 \le \mu \le 1$. Halaman ini mencakup himpunan crisp vs fuzzy, operasi himpunan, fungsi keanggotaan, dan pipeline sistem kontrol fuzzy dari fuzzifikasi sampai defuzzifikasi.

---

## Konsep Kunci

### 1. Crisp set vs fuzzy set

- **Crisp**: $\mu_A[x] \in \{0, 1\}$. Anggota penuh (1) atau bukan anggota (0).
- **Fuzzy**: $\mu_A[x] \in [0, 1]$. Nilai $0$ = bukan anggota, $1$ = anggota penuh, di antaranya = anggota sebagian.
- **Contoh suhu (BKPM Gb. 1–2)**: himpunan klasik punya batas tajam (`[` = awal scope, `)` = akhir scope) — misal suhu $35^{\circ}\mathrm{C}$ langsung "panas penuh". Himpunan fuzzy punya batas kabur — $35^{\circ}\mathrm{C}$ bisa sekaligus anggota "hangat" $0{,}4$ dan "panas" $0{,}6$.

### 2. Atribut himpunan fuzzy dan domain

- **Linguistik**: nama grup berbahasa alami, misal MUDA, PAROBAYA, TUA; TURUN/NAIK; SEDIKIT/BANYAK.
- **Numerik**: angka ukuran variabel, misal 40, 25, 50.
- **Domain**: keseluruhan nilai yang diizinkan dalam semesta pembicaraan.
- **Fungsi keanggotaan**: kurva pemetaan input $\to$ derajat $0 \le \mu \le 1$, dinyatakan numerik atau berupa fungsi.

### 3. Operasi himpunan fuzzy (plus koreksi BKPM)

- Gabungan (union) = **Max**: $\mu_{A \cup B} = \max(\mu_A[x], \mu_B[y])$.
- Irisan (intersection) = **Min**: $\mu_{A \cap B} = \min(\mu_A[x], \mu_B[y])$.
- Kesamaan (equality): $\mu_A(x) = \mu_B(x), x \in U$.
- > ⚠️ **Label tertukar di BKPM**: butir "Produk (Product)" diberi rumus $\mu_{A'}(x) = 1 - \mu_A(x)$ — itu sebenarnya **komplemen**. Butir "Komplemen (complement)" diberi rumus $\mu_{(A \cdot B)}(x) = \mu_A(x) \cdot \mu_B(x)$ — itu sebenarnya **produk**. Yang benar:
- **Komplemen**: $\mu_{A'}(x) = 1 - \mu_A(x), x \in U$.
- **Produk**: $\mu_{(A \cdot B)}(x) = \mu_A(x) \cdot \mu_B(x), x \in U$.

### 4. Fungsi keanggotaan

**Linear naik** (derajat 0 di kiri, naik ke kanan), parameter $(a, b)$:

$$\mu(x) = \begin{cases} 0 & x \le a \\ \dfrac{x - a}{b - a} & a < x < b \\ 1 & x \ge b \end{cases}$$

**Linear turun** (derajat tertinggi di kiri, turun ke kanan), parameter $(a, b)$:

$$\mu(x) = \begin{cases} 1 & x \le a \\ \dfrac{b - x}{b - a} & a < x < b \\ 0 & x \ge b \end{cases}$$

**Segitiga**, parameter $(a, b, c)$:

$$\mu(x) = \begin{cases} 0 & x \le a \text{ atau } x \ge c \\ \dfrac{x - a}{b - a} & a < x \le b \\ \dfrac{c - x}{c - b} & b < x < c \end{cases}$$

**Trapesium**, parameter $(a, b, c, d)$:

$$\mu(x) = \begin{cases} 0 & x \le a \text{ atau } x \ge d \\ \dfrac{x - a}{b - a} & a < x < b \\ 1 & b \le x \le c \\ \dfrac{d - x}{d - c} & c < x < d \end{cases}$$

### 5. Pipeline sistem kontrol logika fuzzy

![[Excalidraw/blok-kontrol-fuzzy.excalidraw]]

1. **Fuzzifikasi**: ubah input tegas $\to$ derajat keanggotaan (misal suhu $35^{\circ}\mathrm{C}$ pada kurva segitiga).
2. **Rule base**: set aturan IF-THEN fuzzy — inti sistem. Perhatian: pemilihan variabel input/output, jangkauan nilai linguistik (perlu tuning), penurunan aturan (pengalaman / model fuzzy / model matematika).
3. **Implikasi**: fungsi akibat "jika p maka q" — Dienes-Rescher, Lukasiewicz, Zadeh, Gödel, Mamdani.
4. **Inference engine**: gabung aturan anteseden $\to$ konsekuen. Kombinasi gabungan: Product / Minimum. Kombinasi irisan: Lukasiewicz / Zadeh / Dienes-Rescher.
5. **Defuzzifikasi**: ubah output fuzzy $\to$ tegas. Metode: Center of Gravity (COG), Center of Average, Bisector, MOM, LOM, SOM.

---

## Contoh (dengan perhitungan)

Variabel PERMINTAAN domain $1000$–$5000$ (linear), input $x = 4000$:

- $\mu_\text{TURUN} = \dfrac{5000 - 4000}{5000 - 1000} = \dfrac{1000}{4000} = 0{,}25$
- $\mu_\text{NAIK} = \dfrac{4000 - 1000}{5000 - 1000} = 0{,}75$

Variabel PERSEDIAAN domain $100$–$600$, input $y = 300$:

- $\mu_\text{BANYAK} = \dfrac{300 - 100}{600 - 100} = \dfrac{200}{500} = 0{,}4$
- $\mu_\text{SEDIKIT} = \dfrac{600 - 300}{600 - 100} = 0{,}6$

Operasi pada derajat tersebut:

- Irisan (Min): $\min(0{,}25;\ 0{,}4) = 0{,}25$ → dipakai sebagai $\alpha$-predikat rule "TURUN AND BANYAK".
- Gabungan (Max): $\max(0{,}25;\ 0{,}4) = 0{,}4$.
- Komplemen: $1 - 0{,}25 = 0{,}75$ (= $\mu_\text{NAIK}$, konsisten karena linear naik/turun saling komplemen).

---

## Keterkaitan

- Prasyarat: —
- Lanjutan: [[workshop-sistem-cerdas/materi/16 - fis-tsukamoto-sugeno|FIS Tsukamoto–Sugeno]] (memakai derajat dan $\alpha$-predikat dari halaman ini); metode Mamdani (disebut BKPM, belum dipraktikkan).
