---
matkul: workshop-sistem-cerdas
topik: Parse Tree dan TF-IDF
urutan: 19
tanggal: 2026-10-12
tags: [workshop-sistem-cerdas, nlp, parse-tree, cfg, derivasi, tf-idf]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas_Sistem_Pakar_dan_NLP.pdf"
---

# Parse Tree dan TF-IDF

## Ringkasan

Dua fondasi NLP: (1) **parse tree** — pohon penurunan yang menyimpan rule CFG dan cara pencocokannya; string diturunkan dari variabel ke terminal via leftmost atau rightmost derivation; soal BKPM menurunkan "baabaab" dari $S \to AB$, $A \to Aa \mid bB$, $B \to a \mid Sb$. (2) **TF-IDF** — bobot tiap kata $W = TF \times IDF$; kata yang tersebar di banyak dokumen bobotnya kecil (mis. "saya" → $IDF = 0$). Hitungan 3 artikel BKPM: koreksi — $0{,}25 \times 0{,}1761 = \mathbf{0{,}0440}$, bukan 0,440 seperti tertulis di BKPM.

---

## Konsep Kunci

### 1. Parse tree + CFG

- Parse tree: tiap node = kata input atau nonterminal grammar; tiap level = penerapan satu rule.
- Context Free Grammar (CFG) = dasar parser/analisis sintaksis; bagian sintaks kompiler umumnya didefinisikan dalam tata bahasa bebas konteks.
- Penurunan: simbol variabel diperluas sampai semua terminal.
  - **Leftmost**: variabel terkiri dulu.
  - **Rightmost**: variabel terkanan dulu.

### 2. Rumus TF–IDF

$$TF = \frac{\text{jumlah kemunculan term di dokumen}}{\text{jumlah kata dokumen}}, \quad IDF = \log_{10}\frac{N}{DF}, \quad W = TF \times IDF$$

$N$ = jumlah dokumen, $DF$ = jumlah dokumen yang memuat term. Logaritma menekan bilangan besar (mis. $\log(1000) = 3$). Intuisi: sering di satu dokumen tapi jarang di korpus → bobot besar; muncul di semua dokumen → $IDF = 0$ → bobot 0.

---

## Contoh (dengan perhitungan)

### A. Leftmost derivation "baabaab" (aturan $S \to AB$; $A \to Aa \mid bB$; $B \to a \mid Sb$)

$$S \Rightarrow AB \Rightarrow AaB \Rightarrow bBaB \Rightarrow baaB \Rightarrow baaSb \Rightarrow baaABb \Rightarrow baabBBb \Rightarrow baabaBb \Rightarrow baabaab$$

(9 langkah; penurunan terkanan menghasilkan pohon yang sama, mis. $S \Rightarrow AB \Rightarrow ASb \Rightarrow \ldots \Rightarrow baabaab$.)

### B. Latihan 1 — $S \to A1B$; $A \to 0A \mid \epsilon$; $B \to 0B \mid 1B \mid \epsilon$; $w = 00101$

Leftmost: $S \Rightarrow A1B \Rightarrow 0A1B \Rightarrow 00A1B \Rightarrow 001B \Rightarrow 0010B \Rightarrow 00101B \Rightarrow 00101$. Rightmost: $S \Rightarrow A1B \Rightarrow A10B \Rightarrow A101B \Rightarrow A101$ lalu $A \Rightarrow^* 00$ dari kiri. Parse tree: $S$ → $A(0,0)$ + $1$ + $B(0,1)$.

### C. Latihan 2 — $S \to aAB$; $A \to aA \mid b$; $B \to bB \mid \epsilon$; $w = aabb$

Leftmost: $S \Rightarrow aAB \Rightarrow aaAB \Rightarrow aabB \Rightarrow aabbB \Rightarrow aabb$. Rightmost: $S \Rightarrow aAB \Rightarrow aAbB \Rightarrow aAbb \Rightarrow aaAb \Rightarrow aabb$. Parse tree: $S$ → $a$ + $A(a,b)$ + $B(b)$.

### D. TF–IDF 3 artikel (A1 "saya suka makan nasi", A2 "saya suka makan ayam", A3 "saya suka minum kopi")

TF tiap kata = 1/4 = 0,25 (tiap kata muncul sekali per 4 kata). DF: saya 3, suka 3, makan 2, nasi/ayam/minum/kopi 1. IDF ($\log_{10}$): saya 0, suka 0, makan 0,1761, sisanya 0,4771.

| Artikel | saya | suka | makan | nasi | ayam | minum | kopi |
|---|---|---|---|---|---|---|---|
| A1 | 0 | 0 | **0,0440** | 0,119 | 0 | 0 | 0 |
| A2 | 0 | 0 | **0,0440** | 0 | 0,119 | 0 | 0 |
| A3 | 0 | 0 | 0 | 0 | 0 | 0,119 | 0,119 |

⚠️ **Koreksi BKPM:** BKPM menulis $0{,}25 \times 0{,}1761 = 0{,}440$ — benar $0{,}25 \times 0{,}1761 = 0{,}0440$ (naskah salah penempatan koma desimal; sel "makan" di tabel gabungan ikut terkoreksi). Nilai $0{,}25 \times 0{,}4771 = 0{,}119$ sudah benar.

Kesimpulan: kata di semua dokumen ("saya") tak membedakan ($IDF = 0$); kata langka ("nasi", "kopi") paling pembeda.

---

## Keterkaitan

- Prasyarat: [[nlp-dasar]] (alur NLP, sintaksis/semantik tempat parse tree dan TF-IDF dipakai)
- Lanjutan: —
