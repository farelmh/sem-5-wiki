---
matkul: aplikasi-sistem-tertanam
topik: Jenis Electronics Development Board dan Komunikasi Data
urutan: 3
tanggal: 2026-10-12
tags: [aplikasi-sistem-tertanam, development-board, arduino, esp8266, esp32, komunikasi-data, iot, wireless]
status: selesai
sumber:
  - "aplikasi-sistem-tertanam/resource/kampus/Topik_4_Jenis_Electronics_Development_Board_dan_Komunikasi_Data.pdf"
---

# Jenis Electronics Development Board dan Komunikasi Data

## Ringkasan

Electronics Development Board adalah kumpulan hardware (CPU, memori, peripheral I/O) dalam satu PCB untuk eksperimen sistem elektronika. Materi ini membahas definisi board, keluarga Arduino (khususnya Uno: catu daya, pin analog/digital/serial, SPI, I2C), ESP8266 (NodeMCU), ESP32 (spesifikasi, pin, wired/wireless), konsep komunikasi data (terestrial vs satelit, 5 komponen sistem), komunikasi IoT (sensor → gateway → network server → application server), teknologi wireless (WiFi, Bluetooth/BLE, LoRa, ZigBee, seluler 1G–5G, NFC), dan cara memilih teknologi komunikasi.

---

## Konsep Kunci

### 1. Electronics Development Board

- Definisi: kumpulan CPU + memori + peripheral I/O dalam satu PCB untuk pengembangan/eksperimen elektronika. Sering disebut juga "mikrokontroler", padahal board punya komponen pendukung lebih lengkap (IC downloader, penurun tegangan, catu daya, proteksi).
- Contoh: Arduino, ESP8266, ESP32, STM32, Raspberry Pi, Jetson Nano.
- Spesifikasi yang wajib dicek sebelum pakai: Bits, RAM, Flash, GPIO, konektivitas IoT, konsumsi daya, toolchain/komunitas.

### 2. Arduino: Pengertian dan Macam

- Platform elektronika hardware + software open source, mudah dipakai untuk proyek input/output.
- Varian: UNO, Mega, Nano, Mini/Pro Mini, Fio, dll. (detail: arduino.cc).

### 3. Arduino Uno (gambar: tabel spesifikasi + foto board slide 6)

| Spesifikasi | Nilai |
|---|---|
| Tegangan input | 7–12 V (via DC jack / VIN) |
| Tegangan operasi | 5 V |
| Digital I/O | 14 pin (D0–D13) |
| Analog pin | 6 pin (A0–A5) |
| Flash | 32 KB |
| SRAM | 2 KB |
| EEPROM | 1 KB |
| Clock | 16 MHz |
| Berat | 25 g |
| Dimensi | 68,6 × 53,4 mm |
| Komunikasi | I2C, SPI, Serial |

- **Catu daya** (3 cara): kabel USB (5 V), power jack (7–12 V), pin VIN (7–12 V).
- **Pin analog** (A0–A5): input analog via ADC, mengubah tegangan jadi data digital (contoh: sensor analog, potensiometer).
- **Pin digital** (0–13): input/output HIGH/LOW 5 V (contoh: tombol, sensor digital, LED, aktuator).
- **Pin serial** (RX/TX + USB): tukar data dengan komputer, display, sensor, board lain.
- **Komunikasi serial antar board** (gambar wiring slide 11): TX board-1 → RX board-2, RX board-1 → TX board-2, GND → GND (silang wajib + ground bersama). Guna: debugging via serial monitor, kirim data antar board.
- **SPI** (gambar: teks + pin slide 12): protokol serial master–periferal model bus. Pin: MISO (slave→master), MOSI (master→slave), SCK (clock dari master).
- **Contoh SPI** (gambar wiring + screenshot monitor slide 13): baca sensor BME280 (tekanan, suhu, kelembaban). Wiring: 5V→VIN, GND→GND, D13→SCK, D12→SDO, D11→SDI, D10→CS. Hasil di serial monitor 9600 baud stabil: ~1015,88 mb, ~57,59 %, ~26,86 °C.
- **I2C**: bus antar komponen satu papan. Dua kabel: SDA (data), SCL (clock).

