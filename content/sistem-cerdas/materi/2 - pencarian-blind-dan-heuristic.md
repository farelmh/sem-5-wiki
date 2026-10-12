---
matkul: sistem-cerdas
topik: Pencarian dan Penalaran - Blind Search dan Heuristic Search
urutan: 2
tanggal: 2026-10-12
minggu: 4
tags: [sistem-cerdas, search, blind-search, heuristic-search, bfs, dfs, ucs, greedy, a-star]
status: selesai
sumber:
  - "sistem-cerdas/resource/kampus/SISTEM_CERDAS_-_Pertemuan_4.pdf"
---

# Pencarian dan Penalaran - Blind Search dan Heuristic Search

## Ringkasan

Sistem cerdas tidak cukup punya pengetahuan; butuh strategi temukan solusi dari banyak kemungkinan. Materi ini bahas search algorithm sebagai mekanisme ambil keputusan di ruang solusi: representasi state, Blind Search (BFS, DFS, UCS), Heuristic Search (Greedy, A*), syarat heuristic admissible, dan penerapan route searching. Inti: $f(n) = g(n) + h(n)$.

---

## Konsep Kunci

### 1. Masalah: robot cari tujuan

- Robot di titik awal A, harus capai tujuan G, ada beberapa jalur dan obstacle.
- Ini path finding: tentukan urutan langkah dari awal ke tujuan tanpa tabrak penghalang.
- Aplikasi sama: navigasi robot, GPS, kendaraan otonom, game AI, motion planning.
- Pertanyaan inti: dari banyak tindakan mungkin, mana dipilih agar capai tujuan?

### 2. Representasi state

![[Excalidraw/peta-search-tree-blind-heuristic.excalidraw]]

- Masalah dimodelkan sebagai search tree, contoh: A → B, C, D → E, F, G.
- Komponen:
  - **Initial state**: kondisi awal.
  - **State**: kondisi mungkin dicapai.
  - **Action**: tindakan pindah antar state.
  - **Goal state**: kondisi target.
  - **Path**: urutan state dari awal sampai tujuan, contoh solusi: A → D → G.
- Search = eksplorasi state space, bukan sekadar "cari data".

### 3. Dua kelompok algoritma

- **Blind Search / Uninformed**: tanpa info tambahan soal kedekatan node ke tujuan. Contoh: BFS, DFS, UCS.
- **Heuristic Search / Informed**: pakai heuristic untuk arahkan pencarian. Contoh: Greedy Best-First, A*.
- Analogi: blind = cari alamat di kota asing tanpa peta, coba jalan berdasar aturan; heuristic = pakai estimasi jarak ke tujuan, pencarian lebih cerdas.

### 4. BFS (Breadth-First Search)

- Bergerak per level: Level 0 → Level 1 → Level 2 → dst.
- Contoh pohon A / B C D / E F (di bawah B) dan G (di bawah D): urutan cek A, lalu B, C, D, lalu E, F, G.
- Struktur data: Queue, FIFO.
- Plus: temukan solusi langkah minimum bila tiap langkah biaya sama.
- Minus: memori besar, karena banyak node satu level disimpan sebelum lanjut.
- Cocok: jalur terpendek di graf tiap edge biaya sama.

### 5. DFS (Depth-First Search)

- Lawan BFS: masuk sedalam mungkin satu cabang, lalu backtracking coba cabang lain.
- Contoh: dari A masuk B, lalu E atau F, mentok, backtrack, coba cabang lain.
- Struktur data: Stack, LIFO.
- Plus: memori relatif lebih kecil dari BFS pada kondisi tertentu.
- Minus: tidak jamin optimal. Contoh: Jalur 1 = 5 langkah, Jalur 2 = 20 langkah; DFS bisa temukan Jalur 2 dulu bila cabang itu dieksplorasi lebih awal.
- Prinsip: DFS cari secara mendalam, bukan berdasar cost minimum.

### 6. UCS (Uniform Cost Search)

- Pilih node berdasar total biaya tempuh (path cost), bukan level/kedalaman.
- Konsep: $g(n)$ = biaya aktual dari awal menuju node $n$.
- Struktur data: Priority Queue; node biaya terendah diprioritaskan.
- Contoh: A→B→D = $2+2=4$ vs A→C→D = $5+1=6$; UCS pilih jalur pertama.

