---
matkul: workshop-sistem-cerdas
topik: Pencarian A-Star
urutan: 4
tanggal: 2026-10-12
minggu: 2
tags: [workshop-sistem-cerdas, a-star, heuristic-search, dijkstra, shortest-path]
status: selesai
sumber:
  - "workshop-sistem-cerdas/resource/kampus/BKPM_WSistem_Cerdas-Minggu_2.pdf"
---

# Pencarian A-Star

## Ringkasan

Ruang keadaan besar bikin blind search (BFS/DFS) boros karena telusuri hampir semua simpul. A* gabung jaminan optimal Dijkstra/UCS (biaya nyata) + efisiensi Greedy (arahan heuristik). Fungsi evaluasi $f(n) = g(n) + h(n)$; tiap iterasi ekspansi $f$ terkecil di open list. Heuristik admissible (tak overestimate) jamin optimal, simpul dieksplorasi lebih sedikit. Tugas: hitung manual + verifikasi Python.

---

## Konsep Kunci

### 1. Blind vs heuristic search

- **Blind (BFS/DFS)**: tanpa info kedekatan ke tujuan; telusuri buta, mahal di state space besar.
- **Heuristic**: pakai $h(n)$ = estimasi biaya/jarak simpul $n$ ke tujuan; dahulukan simpul menjanjikan. Contoh $h$: jarak garis lurus (Euclidean) ke tujuan.

### 2. Fungsi evaluasi A*

$$f(n) = g(n) + h(n)$$

$g(n)$ biaya nyata awal → $n$; $h(n)$ estimasi $n$ → tujuan; $f(n)$ estimasi total lintasan terbaik lewat $n$, penentu ekspansi. UCS/Dijkstra = kasus khusus $h(n) = 0$; Greedy = kasus khusus $g(n) = 0$.

### 3. Admissible

$h(n)$ admissible bila tak pernah overestimate biaya sebenarnya ke tujuan ($h(n) \le h^*(n)$). Jarak garis lurus admissible untuk rute jalan. Admissible → A* optimal.

### 4. Langkah open/closed list

1. Open = {awal}, closed = {}.
2. Selama open tak kosong, ambil $n$ dengan $f(n) = g(n) + h(n)$ terkecil.
3. Bila $n$ tujuan → berhenti, rekonstruksi lintasan via parent.
4. Bukan tujuan → pindah $n$ ke closed, evaluasi semua successor.
5. Tiap successor: hitung $g$, $f$. Bila belum di open, atau $g$ baru lebih kecil → perbarui $g$, $f$, parent, masukkan/perbarui di open.
6. Ulangi sampai tujuan ketemu atau open kosong (tak ada solusi).

---

## Contoh (dengan perhitungan)

### SF ke Boston pakai A*

Graf penerbangan sama seperti Dijkstra + $h(n)$ garis lurus ke Boston (admissible). A* evaluasi $f(n) = g(n) + h(n)$ tiap simpul dikunjungi, selalu ekspansi $f$ terkecil. Hasil rute + total sama seperti Dijkstra: **SF – Chicago – Boston = 2715**, tapi simpul dieksplorasi lebih sedikit karena $h$ arahkan pencarian ke timur.

### Mini-hitungan $f$

Simpul X: $g = 1855$, $h = 900$ → $f = 1855 + 900 = 2755$. Simpul Y: $g = 2091$, $h = 600$ → $f = 2091 + 600 = 2691$. Ekspansi Y dulu ($2691 < 2755$).

### Verifikasi Python (tugas)

```python
import heapq

graph = {
    'SF': [('LA', 349), ('Chicago', 1855)],
    'LA': [('Denver', 834)],
    'Denver': [('Chicago', 908)],
    'Chicago': [('Atlanta', 606), ('Boston', 860)],
    'Atlanta': [('Boston', 1000)],
}
h = {'SF': 2700, 'LA': 2500, 'Denver': 1800, 'Chicago': 860, 'Atlanta': 700, 'Boston': 0}

def astar(start, goal):
    open = [(h[start], 0, start, [start])]
    best = {}
    while open:
        f, g, n, path = heapq.heappop(open)
        if n == goal:
            return path, g
        if best.get(n, float('inf')) <= g:
            continue
        best[n] = g
        for m, w in graph.get(n, []):
            heapq.heappush(open, (g + w + h[m], g + w, m, path + [m]))

print(astar('SF', 'Boston'))  # (['SF', 'Chicago', 'Boston'], 2715)
```

Cocokkan output `(2715)` dengan hitungan manual tiap iterasi $f, g, h$. Syarat tugas: graf berbobot sendiri + $h$ admissible, hitung manual tiap iterasi, implementasi Python, kumpul via LMS.

---

## Keterkaitan

- Prasyarat: [[workshop-sistem-cerdas/materi/2 - representasi-pengetahuan|Representasi Pengetahuan]], [[workshop-sistem-cerdas/materi/3 - graf-dan-dijkstra|Graf dan Dijkstra]]
- Lanjutan: [[sistem-cerdas/materi/2 - pencarian-blind-dan-heuristic|Pencarian Blind dan Heuristic]]