### 4. ESP8266 / NodeMCU (gambar: daftar spesifikasi + pinout slide 16)

- SoC WiFi dari Espressif untuk konektivitas internet ringkas; cocok IoT; daya rendah; mudah diprogram. Varian: NodeMCU, Wemos D1 Mini, Wemos D1 R2.
- Spesifikasi NodeMCU: CPU Tensilica 32-bit RISC Xtensa LX106; operasi 3,3 V; input 7–12 V; DIO 16; ADC 1; UART 2; SPI 1; I2C 1; Flash 4 MB; SRAM 64 KB; clock 80 MHz; antena PCB onboard.
- Pinout (dari gambar): header kiri (TOUT/ADC0, GPIO9/10, MOSI/MISO/SCLK/CS, GND, 3V3, EN, RST, VIN), header kanan (GPIO0/1/2/3/4/5/12/13/14/15/16, TXD/RXD, 3V3, GND), USB + tombol RST/FLASH di bawah.

### 5. ESP32 (gambar: tabel spesifikasi + pinout dev board slide 18-19)

- Penerus ESP8266 dari Espressif; WiFi + Bluetooth dalam chip; sangat cocok IoT.

| Spesifikasi (dari tabel slide) | Nilai |
|---|---|
| Tegangan input | 5 V |
| Tegangan operasi | 5 V |
| ADC | 18 pin |
| DAC | 2 pin |
| Flash | 128 KB |
| SRAM | 320 KB |
| Clock | 240 MHz |
| Berat | 25 g |
| Dimensi | 58,6 × 29 mm |
| Komunikasi | WiFi, Bluetooth, I2C, SPI, Serial |

- **Pin** (gambar pinout ESP32 Dev Board, sumber Last Minute Engineers; teks slide menyebut "ESP32Cam 48 pin" tapi gambar yang tampil adalah Dev Board WROOM-32): multifungsi analog/digital sesuai konfigurasi — GPIO, ADC1/ADC2, Touch, DAC1/DAC2, SPI/VSPI/HSPI (MOSI/MISO/SCK/CS), UART (U0/U2 TXD/RXD), I2C (SDA/SCL), PWM, EN, VIN/3V3/GND.
- **Komunikasi wired**: I2C via SCL (22) + SDA (pin 42 menurut teks slide); SPI via MOSI (23), MISO (19), SCK (18), CS (2); UART via Tx (1) + Rx (3).
- **Komunikasi wireless**: WiFi 802.11 b/g/n up to 150 Mbps; Bluetooth V4 jarak dekat.

### 6. Komunikasi Data: Konsep Dasar

- Definisi: pengiriman + penerimaan data/informasi antar 2+ device (komputer, laptop, HP, printer) dalam satu jaringan, lokal maupun internet; pada dasarnya pengiriman info antar dua titik memakai kode biner via saluran transmisi + switching.
- Definisi lain: transmisi data elektronik via media (koaksial, fiber optik, microwave, dll.); sistemnya disebut jaringan komunikasi data. Dua jenis: infrastruktur terestrial dan satelit.
- **Diagram segitiga** (gambar slide 23): Smartphone ↔ Cloud Server ↔ IoT Device (contoh CCTV), panah dua arah — device kirim data ke cloud, cloud teruskan ke ponsel, ponsel kirim perintah balik.
- **Terestrial**: media kabel + nirkabel (SDL, Frame Relay, VPN MultiService, SKDP); biaya pembangunan tinggi.
- **Satelit**: cakupan luas, jangkau lokasi tanpa infrastruktur; lambat; gangguan radiasi matahari (sun outage, puncak tiap 11 tahun).

### 7. Komponen Sistem Komunikasi Data (5 komponen, gambar ilustrasi tiap slide)

