---
matkul: aplikasi-sistem-tertanam
topik: Pengantar Elektronika, Rangkaian, Alat Ukur, dan K3
urutan: 1
tanggal: 2026-10-11
minggu: 2
tags: [aplikasi-sistem-tertanam, komponen, alat-ukur, k3, elektronika-dasar]
status: selesai
sumber:
  - "aplikasi-sistem-tertanam/resource/kampus/Minggu_2._Pengantar_Elektronika_Rangkaian_Alat_Ukur_Multimeter_Alat-alat_pendukung_elektronika_dan_K3.pdf"
---

# Pengantar Elektronika, Rangkaian, Alat Ukur, dan K3

## Ringkasan

Materi ini membahas dasar-dasar elektronika yang mencakup klasifikasi komponen elektronika (aktif dan pasif), simbol dasar diagram skematik, prinsip dasar rangkaian dan tata letak PCB, penggunaan berbagai alat ukur listrik (Voltmeter, Amperemeter, Tang Ampere, AVOMeter/Multimeter, Osiloskop), alat pendukung perakitan, serta standar Keselamatan dan Kesehatan Kerja (K3) dalam praktikum/projek elektronika.

---

## Konsep Kunci

### 1. Komponen Elektronika

![[Excalidraw/peta-komponen-elektronika.excalidraw]]

Komponen elektronika adalah elemen dasar penyusun rangkaian elektronik yang terbagi menjadi dua kategori utama:

#### A. Komponen Aktif
Komponen yang memerlukan sumber arus/tegangan eksternal untuk dapat beroperasi.
- **Dioda**: Dihasilkan dari persambungan semikonduktor tipe-P dan tipe-N.
  - *Kondisi kerja*: Unbiased (tanpa tegangan), Forward Biased (panjar maju), Reverse Biased (panjar mundur).
  - *Jenis*: Dioda Penyearah/Biasa, Dioda Zener (penstabil tegangan/pengaman), LED (indikator cahaya), Photodioda (sensor cahaya), Dioda Schottky, Dioda Bridge, Dioda Varactor.
- **Transistor**: Digunakan sebagai penguat (amplifier), saklar (switching), dan modulasi.
  - *Transistor Bipolar (BJT)*: Memiliki kaki Kolektor (C), Emiter (E), dan Basis (B).
    - **NPN**: Arus mengalir dari C ke E jika Basis diberi arus/tegangan positif.
    - **PNP**: Arus mengalir dari C ke E jika arus mengalir keluar dari Basis ke Ground (input negatif/rendah).
  - *Field Effect Transistor (FET)*: Arus dari Source (S) ke Drain (D) dikendalikan oleh tegangan pada Gate (G). Jenisnya: JFET, MOSFET, UJT.
- **Integrated Circuit (IC)**: Kombinasi ratusan hingga jutaan transistor, resistor, dan komponen lain dalam satu kemasan diskrit.
  - *IC Analog*: Beroperasi pada sinyal gelombang kontinyu (contoh: IC Op-Amp LM358, Regulator LM7805, Penguat Daya LM386).
  - *IC Digital*: Beroperasi pada sinyal biner `0` dan `1` (contoh: Mikrokontroler ATmega328P, IC Counter 4520, IC Memory 24C08).
  - *IC Campuran (Mixed-Signal)*: Menggabungkan sinyal analog dan digital (contoh: ADC0804 untuk konversi Analog-to-Digital).

#### B. Komponen Pasif
Komponen yang dapat beroperasi tanpa memerlukan sumber arus eksternal.
- **Resistor**: Menghambat dan mengatur arus listrik. Satuan: Ohm ($\Omega$).
  - *Resistor Tetap (Fixed)*: Carbon composition, Film, Wirewound.
  - *Resistor Variabel*: Potensiometer, Rheostat, Trimpot, Thermistor (NTC/PTC sensitif suhu), LDR (Light Dependent Resistor sensitif cahaya).
- **Kapasitor**: Menyimpan muatan listrik sementara. Satuan: Farad ($F$).
  - *Kapasitor Nilai Tetap*: Elektrolit (elco - polar), Keramik, Tantalum, Film/Polyester, Kertas, Mika.
  - *Kapasitor Variabel*: VARCO (Variable Condensator), Trimmer.
