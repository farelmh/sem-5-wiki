---
matkul: workshop-sistem-cerdas
topik: DSS dan Metode SAW
urutan: 17
tanggal: 2026-10-12
tags: [workshop-sistem-cerdas, dss, spk, saw, ranking, pengambilan-keputusan]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas_Fuzzy_dan_Decision_Support_System.pdf"
---

# DSS dan Metode SAW

## Ringkasan

Decision Support System (DSS) / Sistem Pendukung Keputusan (SPK) adalah sistem berbasis komputer yang membantu manusia/organisasi mengambil keputusan memakai data, model, dan metode analisis — bukan menggantikan pengambil keputusan, melainkan memberi informasi, analisis, dan rekomendasi yang terstruktur. Metode yang dibahas BKPM: SAW (dikerjakan penuh lewat dua contoh), plus AHP, TOPSIS, WP, MOORA, SMART, Profile Matching, PROMETHEE (disebut saja). Alur SAW: Alternatif → Kriteria → Bobot → Normalisasi → Nilai akhir → Ranking.

---

## Konsep Kunci

### 1. Komponen utama SPK

| Komponen | Fungsi |
|---|---|
| Data | Menyimpan data untuk pengambilan keputusan |
| Model/Metode | Mengolah dan menganalisis data |
| Basis Pengetahuan | Menyimpan aturan / informasi pendukung |
| User Interface | Media interaksi pengguna–sistem |
| Pengguna / Decision Maker | Tetap menentukan keputusan akhir |

### 2. Benefit vs cost

- **Benefit**: makin besar makin baik (misal nilai investasi, daya dukung). Normalisasi: $r_{ij} = \dfrac{x_{ij}}{\max_i x_{ij}}$.
- **Cost**: makin kecil makin baik (misal harga). Normalisasi: $r_{ij} = \dfrac{\min_i x_{ij}}{x_{ij}}$.
- Salah menetapkan jenis = ranking terbalik; ini titik paling rawan.

### 3. Alur 5 tahap SAW (BKPM)

1. Tentukan alternatif dan kriteria.
2. Tentukan bobot tiap kriteria + jenisnya (benefit/cost).
3. Normalisasi matriks keputusan $\to r_{ij}$ (rumus di atas; $r_{ij}$ = rating ternormalisasi alternatif $A_i$ pada atribut $C_j$).
4. Hitung nilai preferensi: $V_i = \sum_j w_j r_{ij}$.
5. Ranking: $V_i$ terbesar = alternatif terpilih.

![[Excalidraw/flowchart-saw-ranking.excalidraw]]

### 4. Keterbatasan

Hasil sangat bergantung pada ketepatan penentuan kriteria, bobot, jenis benefit/cost, dan kualitas data. Bobot subjektif pengambil keputusan langsung menggeser pemenang.

---

## Contoh (dengan perhitungan)

### Contoh 1 — Kepala UPT TIK (semua benefit, $6 \times 4$)

Kriteria: C1 wawasan SI, C2 praktik jaringan, C3 kepribadian, C4 agama. Bobot $w = [0{,}35;\ 0{,}25;\ 0{,}25;\ 0{,}15]$. Alternatif: A1 Indra, A2 Roni, A3 Putri, A4 Dani, A5 Ratna, A6 Mira.

Matriks (C1–C4): Indra [70, 50, 80, 60]; Roni [50, 60, 82, 70]; Putri [85, 55, 80, 75]; Dani [82, 70, 65, 85]; Ratna [75, 75, 85, 74]; Mira [62, 50, 75, 80]. Maks kolom: 85, 75, 85, 85.

Normalisasi (contoh Indra): $r = [70/85;\ 50/75;\ 80/85;\ 60/85] = [0{,}8235;\ 0{,}6667;\ 0{,}9412;\ 0{,}7059]$.

Nilai preferensi (hitung ulang terverifikasi):

- V1 Indra $= 0{,}35(0{,}8235) + 0{,}25(0{,}6667) + 0{,}25(0{,}9412) + 0{,}15(0{,}7059) = 0{,}7961$
- V2 Roni $= 0{,}7706$
- V3 Putri $= 0{,}9010$
- V4 Dani $= 0{,}9122$
- **V5 Ratna $= 0{,}35(0{,}8824) + 0{,}25(1) + 0{,}25(1) + 0{,}15(0{,}8706) = 0{,}9394$ ← pemenang**
- V6 Mira $= 0{,}7837$

### Contoh 2 — Investasi SIP ($4 \times 5$, campuran)

Alternatif: A1 mobil box, A2 tanah gudang, A3 maintenance TI, A4 produk baru. Bobot $w = [0{,}25;\ 0{,}15;\ 0{,}30;\ 0{,}25;\ 0{,}05]$. Jenis: C1 harga = cost; C2 nilai investasi = benefit; C3 daya dukung = benefit; C4 prioritas (1 sangat → 3 cukup) = cost; C5 ketersediaan = benefit.

Data: A1 [150, 15, 2, 2, 3]; A2 [500, 200, 2, 3, 2]; A3 [200, 10, 3, 1, 3]; A4 [350, 100, 3, 1, 2]. Min C1 $= 150$, maks C2 $= 200$, maks C3 $= 3$, min C4 $= 1$, maks C5 $= 3$.

Normalisasi (contoh A3): $r = [150/200;\ 10/200;\ 3/3;\ 1/1;\ 3/3] = [0{,}75;\ 0{,}05;\ 1;\ 1;\ 1]$.

Nilai preferensi:

- V1 $= 0{,}25(1) + 0{,}15(0{,}075) + 0{,}30(0{,}6667) + 0{,}25(0{,}5) + 0{,}05(1) = 0{,}6362$
- V2 $= 0{,}5417$
- **V3 $= 0{,}25(0{,}75) + 0{,}15(0{,}05) + 0{,}30(1) + 0{,}25(1) + 0{,}05(1) = 0{,}795$ ← pemenang (maintenance TI)**
- V4 $= 0{,}7655$

---

## Keterkaitan

- Mandiri terhadap dua halaman fuzzy (tidak ada prasyarat hitungan fuzzy).
- Jembatan konseptual: output tegas FIS (misal [[workshop-sistem-cerdas/materi/16 - fis-tsukamoto-sugeno|FIS Tsukamoto–Sugeno]]) dapat menjadi salah satu kriteria/skor input DSS; DSS menggabungkan banyak kriteria berbobot menjadi ranking.
