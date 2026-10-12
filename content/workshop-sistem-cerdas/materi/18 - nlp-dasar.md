---
matkul: workshop-sistem-cerdas
topik: NLP Dasar
urutan: 18
tanggal: 2026-10-12
tags: [workshop-sistem-cerdas, nlp, machine-learning, sintaksis, semantik, analisis-sentimen]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas_Sistem_Pakar_dan_NLP.pdf"
---

# NLP Dasar

## Ringkasan

Natural Language Processing (NLP) adalah cabang AI untuk pemrosesan bahasa alami agar mesin memahami, menganalisis, dan menginterpretasikan bahasa manusia — menggabungkan linguistik komputasi dengan statistik, machine learning, dan deep learning. Alur garis besarnya 4 langkah: text vectorization → data latih + output harapan → analisis statistik/prediksi → makin banyak data makin akurat. Dua pendekatan algoritma: rules-based vs machine-learning-based; dua analisis utama: sintaksis (5 teknik) dan semantik (3 teknik), yang menurunkan teknik seperti PoS tagging, lemmatization, tokenization, NER, dan text classification.

![[Excalidraw/alur-nlp-vectorize-prediksi.excalidraw]]

---

## Konsep Kunci

### 1. Definisi + contoh

NLP memproses teks maupun audio untuk tugas seperti penerjemahan, pengenalan suara, rangkuman otomatis, segmentasi topik, text-to-speech, chatbot, deteksi spam, analisis sentimen. Contoh BKPM: "We need to address this issue" tanpa NLP → "Kita butuh ke alamat isu ini"; dengan NLP → "Kita harus membahas masalah ini" (konteks tertangkap).

### 2. Alur 4 langkah

1. **Text vectorization**: kata → sekumpulan angka yang dimengerti mesin.
2. **Data percobaan + output harapan**: mis. teks "saya suka produk ini" → output "Ramah".
3. **Analisis statistik**: bangun wawasan dari input; prediksi output teks berikutnya.
4. **Skala data**: makin banyak data → makin akurat (lebih banyak referensi belajar).

Hasil contoh: "Produk ini sangat membantu saya" (Ramah); "Produknya bisa berfungsi dengan baik, tapi tidak istimewa" (Netral); "Saya merasa rugi telah membeli produk ini" (Tidak ramah).

### 3. Rules-based vs ML-based

- **Rules-based**: aturan linguistik rancangan teliti; dipakai sejak awal NLP sampai kini.
- **Machine-learning-based**: metode statistik; belajar dari data pelatihan, menyesuaikan algoritma saat data bertambah; kombinasi ML, deep learning, neural network; mengasah aturannya sendiri lewat pemrosesan berulang.

Dua tahap kerja: data preprocessing (siapkan/bersihkan teks, ambil fitur) → pengembangan algoritma.

### 4. Analisis sintaksis (5 teknik)

Sintaksis = susunan kata pembentuk arti gramatikal.

1. **Parsing**: analisis gramatikal + part-of-speech; "Ayam berkokok" → ayam = kata benda, berkokok = kata kerja.
2. **Word segmentation**: turunkan kata dari string teks; kenali pemisah spasi (mis. dokumen tulisan tangan terpindai).
3. **Sentence breaking**: batas kalimat; "Ayam berkokok. Saya bangun." → titik memecah dua kalimat.
4. **Morphological segmentation**: pecah kata jadi morfem; "ketidakpastian" → [[ke][[tidak][pasti]][an]].
5. **Stemming**: imbuhan → kata dasar; "berkokok" → "kokok".

### 5. Analisis semantik (3 teknik)

Semantik = fungsi dan makna di balik kata.

1. **Word sense disambiguation**: arti menurut konteks; "bisa ular dapat berakibat fatal…" → "bisa" = racun.
2. **Named entity recognition (NER)**: kategorikan kata ke kelompok; bedakan entitas yang visualnya sama (perusahaan/produk dalam berita).
3. **Natural language generation (NLG)**: hasilkan frasa/kalimat bermakna dari data terstruktur (artikel, tweet) ribuan halaman per detik.

### 6. Teknik turunan

- **PoS tagging** (sintaktis): tag peran tiap kata; "Kemarin, saya membeli laptop" → keterangan waktu / subjek / kata kerja / kata benda.
- **Lemmatization** (semantis): kata → bentuk dasar; "udah… ngerasa" → "sudah… rasa".
- **Tokenization**: bagi elemen besar ke kecil; "Saya merasa puas dengan produk ini" → "Saya" "Merasa" "Puas" "Dengan" "Produk" "Ini".
- **NER** (relasi): "Budi bekerja di Bitlabs" → Budi–Bitlabs berhubungan via kata kerja.
- **Text classification** (semantis): himpunan teks → tag; tag "Ramah/Netral/Tidak ramah" untuk analisis sentimen review/komentar.

---

## Contoh (dengan perhitungan)

BKPM menyertakan kode Python NLTK untuk tokenization, lemmatization, dan stemming (halaman lampiran, tanpa angka untuk dihitung). Contoh kerja text classification: beri data latih ("saya suka produk ini" → "Ramah"), mesin bangun model statistik, lalu teks baru "Produk ini sangat membantu saya" diprediksi "Ramah" — akurasi naik seiring volume data (langkah 4 alur).

---

## Keterkaitan

- Prasyarat: —
- Lanjutan: [[parse-tree-dan-tf-idf]] (parse tree/CFG dan pembobotan kata TF-IDF)
