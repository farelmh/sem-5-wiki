---
matkul: aplikasi-sistem-tertanam
topik: Bahasa Pemrograman pada Sistem Tertanam
urutan: 2
tanggal: 2026-10-12
tags: [aplikasi-sistem-tertanam, bahasa-pemrograman, embedded, c, assembly, micropython]
status: selesai
sumber:
  - "aplikasi-sistem-tertanam/resource/kampus/Bahasa_Pemrograman_Pada_Sistem_Tertanam.pdf"
---

# Bahasa Pemrograman pada Sistem Tertanam

## Ringkasan

Embedded system adalah sistem komputer untuk fungsi tertentu yang terintegrasi dengan perangkat elektronik/mekanik. Agar bekerja, diperlukan program yang mengatur hardware. Materi ini membahas alur program (program → bahasa → compiler → machine code → mikrokontroler → sensor/aktuator/display), karakteristik pemrograman embedded, bahasa-bahasa yang dipakai (Assembly, C, C++, MicroPython, Rust, Ada, Java, JS), serta konsep kunci C: register, pointer, dan bit manipulasi.

---

## Konsep Kunci

### 1. Pengantar: Alur Program ke Hardware (gambar: diagram alur slide 2)

- **PROGRAM**: logika/algoritma pengatur sistem (contoh: kontrol sensor, aktuator, display).
- **Bahasa Pemrograman**: aturan penulisan program — C, C++, MicroPython, Assembly, Rust.
- **Compiler**: menerjemahkan source code ke machine code (contoh: GCC, ARM GCC, Arduino Compiler).
- **Machine Code**: kode biner (0 dan 1) yang dijalankan mikrokontroler.
- **Mikrokontroler**: mengeksekusi machine code, mengatur I/O (contoh: Arduino, ESP32, STM32, AVR).
- **Output**: Sensor (baca suhu/cahaya/jarak), Aktuator (motor, relay), Display (tampilan visual).

### 2. Karakteristik Pemrograman Embedded System (gambar: 4 blok slide 3)

- **Berorientasi hardware**: program berhubungan langsung dengan GPIO, ADC, PWM, timer, interrupt, UART, I²C, SPI.
- **Sumber daya terbatas**: RAM, Flash, CPU kecil; konsumsi daya harus diperhatikan.
- **Butuh respons cepat**: contoh pada slide — sensor conveyor deteksi kardus → mikrokontroler proses → motor berhenti (dengan indikator STOP/alarm).
- **Interaksi fisik**: program tidak hanya olah data, tapi mengendalikan perangkat fisik.

### 3. Jenis Bahasa Pemrograman (gambar: daftar + foto PCB slide 4)

1. Assembly, 2. C, 3. C++, 4. MicroPython, 5. Rust, 6. Ada, 7. Java, 8. JavaScript/TypeScript (platform tertentu).
- Prioritas pembelajaran: **C dan C++ sangat penting**; Assembly, MicroPython, Rust sebagai alternatif/pengantar.

### 4. Assembly (gambar: contoh MOV/ADD + tabel register slide 5-6)

- Bahasa sangat dekat instruksi mesin; kontrol hardware sangat tinggi; kode sangat efisien.
- Kelemahan: sangat tergantung arsitektur prosesor; sulit dipelihara.
- Dipakai terbatas untuk: startup code, interrupt tertentu, optimasi, akses hardware level sangat rendah.
- Contoh konseptual (ARM):
  ```
  MOV R0, #01   ; pindah konstanta 01 ke register R0 (R0: 00 → 01)
  ADD R1, R0    ; R1 = R1 + R0 → R1: 00 + 01 = 01
  ```
- Inti: R0–R3 register prosesor; MOV = pindah nilai; ADD = jumlahkan.

### 5. Bahasa C (gambar: kode + flowchart LED + diagram register slide 7-10)

- Kombinasi: kemudahan pemrograman + kontrol hardware + efisiensi.
- Struktur program embedded tipikal: `int main() { while(1) { // program utama } }` — loop selamanya karena mikrokontroler jalan terus.
- Mengapa C banyak dipakai:
  - **Efisien**: dikompilasi ke machine code yang relatif efisien.
  - **Low-level**: manipulasi memory, pointer, register, bit.
  - **Portabel**: mudah pindah platform, walau lapisan hardware perlu sesuaikan.
  - **Ekosistem luas**: compiler, IDE, library, SDK, driver, RTOS.
- Contoh konsep blink LED (Arduino-style):
  ```c
  void setup() {
    pinMode(LED_BUILTIN, OUTPUT);
  }
  void loop() {
    digitalWrite(LED_BUILTIN, HIGH);
    delay(1000);
    digitalWrite(LED_BUILTIN, LOW);
    delay(1000);
  }
  ```
- Alur flowchart pada slide: Start → konfigurasi GPIO OUTPUT → LED ON → tunggu 1000 ms → LED OFF → tunggu 1000 ms → ulangi terus.
- Akses register (konsep, contoh STM32-style): `GPIO->ODR |= (1 << 5);`
  - Diagram 4 tahap pada slide: kode C → register ODR bit5 = 1 → modul GPIO set pin 5 HIGH (3,3 V) → LED menyala lewat resistor ke GND.