1. **Sumber data**: asal data/informasi (teks, gambar, video) yang diinput lalu dikirim.
2. **Transmitter**: mengolah data jadi file/sinyal yang bisa dikirim (contoh: modem/router — kirim digital bit stream).
3. **Media transmisi**: jalur pembawa sinyal (tunggal/kompleks); contoh: copper, wireless, optical (WiFi, fiber optik).
4. **Penerima (receiver)**: perangkat penerima sinyal via tahap seperti transmitter (contoh: workstation/PC).
5. **Pengirim dan tujuan**: data diproses ulang ke bentuk awal; software aplikasi menjembatani info ke pengguna.

### 8. Komunikasi pada IoT (gambar: rantai sensor → aplikasi slide 31)

- Tanpa konektivitas, data device tidak sampai ke sistem. Pilihan: seluler, satelit, WiFi, Bluetooth, LPWAN, dll. — disesuaikan kebutuhan (LPWAN untuk banyak device kecil area luas seperti pertanian; SD-WAN/managed untuk finansial butuh keamanan tinggi).
- Rantai (dari gambar): Sensor → Gateway → Network Server → Application Server.

### 9. Komunikasi Data Wireless

- Definisi: transfer info jarak jauh tanpa konduktor/kawat. Contoh: WiFi, Bluetooth, LoRa, ZigBee, seluler, NFC.

#### WiFi (gambar: teks + tabel generasi + tabel frekuensi slide 33-35)

- Berbasis sinyal radio, umum 2,4 GHz dan 5 GHz. Router/AP menerjemahkan data internet jadi sinyal radio via antena.
- Standar IEEE 802.11, generasi (dari tabel slide):

| Standar | Kecepatan maks | Frekuensi |
|---|---|---|
| 802.11a & b | 11 Mbps | 2,4 GHz |
| 802.11g | 54 Mbps | 2,4 GHz |
| 802.11n | 450 Mbps | 2,4 GHz |
| 802.11ac | 1300 Mbps | 2,4 & 5 GHz |
| 802.11ax | 3,5 Gbps | 2,4 & 5 GHz |

- Perbandingan frekuensi (dari tabel slide): 2,4 GHz = cakupan luas, tembus objek padat, tapi lambat + interferensi padat, up to 150 Mbps. 5 GHz = cepat sampai Gbps, gangguan sedikit, tapi cakupan kecil + lemah tembus objek.

#### Bluetooth & BLE (gambar: teks + tabel perbandingan slide 36-37)

- Bluetooth: wireless 2,4 GHz ISM, frequency hopping, data + suara real-time, jangkauan ~10 m.
- BLE = Bluetooth Low Energy, bagian Bluetooth 4.0 (bersama klasik + high speed); untuk efisiensi daya.
- Perbandingan (dari tabel slide): Classic = range 100 m, rate 1–3 Mbps, throughput 0,7–2,1 Mbps, bisa suara, daya 1 W, arus <30 mA, latensi 100 ms. BLE = range >100 m, rate 1 Mbps, throughput 0,27 Mbps, tanpa suara, daya 0,01–0,5 W, arus <15 mA, latensi 6 ms, time lag 3 ms, keamanan 128-bit AES. BLE cocok sensor hemat baterai satu area.

#### LoRa (gambar: teks + keunggulan + diagram implementasi + produk + modulasi slide 39-43)

- Sistem wireless IoT jarak jauh (>15 km remote), daya rendah (baterai 5–10 tahun, klaim slide sampai 20 tahun); asal Cycleo Grenoble, akuisisi Semtech 2012; frekuensi 433/868/915/923 MHz ikut regulasi; Indonesia (Kominfo, ikut LoRa Alliance Asia) 923–925 MHz (AS923).
- Keunggulan (dari slide): geolocation gratis, biaya rendah, terstandar, daya 13–15 mA, jarak sampai 100 km (klaim slide), enkripsi AES128 end-to-end, kapasitas jutaan pesan per base station.
- Implementasi (diagram): Devices (sensor) → LoRa protocol → Gateways → TCP/IP → AWS (AWS IoT Core for LoRaWAN + aplikasi cloud).
- Produk: Transceiver SX1272/1273/1276/1277/1278 (137–1020 MHz varian), Concentrator SX1301.
- Modulasi CSS (Chirp Spread Spectrum, adopsi IEEE 802.15.4 LR-WPAN): Bit (satuan biner, rate Rb), Symbol (muat beberapa bit, rate Rs), Chip (pulsa CSS, rate Rc), Chirp (sinyal frekuensi naik/turun terhadap waktu; chirp rate ≈ symbol rate).

