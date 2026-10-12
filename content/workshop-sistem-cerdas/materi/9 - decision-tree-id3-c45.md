---
matkul: workshop-sistem-cerdas
topik: Decision Tree ID3 & C4.5
urutan: 9
tanggal: 2026-10-12
tags:
  - workshop-sistem-cerdas
  - decision-tree
  - id3
  - c45
  - klasifikasi
status: selesai
sumber:
  - workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas__SVM__Decision_Tree_Rev.pdf
---

# Decision Tree ID3 & C4.5

## Ringkasan

Decision tree adalah model klasifikasi berstruktur pohon/hierarki: alur ditelusuri dari simpul akar ke simpul daun yang memegang prediksi kelas, lalu mudah dikonversi menjadi aturan IF–THEN. ID3 (Quinlan, 1979; Iterative Dichotomiser 3, berbasis teori informasi Shannon) memilih atribut split lewat information gain terbesar. C4.5 adalah pengembangan ID3 yang menangani atribut kontinu, missing value, pruning, dan memilih split lewat gain ratio. Studi kasus BKPM (PlayTennis, 14 record) menghasilkan root Outlook dengan 7 aturan.

![[Excalidraw/pohon-keputusan-playtennis.excalidraw]]

---

## Konsep Kunci

### 1. Data sebagai tabel atribut–record

- Atribut = parameter kriteria pembentukan tree (contoh: cuaca, angin, temperatur untuk main tenis).
- Instance = nilai atribut (contoh: cuaca → cerah, berawan, hujan).
- Record = satu baris data; atribut identitas murni (seperti Day) tidak dipakai sebagai pemisah.

### 2. Tiga jenis node

1. **Root node**: simpul paling atas.
2. **Internal node**: simpul percabangan — satu input, minimal dua output.
3. **Leaf node**: simpul akhir — satu input, tanpa output; menandai label kelas.

### 3. ID3: langkah + gain + bias banyak-nilai

- Langkah: (1) mulai dari simpul akar berisi sampel latih; (2) bila semua sampel sekelas → jadi daun; bila tidak, pilih atribut pemisah via information gain; (3) buat cabang per nilai atribut, partisi data; (4) rekursif per partisi, atribut terpakai tak dipakai lagi di bawahnya; (5) berhenti bila murni atau atribut habis (pakai suara terbanyak).
- Entropy $= 0$ bila semua contoh sekelas; $= 1$ bila positif–negatif sama banyak; di antaranya bila tak sama.
- Information gain memilih atribut dengan penurunan entropy terbesar, tetapi **bias ke atribut bernilai banyak** (contoh: product_ID sebagai unique identifier menghasilkan keluaran sangat banyak yang masing-masing satu tuple) — inilah yang diperbaiki C4.5.

### 4. C4.5: kontinu, missing, pruning, gain ratio

- **Atribut kontinu**: didiskritisasi dulu — misal binning (rentang 0–100 → 4 bin 0..24/25..49/50..74/75..100, unsupervised discretization) atau normalisasi via rerata–standar deviasi.
- **Missing value**: diisi nilai terbanyak atribut itu (contoh BKPM: atribut Shape yang kosong diisi mayoritas kelompok kelasnya).
- **Pruning** (atasi noise/overfitting): prepruning (henti dini bila partisi tak signifikan, misal uji chi-squared), postpruning standar C4.5 (buang cabang setelah pohon jadi), reduced error pruning (bandingkan error rate sebelum/sesudah pangkas pada test data).
- **Gain ratio** sebagai kriteria split menggantikan information gain.

### 5. CART vs ID3/C4.5

| Aspek | ID3 | C4.5 | CART (sklearn) |
|-------|-----|------|----------------|
| Kriteria split | Information gain (entropy) | Gain ratio (gain ternormalisasi) | Gini (default) atau entropy bila `criterion='entropy'` |
| Cabang | Multiway (sebanyak kategori) | Multiway | Selalu biner |
| Fitur kategorikal | Native (label langsung) | Native | Harus di-encode angka (LabelEncoder) |
| Missing value | Tidak ditangani | Otomatis | Tidak otomatis |
| Pruning | Tidak ada | Post-pruning otomatis | Manual (`max_depth`, `ccp_alpha`, dll) |

