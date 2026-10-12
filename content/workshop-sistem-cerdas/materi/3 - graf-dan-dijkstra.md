---
matkul: workshop-sistem-cerdas
topik: Graf dan Dijkstra
urutan: 3
tanggal: 2026-10-12
minggu: 2
tags: [workshop-sistem-cerdas, graf, dijkstra, shortest-path, adjacency]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/Knowledge_Representation_-_Graph_Dijkstra_-_Share.pdf"
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas-Minggu_2.pdf"
---

# Graf dan Dijkstra

## Ringkasan

Graf wakili pengetahuan relasional dan ruang keadaan search. Algoritma Dijkstra cari jarak terpendek di graf berbobot (bobot non-negatif) dari satu sumber ke semua simpul. Materi: definisi formal $G = (V, E)$, jenis graf, inisialisasi, relaksasi, dua contoh hitung (SF-Boston = 2715, B-E-C-F = 9.1), peta graf B-F.

---

## Konsep Kunci

### 1. Definisi formal

$$G = (V, E)$$

$G$ graf, $V$ himpunan simpul (vertex/node), $E$ himpunan sisi (edge) hubungkan dua simpul. Contoh: $V = \{A,B,C,D,E\}$, $E = \{AB, BC, CA, AD\}$.

### 2. Jenis graf

- **Directed (digraph)**: sisi berpanah satu arah; $A \to B$ tak berarti $B \to A$.
- **Undirected**: sisi tanpa arah, dua arah.
- **Weighted**: sisi berlabel bobot; panjang lintasan = jumlah bobot. Contoh $a \to b \to c \to d \to e \to g$ = $5+4+5+6+5 = 25$.
- **Unweighted**: tanpa bobot; panjang = jumlah sisi.
- **Cyclic**: ada siklus. **Acyclic**: tanpa siklus (pohon salah satunya).

### 3. Algoritma Dijkstra: inisialisasi

Jarak sumber ke diri sendiri $d(s) = 0$, ke semua lain $d(v) = \infty$. Tandai sumber dikunjungi. Ulangi sampai semua dikunjungi atau tujuan ketemu.

### 4. Relaksasi

Untuk tiap tetangga $v$ simpul kini $u$ dengan bobot $w(u,v)$, hitung jarak sementara $d(u) + w(u,v)$. Bila lebih kecil dari $d(v)$ tersimpan, perbarui:

$$d(v) = \min(d(v), d(u) + w(u,v))$$

Lalu pilih simpul belum dikunjungi dengan $d$ terkecil sebagai simpul kini. Syarat optimal: bobot non-negatif.

### 5. Metode graf terkait

Shortest path: Dijkstra, A*. Traversal: BFS, DFS.

---

## Contoh (dengan perhitungan)

### SF ke Boston (jarak terpendek 2715)

SF proses tetangga: LA = 349. LA → Denver $349 + 834 = 1183$ (lebih pendek dari LA → NY $349 + 2451 = 2800$) → Denver 1183. Denver → Chicago $1183 + 908 = 2091$, tapi langsung SF → Chicago 1855 lebih pendek → Chicago 1855. Chicago → Atlanta $1855 + 606 = 2461$ (vs Chicago → NY $1855 + 722 = 2577$, Chicago → Boston $1855 + 860 = 2715$) → Atlanta 2461. Atlanta → Miami $2461 + 595 = 3056$, Atlanta → NY $2461 + 760 = 3221$; Chicago → Boston 2715 paling pendek. Rute menang: **SF – Chicago – Boston = $1855 + 860 = 2715$**.

### B ke F (total 9.1, 6 iterasi)

Bobot: B-A 3, B-D 3.5, B-E 2.8, E-C 2.8, E-D 3.1, E-G 7, A-C 3, D-G 10, C-F 3.5 F-G 2.5.

| Iterasi | Kunjungi | Update                                                         |
| ------- | -------- | -------------------------------------------------------------- |
| 0       | —        | `{A:∞, B:0, C:∞, D:∞, E:∞, F:∞, G:∞}`                          |
| 1       | B        | A=3, D=3.5, E=2.8                                              |
| 2       | E        | C=$2.8+2.8=5.6$, D coba $2.8+3.1=5.9>3.5$ tolak, G=$2.8+7=9.8$ |
| 3       | A        | C coba $3+3=6>5.6$ tolak                                       |
| 4       | D        | G coba $3.5+10=13.5>9.8$ tolak                                 |
| 5       | C        | F=$5.6+3.5=9.1$                                                |
| 6       | F        | tujuan, selesai                                                |

Rute menang: **B → E → C → F = $2.8 + 2.8 + 3.5 = 9.1$**.

![[Excalidraw/graf-dijkstra-be-cf.excalidraw]]

---

## Keterkaitan

- Prasyarat: [[workshop-sistem-cerdas/materi/2 - representasi-pengetahuan|Representasi Pengetahuan]]
- Lanjutan: [[workshop-sistem-cerdas/materi/4 - pencarian-a-star|Pencarian A-Star]]