### 6. Pointer (gambar: diagram variabel-memori slide 11)

- Pointer menyimpan alamat memori; penting untuk akses memory, buffer, komunikasi data, driver, register hardware.
- Contoh pada slide:
  ```c
  int data = 100;
  int *ptr = &data;
  ```
- Visual: variabel `data` = 100 di alamat `0x2000`; pointer `ptr` = `0x2000`; tabel memori 0x1FFC–0x2004 dengan 0x2000 berisi 100.

### 7. Bit Manipulasi (gambar: infografis operator slide 12)

- Definisi: operasi ubah/atur/baca bit tertentu pada data/register memakai operator bit.
- Ilustrasi register 8-bit: bit7–bit0 = `0 0 0 1 0 0 0 0`, bit4 = 1 (pin aktif).
- Fungsi: set bit (1), clear bit (0), baca status bit, ubah satu bit tanpa ganggu lain, konfigurasi peripheral.
- Operator + contoh pada slide:
  - `AND (&)`: clear bit — `data = data & ~(1 << 3);`
  - `OR (|)`: set bit — `data = data | (1 << 3);`
  - `XOR (^)`: balik bit — `data = data ^ (1 << 3);`
  - `NOT (~)`: balik semua bit — `data = ~data;`
  - `Shift Left (<<)`: geser kiri (≈ ×2) — `data = data << 1;`
  - `Shift Right (>>)`: geser kanan (≈ ÷2) — `data = data >> 1;`

### 8. C++ (gambar: definisi + struktur + eksekusi slide 13)

- C++ = C + OOP (class, object, inheritance, polymorphism); compiled, strongly typed, portabel, STL lengkap, cocok performa tinggi.
- Guna: aplikasi desktop, game, embedded, real-time, OS/browser/perangkat lunak besar.
- Struktur dasar:
  ```cpp
  #include <iostream>
  using namespace std;
  int main() {
      cout << "Hello C++!";
      return 0;
  }
  ```
- Alur eksekusi pada slide: tulis `.cpp` → kompilasi `g++` → file eksekusi `.exe` → jalankan.

### 9. MicroPython (gambar: board + IDE + wiring slide 14)

- MicroPython = implementasi Python 3 untuk mikrokontroler sumber daya terbatas.
- Keunggulan: mudah dipelajari, ringan, cocok mikrokontroler, akses GPIO/sensor/aktuator, komunitas luas.
- Board pendukung (foto pada slide): ESP32, ESP8266 (NodeMCU), Raspberry Pi Pico, Micro:bit, Pyboard.
- Lingkungan: Thonny IDE / WebREPL / editor teks; kode diunggah ke mikrokontroler.
- Contoh LED blink:
  ```python
  from machine import Pin
  import time
  led = Pin(2, Pin.OUT)
  while True:
      led.value(1)
      time.sleep(1)
      led.value(0)
      time.sleep(1)
  ```
- Cara kerja (diagram): tulis kode → unggah ke mikrokontroler → kontrol hardware via GPIO. Wiring pada slide: GPIO 2 ESP32 → LED → resistor → GND.
- Contoh sensor DHT22: VCC → 3,3 V; data → GPIO 4; GND → GND. Kode: `sensor.measure()`, `sensor.temperature()`, `sensor.humidity()`, cetak tiap 2 detik.

### 10. Perbandingan Bahasa (gambar: tabel 8 bahasa slide 15)

| Bahasa | Level | Performa | Kemudahan | Kontrol HW | Contoh guna |
|---|---|---|---|---|---|
| Assembly | Sangat rendah | Sangat tinggi | Rendah ★ | Sangat tinggi | low-level langsung |
| C | Rendah-menengah | Tinggi | Sedang ★★ | Sangat tinggi | firmware, driver |
| C++ | Menengah | Tinggi | Sedang ★★ | Tinggi | firmware kompleks, RTOS |
| MicroPython | Tinggi | Sedang | Tinggi ★★★★ | Tinggi | prototyping/IoT (ESP32, Pico) |
| Rust | Rendah-menengah | Tinggi | Sedang-rendah ★★ | Tinggi | embedded modern aman |
| Ada | Menengah | Tinggi | Sedang ★★ | Tinggi | safety-critical (aviasi, militer) |
| Java | Tinggi | Bervariasi | Tinggi ★★★★ | Terbatas | embedded tertentu (Android) |
| JavaScript | Tinggi | Bervariasi | Tinggi ★★★★ | Tergantung platform | IoT tertentu (Node.js) |

---

## Contoh (dengan perhitungan)

### Hitung nilai register setelah operasi bit (contoh dari slide 12)

Register awal `data = 0b00001000` (bit3 = 1). Set bit4: `data = data | (1 << 4)` → `0b00011000` ($8 + 16 = 24$). Clear bit3: `data = data & ~(1 << 3)` → `0b00010000` ($16$).

---

## Keterkaitan

- Prasyarat: [[aplikasi-sistem-tertanam/materi/1 - pengantar-elektronika-dan-k3|Pengantar Elektronika, Rangkaian, Alat Ukur, dan K3]]
- Lanjutan: pemrograman GPIO/ADC/PWM spesifik board (belum ada halaman)