### 7. Mengapa perlu heuristic

- Bila kemungkinan jalur sangat besar (ribuan persimpangan kota), cek semua node tanpa info tambahan = mahal.
- Heuristic: $h(n)$ = estimasi biaya dari node $n$ menuju tujuan, misal estimasi jarak robot ke G.
- Makin kecil $h(n)$, node dipandang makin dekat tujuan.
- Catatan: heuristic itu estimasi, bukan biaya sebenarnya. Kualitas heuristic tentukan performa pencarian.

### 8. Greedy Best-First Search

- Rumus: $f(n) = h(n)$. Hanya tanya: node mana terlihat paling dekat tujuan?
- Contoh: $h(B)=8$, $h(C)=3$; greedy pilih C karena $3 < 8$.
- Plus: cepat dan terarah.
- Minus: tidak selalu optimal; node terlihat dekat belum tentu hasilkan total biaya terendah.

### 9. A* Search

- Gabung dua info: biaya sudah keluar + estimasi sisa.
- Rumus: $f(n) = g(n) + h(n)$.
- Contoh: B: $g=2$, $h=6$, $f=8$; C: $g=4$, $h=2$, $f=6$; A* pilih C karena $6 < 8$.
- A* tidak hanya tanya "mana paling dekat?", tapi "berapa biaya sudah keluar + kira-kira sisa berapa?"
- Populer untuk path finding dan route planning.

### 10. Admissible heuristic

- Syarat A* optimal salah satunya: $h(n) \leq h^*(n)$; estimasi tidak boleh overestimate biaya sebenarnya.
- Contoh: biaya sebenarnya A→G = 10. $h(A)=7$ masih di bawah → OK. $h(A)=15$ overestimate → tidak admissible.
- Kualitas heuristic tentukan kualitas dan efisiensi pencarian.

### 11. Penerapan route searching

- Contoh ilustrasi: lokasi seperti Jember, Ajung, Ambulu, Tanggul, dan tujuan.
- Berlaku untuk: navigasi robot, GPS, game AI, route optimization, autonomous vehicle.
- Aturan pilih: UCS bila fokus biaya aktual; A* bila mau pencarian terarah pakai estimasi jarak.

### 12. Tabel perbandingan

| Algoritma | Fokus | Ingatan singkat |
|---|---|---|
| BFS | Level/kedalaman | Cari berdasar level |
| DFS | Kedalaman | Masuk sedalam mungkin |
| UCS | $g(n)$ | Pilih biaya aktual terkecil |
| Greedy | $h(n)$ | Pilih yang terlihat paling dekat |
| A* | $g(n) + h(n)$ | Biaya tempuh + estimasi sisa |

- Alur belajar: Masalah → State Space → Search Tree → Blind Search → Heuristic → Heuristic Search → A* → Solusi → Evaluasi Efisiensi.
- Kecerdasan search bukan sekadar temukan solusi, tapi temukan secara efektif dan efisien.

---

## Contoh (dengan perhitungan)

### UCS pilih jalur murah (dari materi)

- A→B→D: $2 + 2 = 4$.
- A→C→D: $5 + 1 = 6$.
- UCS pilih A→B→D karena $4 < 6$.

### Greedy pilih estimasi terkecil (dari materi)

- $h(B) = 8$, $h(C) = 3$.
- $f(B) = 8$, $f(C) = 3$. Pilih C karena $3 < 8$.
- Risiko: belum tentu total jalur via C paling murah.

### A* gabung cost + estimasi (dari materi)

- B: $g = 2$, $h = 6$ → $f(B) = 2 + 6 = 8$.
- C: $g = 4$, $h = 2$ → $f(C) = 4 + 2 = 6$.
- Pilih C karena $f(C) = 6 < f(B) = 8$.

### Cek admissible (dari materi)

- Biaya sebenarnya A→G: $h^*(A) = 10$.
- $h(A) = 7$: $7 \leq 10$ → admissible.
- $h(A) = 15$: $15 > 10$ → tidak admissible, A* bisa tidak optimal.

---

## Keterkaitan

- Prasyarat: [[sistem-cerdas/materi/1 - pengantar-kecerdasan-buatan|Pengantar Kecerdasan Buatan]]
- Lanjutan: game AI, sistem pakar, automatic planning, optimasi, robotika, navigasi, sistem pendukung keputusan (belum ada halaman)