#### ZigBee (gambar: foto modul + tabel spesifikasi slide 44-45)

- Nama dari zigzag + bee (lebah terbang berubah arah). Protokol tingkat tinggi mengacu IEEE 802.15.4 untuk WPAN; untuk M2M; bisa atur jaringan + pertukaran data sendiri.
- Spesifikasi (dari tabel slide): frekuensi 2,4 GHz / 868 / 915 MHz (ikut regulasi); kecepatan 250 Kbps; jarak 76 m; daya 100 mW; enkripsi 128-bit AES.
- Foto: modul Telegesis ETRX357 (PCB hijau, shield logam, antena biru Antenova), skala 0,5 inch/10 mm — fisik kecil untuk WPAN/M2M.

#### Seluler 1G–5G (gambar: foto tower + tabel generasi slide 46-47)

- Pengembangan radio + telepon; dipakai komunikasi telepon seluler.
- Perbandingan (dari tabel slide): 1G (1979, 450 MHz, 2,4 kbps, suara); 2G (1991, 1900 MHz, 473 kbps, +SMS/MMS); 3G (2001, 900 MHz, 2–5,76 Mbps, +internet); 4G (2009, 800–2300 MHz, 1 Gbps, +streaming); 5G (2020, 1800 & 2300 MHz, >1 Gbps, +streaming).

#### NFC (gambar: teks + cara kerja slide 48-49)

- Teknologi perangkat berdekatan saling komunikasi tanpa kontak (smartphone, kartu bayar, smartband).
- Cara kerja: satu perangkat transmitter, satu penangkap; harus kompatibel. Transmitter menginduksi arus di perangkat pasif (e-toll/e-money) sehingga kartu tanpa baterai tetap bisa transfer data. Jarak <10 cm.

### 10. Pemilihan Teknologi Komunikasi IoT (gambar: diagram DATA RATE vs RANGE slide 50)

- Faktor: konsumsi daya + jangkauan (teks slide); diagram tampil sumbu DATA RATE (Y) vs RANGE (X).
- Pemetaan (dari gambar): Short-range (Bluetooth, BT 4.0, WiFi, ZigBee, NFC) = range pendek, rate menengah–tinggi. Cellular (2G–5G) = range menengah–jauh, rate tinggi. LPWAN (LoRa, NB-IoT, Sigfox) = range jauh, rate rendah.
- Prinsip: range jauh korbankan data rate (LPWAN); rate tinggi butuh short-range/cellular. Pilih sesuai kebutuhan aplikasi.

---

## Contoh (dengan perhitungan)

### Batas program vs memori Arduino Uno

Flash 32 KB = $32 \times 1024 = 32768$ byte. Jika sketch dikompilasi 20000 byte, sisa $32768 - 20000 = 12768$ byte (~12,5 KB). SRAM 2 KB = 2048 byte untuk variabel/stack/heap saat jalan.

---

## Keterkaitan

- Prasyarat: [[aplikasi-sistem-tertanam/materi/1 - pengantar-elektronika-dan-k3|Pengantar Elektronika, Rangkaian, Alat Ukur, dan K3]] (SPI/I2C/UART), [[aplikasi-sistem-tertanam/materi/2 - bahasa-pemrograman-pada-sistem-tertanam|Bahasa Pemrograman pada Sistem Tertanam]] (GPIO/ADC/PWM)
- Lanjutan: protokol komunikasi + platform IoT (belum ada halaman)