- **Induktor**: Kumparan kawat tembaga yang menimbulkan medan magnet saat dialiri arus listrik. Satuan: Henry ($H$).
  - *Jenis*: Air Core, Iron Core, Ferrite Core, Variable Core.

---

### 2. Simbol Dasar dan Rangkaian Elektronika

- **Skematik**: Menggunakan simbol standar internasional untuk kabel, bus, saklar (Push Button NO/NC, SPST, DPST), sumber tegangan AC/DC, ground, trafo, sekering, dan komponen aktif/pasif.
- **Rangkaian Flip-Flop** (skematik pada slide, sumber: Skemaku.com): rangkaian astable multivibrator dua transistor yang menghasilkan kondisi $1$ dan $0$ bergantian, ditampilkan lewat dua LED yang menyala bergantian.
  - Komponen: R1 470, R4 470 (pembatas arus LED), R2 10k, R3 10k (bias basis + penentu timing), C1 100uF, C2 100uF (kopling silang), T1 BC547, T2 BC547, LED1, LED2, SW1, baterai 9V.
  - Susunan: simetris kiri-kanan. VCC dari baterai lewat SW1 ke R1/R4 dan R2/R3. C1 hubungkan kolektor T1 ke basis T2, C2 hubungkan kolektor T2 ke basis T1. Emitor T1 dan T2 ke GND.
  - Cara kerja: saat T1 jenuh, kolektor T1 rendah, LED1 menyala, dan C1 menarik basis T2 ke rendah sehingga T2 padam dan LED2 mati. Setelah C1 terisi lewat R3, basis T2 naik, T2 jenuh, LED2 menyala, dan C2 menarik basis T1 ke rendah sehingga T1 padam dan LED1 mati. Siklus berulang selama SW1 tertutup; kecepatan kedip ditentukan R2, R3, C1, C2.
- **Printed Circuit Board (PCB)** (gambar layout pada slide, contoh PCB sistem minimum):
  - *Single Layer*: satu lapis tembaga; terlihat sebagai latar biru penuh (ground pour luas), jalur putih satu lapis, pad hijau oval, footprint DIP dua kolom di tengah, layout renggang.
  - *Double Layer*: dua lapis (atas + bawah); terlihat memakai dua warna jalur (merah = layer top/atas, biru = layer bottom/bawah), pad hijau bulat + pad merah kotak SMD, footprint modul ESP-12E + area antena, layout padat dengan jalur silang antar layer.

---

### 3. Alat Ukur Elektronika

- **Voltmeter**: Mengukur beda potensial/tegangan ($V$) secara **paralel** terhadap beban/sumber.
- **Amperemeter**: Mengukur kuat arus ($I$) secara **seri** memutus jalur Rangkaian.
- **Tang Ampere (Clamp Meter)**: Mengukur arus AC/DC secara non-invasif dengan menjepitkan head clamp pada kabel tanpa memutus rangkaian.
- **AVOMeter / Multimeter**: Alat ukur kombinasi untuk Ampere (arus), Volt (tegangan), dan Ohm (hambatan/resistansi).
- **Osiloskop**: Memetakan sinyal listrik dan frekuensi dalam bentuk grafik 2D (Sumbu-X = Waktu $t$, Sumbu-Y = Tegangan $V$).

---

### 4. Alat Pendukung dan K3 (Keselamatan dan Kesehatan Kerja)

#### Alat Pendukung
- **Solder**: Merekatkan timah/tin pada pad PCB.
- **Testpen**: Mendeteksi keberadaan tegangan listrik.
- **Pinset**: Membantu memegang/menempatkan komponen kecil (SMD/THD).
- **Obeng (Screw Driver)**: Memasang/melepas sekrup.

#### Prosedur K3 Elektronika
1. Menggunakan pakaian kerja/praktik dan sepatu isolator (mencegah terhubung langsung ke ground).
2. Memastikan tangan dalam keadaan kering sebelum menyentuh peralatan listrik.
3. Menggunakan **gelang antistatis** (ESD wrist strap) untuk membuang listrik statis tubuh/sisa arus.
4. Memutus sumber listrik sebelum melakukan modifikasi rangkaian/komponen bertegangan tinggi.
5. Menggunakan **stand holder** saat solder panas tidak digunakan dan jauhkan dari anggota tubuh/kabel.

