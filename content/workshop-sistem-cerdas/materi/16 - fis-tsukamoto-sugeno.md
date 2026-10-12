---
matkul: workshop-sistem-cerdas
topik: FIS Tsukamoto dan Sugeno
urutan: 16
tanggal: 2026-10-12
tags: [workshop-sistem-cerdas, fuzzy, fis, tsukamoto, sugeno, inferensi]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas_Fuzzy.pdf"
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas_Fuzzy_dan_Decision_Support_System.pdf"
---

# FIS Tsukamoto dan Sugeno

## Ringkasan

Fuzzy Inference System (FIS) adalah metode logika fuzzy untuk pengambilan keputusan — dipakai sebagai sistem kontrol pengganti operator. BKPM menyebut tiga metode: Tsukamoto, Sugeno, Mamdani; yang dipraktikkan dua pertama. Keduanya memakai anteseden fuzzy + agregasi rata-rata terbobot, tetapi berbeda konsekuen: Tsukamoto memakai himpunan fuzzy monoton yang di-invers dari $\alpha$-predikat, Sugeno memakai konstanta / persamaan linear langsung.

---

## Konsep Kunci

### 1. Rule base (kasus produksi ABC)

Empat aturan dengan dua anteseden (permintaan × persediaan):

1. IF permintaan TURUN AND persediaan BANYAK THEN produksi BERKURANG (Tsukamoto) / $=$ permintaan $-$ persediaan (Sugeno).
2. IF permintaan TURUN AND persediaan SEDIKIT THEN produksi BERKURANG / $=$ permintaan.
3. IF permintaan NAIK AND persediaan BANYAK THEN produksi BERTAMBAH / $=$ permintaan.
4. IF permintaan NAIK AND persediaan SEDIKIT THEN produksi BERTAMBAH / $= 1{,}25 \times$ permintaan $-$ persediaan.

Domain: PERMINTAAN $1000$–$5000$, PERSEDIAAN $100$–$600$, PRODUKSI $2000$–$7000$.

### 2. Tsukamoto: konsekuen monoton + $\alpha$-predikat

- Syarat: tiap konsekuen direpresentasikan himpunan fuzzy dengan fungsi keanggotaan **monoton** (linear naik untuk BERTAMBAH, linear turun untuk BERKURANG).
- **$\alpha$-predikat** (fire strength) tiap rule: $\alpha_i = \min(\mu_\text{permintaan}, \mu_\text{persediaan})$.
- Output tegas tiap rule $z_i$ didapat dengan **invers** fungsi monoton pada $\alpha_i$:
  - BERKURANG (linear turun, $2000$–$7000$): $z = 7000 - \alpha \cdot 5000$.
  - BERTAMBAH (linear naik, $2000$–$7000$): $z = 2000 + \alpha \cdot 5000$.

### 3. Sugeno orde-nol dan orde-satu

- **Orde-nol**: konsekuen konstanta tegas. Bentuk umum: IF $x_1$ is $A_1$ AND … THEN $z = k$.
- **Orde-satu** (Takagi-Sugeno-Kang, 1985): konsekuen persamaan linear. Bentuk umum: IF … THEN $z = p_1 x_1 + \dots + p_n x_n + q$.
- Kasus ABC memakai orde-satu: $z_1 = x - y$, $z_2 = x$, $z_3 = x$, $z_4 = 1{,}25x - y$. $\alpha$-predikat sama seperti Tsukamoto (Min).

### 4. Agregasi: rata-rata terbobot

Kedua metode menutup dengan rumus yang sama:

$$Z = \frac{\sum_i \alpha_i z_i}{\sum_i \alpha_i}$$

---

## Contoh (dengan perhitungan)

### Kasus BKPM: $x = 4000$ (permintaan), $y = 300$ (persediaan)

Derajat (linear; hitung ulang terverifikasi): $\mu_\text{TURUN} = 0{,}25$, $\mu_\text{NAIK} = 0{,}75$, $\mu_\text{BANYAK} = 0{,}4$, $\mu_\text{SEDIKIT} = 0{,}6$.

$\alpha$-predikat:

- $\alpha_1 = \min(0{,}25;\ 0{,}4) = 0{,}25$
- $\alpha_2 = \min(0{,}25;\ 0{,}6) = 0{,}25$
- $\alpha_3 = \min(0{,}75;\ 0{,}4) = 0{,}4$
- $\alpha_4 = \min(0{,}75;\ 0{,}6) = 0{,}6$ (jumlah $= 1{,}5$)

**Tsukamoto**:

- $z_1 = 7000 - 0{,}25 \cdot 5000 = 5750$
- $z_2 = 7000 - 0{,}25 \cdot 5000 = 5750$
- $z_3 = 2000 + 0{,}4 \cdot 5000 = 4000$
- $z_4 = 2000 + 0{,}6 \cdot 5000 = 5000$
- $Z = \dfrac{0{,}25 \cdot 5750 + 0{,}25 \cdot 5750 + 0{,}4 \cdot 4000 + 0{,}6 \cdot 5000}{1{,}5} = \dfrac{7475}{1{,}5} \approx 4983{,}33$ kemasan/hari.

**Sugeno**:

- $z_1 = 4000 - 300 = 3700$
- $z_2 = 4000$
- $z_3 = 4000$
- $z_4 = 1{,}25 \cdot 4000 - 300 = 4700$
- $Z = \dfrac{0{,}25 \cdot 3700 + 0{,}25 \cdot 4000 + 0{,}4 \cdot 4000 + 0{,}6 \cdot 4700}{1{,}5} = \dfrac{6345}{1{,}5} = 4230$ kemasan/hari.

### Tugas BKPM: $x = 2500$, $y = 500$

Derajat: $\mu_\text{TURUN} = 0{,}625$, $\mu_\text{NAIK} = 0{,}375$, $\mu_\text{BANYAK} = 0{,}8$, $\mu_\text{SEDIKIT} = 0{,}2$.

$\alpha$: $\alpha_1 = 0{,}625$, $\alpha_2 = 0{,}2$, $\alpha_3 = 0{,}375$, $\alpha_4 = 0{,}2$ (jumlah $= 1{,}4$).

**Tsukamoto**: $z_1 = 3875$, $z_2 = 6000$, $z_3 = 3875$, $z_4 = 3000$.

$$Z = \frac{0{,}625 \cdot 3875 + 0{,}2 \cdot 6000 + 0{,}375 \cdot 3875 + 0{,}2 \cdot 3000}{1{,}4} = \frac{5675}{1{,}4} \approx 4053{,}57$$

**Sugeno**: $z_1 = 2000$, $z_2 = 2500$, $z_3 = 2500$, $z_4 = 2625$.

$$Z = \frac{0{,}625 \cdot 2000 + 0{,}2 \cdot 2500 + 0{,}375 \cdot 2500 + 0{,}2 \cdot 2625}{1{,}4} = \frac{3212{,}5}{1{,}4} \approx 2294{,}64$$

Catatan praktikum: tugas BKPM juga meminta laporan perhitungan manual + Excel + Python; angka di atas adalah bagian manualnya.

---

## Keterkaitan

- Prasyarat: [[workshop-sistem-cerdas/materi/15 - logika-fuzzy-dasar|Logika Fuzzy Dasar]] (derajat keanggotaan, operasi Min, pipeline fuzzifikasi–defuzzifikasi).
- Lanjutan: metode Mamdani (disebut BKPM, belum dipraktikkan); implementasi Python Tsukamoto/Sugeno di BKPM.