Koreksi penting BKPM: kode `sklearn.tree.DecisionTreeClassifier` **bukan** ID3/C4.5 melainkan CART.

### 6. Rumus

$$Entropy(S) = -\sum_i p_i \log_2(p_i)$$
$$Gain(S,A) = Entropy(S) - Entropy(S|A)$$
$$SplitInfo(S,A) = -\sum_i \frac{|S_i|}{|S|} \log_2\frac{|S_i|}{|S|}$$
$$GainRatio(S,A) = \frac{Gain(S,A)}{SplitInfo(S,A)}$$

---

## Contoh (dengan perhitungan)

### Dataset PlayTennis (14 record)

| Day | Outlook | Temperature | Humidity | Wind | PlayTennis |
|-----|----------|-------------|----------|--------|------------|
| D1 | Sunny | Hot | High | Weak | No |
| D2 | Sunny | Hot | High | Strong | No |
| D3 | Overcast | Hot | High | Weak | Yes |
| D4 | Rain | Mild | High | Weak | Yes |
| D5 | Rain | Cool | Normal | Weak | Yes |
| D6 | Rain | Cool | Normal | Strong | No |
| D7 | Overcast | Cool | Normal | Weak | Yes |
| D8 | Sunny | Mild | High | Weak | No |
| D9 | Sunny | Cool | Normal | Weak | Yes |
| D10 | Rain | Mild | Normal | Strong | Yes |
| D11 | Sunny | Mild | Normal | Strong | Yes |
| D12 | Overcast | Mild | High | Strong | Yes |
| D13 | Overcast | Hot | Normal | Weak | Yes |
| D14 | Rain | Mild | High | Strong | No |

### Entropy awal = 0,9403

9 Yes, 5 No:

$$Entropy(S) = -(9/14)\log_2(9/14) - (5/14)\log_2(5/14) = 0{,}9403$$

### Gain ratio root: Outlook menang (0,1564)

- Outlook (Sunny 2Y/3N, Overcast 4Y/0N, Rain 3Y/2N): $Entropy$ Sunny/Rain $= 0{,}9710$, Overcast $= 0$; $Entropy(S|Outlook) = 0{,}6935$; $Gain = 0{,}2467$; $SplitInfo = 1{,}5774$; **$GainRatio = 0{,}1564$**.
- Temperature: $Gain = 0{,}0292$, $SplitInfo = 1{,}5567$, $GR = 0{,}0188$.
- Humidity (High 3Y/4N, Normal 6Y/1N): $Gain = 0{,}1518$, $SplitInfo = 1{,}0000$, $GR = 0{,}1518$.
- Wind (Weak 6Y/2N, Strong 3Y/3N): $Gain = 0{,}0481$, $SplitInfo = 0{,}9852$, $GR = 0{,}0488$.

ROOT = **Outlook**. Cabang Overcast murni Yes → leaf. Sunny dipecah Humidity ($GR = 1{,}0000$): High → No, Normal → Yes. Rain dipecah Wind ($GR = 0{,}4325$): Weak → Yes; Strong dipecah Temperature (tie $GR = 0{,}2740$ dengan Humidity, dipilih Temperature): Cool → No; Mild dipecah Humidity: High → No, Normal → Yes.

### 7 aturan IF–THEN

1. IF Outlook = Overcast THEN Yes
2. IF Outlook = Sunny AND Humidity = High THEN No
3. IF Outlook = Sunny AND Humidity = Normal THEN Yes
4. IF Outlook = Rain AND Wind = Weak THEN Yes
5. IF Outlook = Rain AND Wind = Strong AND Temperature = Cool THEN No
6. IF Outlook = Rain AND Wind = Strong AND Temperature = Mild AND Humidity = High THEN No
7. IF Outlook = Rain AND Wind = Strong AND Temperature = Mild AND Humidity = Normal THEN Yes

---

## Keterkaitan

- Lanjutan dari: [[workshop-sistem-cerdas/materi/2 - representasi-pengetahuan|Representasi Pengetahuan]] (cabang tree sebagai representasi pengetahuan).
- Mandiri terhadap: [[workshop-sistem-cerdas/materi/10 - support-vector-machine|Support Vector Machine]] (satu sumber, topik klasifikasi berbeda).