---

## Contoh dan Penggunaan Alat Ukur

### 1. Pengukuran Tegangan DC (Baterai) dengan Multimeter
1. Tancapkan probe merah ke terminal `V/\Omega` dan probe hitam ke `COM`.
2. Putar saklar selektor ke mode `DCV` pada batas ukur di atas tegangan baterai (misal $20\text{ V}$ untuk baterai $12\text{ V}$).
3. Hubungkan probe merah ke kutub positif ($+$) dan probe hitam ke kutub negatif ($-$).
4. Baca nilai tegangan pada layar display.

### 2. Perhitungan Hukum Ohm pada Rangkaian Sederhana

Hukum Ohm menyatakan besar tegangan pada penghantar berbanding lurus dengan arus yang mengalirinya.[1] Rumusnya $V = I \times R$, atau $I = V/R$ dan $R = V/I$.[2]

Penjelasan tiap variabel (satuan SI + alat ukur):[3]

| Variabel | Arti | Satuan | Alat ukur |
|---|---|---|---|
| $V$ | Tegangan / beda potensial — "tekanan" yang mendorong elektron bergerak | volt (V) | voltmeter |
| $I$ | Kuat arus — jumlah muatan yang mengalir per satuan waktu | ampere (A); $1\text{ mA} = 0{,}001\text{ A}$ | amperemeter |
| $R$ | Hambatan / resistansi — seberapa besar material melawan aliran arus | ohm ($\Omega$); $1\text{ k}\Omega = 1000\ \Omega$ | ohmmeter |
| $\Delta V$ | Tegangan yang jatuh pada resistor = selisih tegangan sumber dan beban | volt (V) | — |
| $V_{CC}$ | Tegangan sumber/catu daya rangkaian | volt (V) | — |
| $V_{LED}$ ($V_f$) | Tegangan maju (forward voltage) LED — tegangan jatuh saat LED menyala; LED merah tipikal $1{,}8$–$2{,}2\text{ V}$, diambil $2{,}0\text{ V}$ sebagai contoh.[4][5] Cek datasheet LED spesifik untuk nilai pasti.[5] | volt (V) | — |

Contoh: LED merah dengan arus maju $I = 20\text{ mA} = 0{,}02\text{ A}$ dan $V_{LED} = 2{,}0\text{ V}$, dipasok $V_{CC} = 5\text{ V}$. Resistor seri pembatas arus dihitung dengan $R = (V_S - V_f)/I_f$:[4]

$$\Delta V = V_{CC} - V_{LED} = 5\text{ V} - 2\text{ V} = 3\text{ V}$$

$$R = \frac{\Delta V}{I} = \frac{3\text{ V}}{0{,}02\text{ A}} = 150\ \Omega$$

Catatan: $20\text{ mA}$ adalah arus tipikal LED indikator standar.[4][5] Nilai $150\ \Omega$ cocok dengan perhitungan untuk LED merah pada catu $5\text{ V}$.[5]

## Sources

[1] https://id.wikipedia.org/wiki/Hukum_Ohm — Hukum Ohm - Wikipedia bahasa Indonesia
[2] https://www.kompas.com/skola/read/2022/05/25/103000069/bunyi-hukum-ohm-dan-contoh-penerapannya — Bunyi Hukum Ohm - Kompas Skola
[3] https://homiwork.com/id/app/materials/detail/505 — Hukum Ohm - Satuan dan Alat Ukur
[4] https://aionlinecalculator.com/led-resistor-calculator.html — LED Resistor Calculator
[5] https://www.schemalyzer.com/en/tools/led-resistor-calculator — LED Resistor Calculator Schemalyzer

---

## Keterkaitan

- **Prasyarat**: Fisika Dasar (Listrik & Magnet), Konsep Dasar Arus/Tegangan ($V = I \times R$).
- **Lanjutan**: [[aplikasi-sistem-tertanam/materi/desain-skematik-dan-pcb|Desain Skematik dan PCB]], [[aplikasi-sistem-tertanam/materi/pemrograman-mikrokontroler|Pemrograman Mikrokontroler ATmega328P]].
