---
matkul: workshop-sistem-cerdas
topik: Exponential Smoothing
urutan: 12
tanggal: 2026-10-12
tags: [workshop-sistem-cerdas, prediksi, exponential-smoothing, time-series]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas_JST__Prediksi.pdf"
---

# Exponential Smoothing

## Ringkasan

Exponential Smoothing adalah metode statistik sederhana untuk prediksi deret waktu: data terbaru diberi bobot lebih besar, data lama bobotnya mengecil secara eksponensial. Kelebihan: hitungan mudah, komputasi ringan, cocok untuk data berkala yang mengutamakan data terbaru. Ada tiga varian — SES untuk data stabil, DES (Holt) untuk data bertren, TES (Holt-Winters) untuk data bertren + musiman — dengan parameter $\alpha$ (level), $\beta$ (tren), $\gamma$ (musiman), masing-masing $0 < \text{param} < 1$.

![[Excalidraw/pilih-metode-smoothing.excalidraw]]

---

## Konsep Kunci

### 1. Kapan pakai apa

| Metode | Dipakai saat | Parameter |
|--------|--------------|-----------|
| SES (Simple) | Data relatif stabil, tanpa tren dan musiman kuat | $\alpha$ |
| DES (Double/Holt) | Data memiliki tren (level + trend) | $\alpha$, $\beta$ |
| TES (Triple/Holt-Winters) | Data memiliki level + tren + musiman; contoh BKPM memakai varian aditif (pola musiman dianggap tambahan tetap) | $\alpha$, $\beta$, $\gamma$ |

### 2. Peran parameter

- $\alpha = 0{,}1$ → data terbaru sedikit berpengaruh; $\alpha = 0{,}5$ → pengaruh seimbang; $\alpha = 0{,}9$ → prediksi sangat mengikuti data terbaru.
- $\beta$: seberapa cepat komponen tren menyesuaikan; $\gamma$: seberapa cepat komponen musiman menyesuaikan.

### 3. Rumus SES

$$F_{t+1} = \alpha A_t + (1 - \alpha) F_t$$

$F_{t+1}$ = prediksi periode berikut; $A_t$ = data aktual periode sekarang; $F_t$ = prediksi periode sekarang; $0 < \alpha < 1$.

### 4. Rumus DES (Holt)

- Level: $$L_t = \alpha A_t + (1 - \alpha)(L_{t-1} + T_{t-1})$$
- Trend: $$T_t = \beta(L_t - L_{t-1}) + (1 - \beta)T_{t-1}$$
- Prediksi $m$ periode ke depan: $$F_{t+m} = L_t + mT_t$$

### 5. Rumus TES aditif (Holt-Winters)

- Level: $$L_t = \alpha(A_t - S_{t-s}) + (1 - \alpha)(L_{t-1} + T_{t-1})$$
- Trend: $$T_t = \beta(L_t - L_{t-1}) + (1 - \beta)T_{t-1}$$
- Musiman: $$S_t = \gamma(A_t - L_t) + (1 - \gamma)S_{t-s}$$
- Prediksi: $$F_{t+m} = L_t + mT_t + S_{t+m-s}$$

$s$ = panjang musim, $m$ = jumlah periode yang diprediksi.

### 6. Kelebihan vs kelemahan (tabel kesimpulan BKPM)

- SES: cocok data stabil, butuh sedikit data/parameter, utamakan data terbaru; lemah untuk data bertren/musiman, sensitif terhadap pilihan $\alpha$.
- DES: tangani tren naik/turun, lebih baik dari SES untuk data berkembang, masih sederhana; tak tangani musiman, perlu 2 parameter, sensitif pilihan parameter.
- TES: tangani level+tren+musim sekaligus, cocok pola berulang, fleksibel; hitungan kompleks, perlu $\alpha,\beta,\gamma$ + panjang musim + data cukup, sensitif parameter/pola.

---

## Contoh (dengan perhitungan)

### SES: penjualan toko, $\alpha = 0{,}5$ → Juni = 122,5

Data: Jan 100, Feb 120, Mar 110, Apr 130, Mei 125. Prediksi awal $F_{Jan} = 100$.

- $F_{Feb} = 0{,}5(100) + 0{,}5(100) = 100$
- $F_{Mar} = 0{,}5(120) + 0{,}5(100) = 110$
- $F_{Apr} = 0{,}5(110) + 0{,}5(110) = 110$
- $F_{Mei} = 0{,}5(130) + 0{,}5(110) = 120$
- $F_{Jun} = 0{,}5(125) + 0{,}5(120) = 62{,}5 + 60 = 122{,}5$ (≈ 123 unit)

### DES: tren naik +20/bulan, $\alpha = \beta = 0{,}5$ → Juni = 200

Data: Jan 100, Feb 120, Mar 140, Apr 160, Mei 180. Inisialisasi $L_{Jan} = 100$, $T_{Jan} = 20$.

- Feb: $L = 0{,}5(120) + 0{,}5(100+20) = 120$; $T = 0{,}5(120-100) + 0{,}5(20) = 20$
- Mar: $L = 0{,}5(140) + 0{,}5(140) = 140$; $T = 20$ (pola konsisten)
- Apr: $L = 160, T = 20$; Mei: $L = 180, T = 20$
- Prediksi Juni: $F = L_{Mei} + T_{Mei} = 180 + 20 = 200$ unit

### TES: pengguna LMS per kuartal → Tahun 4: 160/180/170/190

Data Tahun 1–3 menunjukkan tren +20/tahun dan musiman Q1 −15, Q2 +5, Q3 −5, Q4 +15 dengan level 115/135/155 (→175). Prediksi Tahun 4: Q1 = 160, Q2 = 180, Q3 = 170, Q4 = 190.

### Tugas dari BKPM (untuk dikerjakan)

1. **SES vs DES** — data layanan admin 12 bulan (Jan 100, Feb 105, Mar 110, Apr 120, Mei 125, Jun 130, Jul 145, Agu 150, Sep 155, Okt 165, Nov 170, Des 175): hitung keduanya dengan Excel, bandingkan, prediksi Januari berikut.
2. **TES** — pengguna aktif S-Learn per kuartal (Tahun 1: 100/130/110/150; Tahun 2: 120/150/130/170; Tahun 3: 140/170/150/190): prediksi Q1–Q4 Tahun 4 dengan Holt-Winters.

---

## Keterkaitan

- Mandiri: tidak bergantung pada halaman LVQ; keduanya dari satu sumber tetapi topik terpisah (prediksi deret waktu vs klasifikasi).
- Prasyarat: pemahaman deret waktu/level-tren-musiman (belum ada halaman).
