---
matkul: aplikasi-sistem-tertanam
topik: Protokol Komunikasi dan Cara Kerjanya
urutan: 4
tanggal: 2026-10-12
tags: [aplikasi-sistem-tertanam, protokol, uart, i2c, spi, can, mqtt]
status: selesai
sumber:
  - "aplikasi-sistem-tertanam/resource/kampus/Protokol_Komunikasi_dan_Cara_Kerjanya.pdf"
---

# Protokol Komunikasi dan Cara Kerjanya

## Ringkasan

Pertemuan 6 (Elly Antika, TIF JTI Polije): UART, I2C, SPI, CAN, dan MQTT — dari sinyal bit hingga contoh kode. Target: menjelaskan konsep + klasifikasi protokol, cara kerja keempat protokol wired hingga tingkat bit, menulis program komunikasi sensor/modul, dan memilih protokol yang tepat termasuk MQTT untuk IoT.

---

## Konsep Kunci

### 1. Apa Itu Protokol Komunikasi

- Sekumpulan aturan yang disepakati 2+ perangkat agar data terkirim dan terbaca benar.
- Empat pilar: format frame (urutan bit awal–data–akhir), sinkronisasi (kapan tiap bit dibaca), pengalamatan (siapa bicara/mendengar), deteksi error (paritas, ACK, CRC).
- Analogi: dua orang harus sepakat bahasa, giliran bicara, dan cara memastikan pesan dipahami.

### 2. Tiga Cara Mengelompokkan Protokol

- **Jumlah jalur data**: Serial (1 bit per jalur, hemat pin — hampir semua protokol modern) vs Paralel (banyak bit bersamaan, cepat jarak pendek, boros pin).
- **Sumber waktu**: Sinkron (clock bersama — I2C, SPI) vs Asinkron (tanpa clock, sepakat baud rate — UART, CAN).
- **Topologi**: Point-to-point (2 perangkat — UART) vs Bus (banyak perangkat satu jalur — I2C, SPI, CAN).

### 3. UART (gambar: diagram timing frame huruf 'A' slide 5)

- 'A' = 0x41 = 0100 0001, dikirim LSB duluan, garis diam di level 1.
- Urutan frame: Idle(1) → Start(0) → D0–D7 = 1,0,0,0,0,0,1,0 → Stop(1) → Idle(1).
- Tanpa clock: kedua sisi sepakat baud rate; pada 9600 bps, 1 bit ≈ 104 µs.
- Kabel disilang: TX→RX, RX→TX, GND bersama.
- Format 8N1: 8 bit data, tanpa paritas, 1 stop bit = 10 bit per byte.
- Contoh kode Arduino (kendali LED via Serial Monitor, Uno UART hardware pin 0 RX / 1 TX via USB):
  ```c
  void setup() {
    pinMode(13, OUTPUT);
    Serial.begin(9600);  // 9600 bps, format 8N1
    Serial.println("Siap. Kirim 1 atau 0");
  }
  void loop() {
    if (Serial.available() > 0) {
      char c = Serial.read();
      if (c == '1') digitalWrite(13, HIGH);
      if (c == '0') digitalWrite(13, LOW);
      Serial.print("Diterima: ");
      Serial.println(c);  // echo
    }
  }
  ```
- Level register: UBRR0 = 16 MHz / (16 × 9600) − 1 = 103.

### 4. I2C (gambar: topologi bus + urutan transaksi slide 7-8)

- Dua kabel banyak perangkat: SDA (data) + SCL (clock dari master). Contoh bus: mikrokontroler master + sensor suhu 0x48 + RTC 0x68 + EEPROM 0x50.
- Sinkron, beralamat (tiap slave alamat 7-bit unik), open-drain (perangkat hanya tarik ke 0; pull-up mis. 4,7 kΩ kembalikan ke 1).
- Transaksi tulis (contoh: pilih register suhu 0x00 pada LM75 alamat 0x48): START → alamat 7-bit (1001000) → R/W(0) → ACK(slave) → data (00000000) → ACK → STOP.
- START = SDA turun saat SCL tinggi; STOP = SDA naik saat SCL tinggi. SDA hanya boleh berubah saat SCL rendah, dibaca saat SCL tinggi.
- Baca data: repeated START, alamat + R/W = 1, slave kirim byte, master balas ACK/NACK.
- Contoh kode Arduino (baca suhu LM75; Wire tangani START/alamat/ACK/STOP otomatis; Uno SDA A4, SCL A5):
  ```c
  #include <Wire.h>
  const uint8_t LM75_ADDR = 0x48;
  void setup() {
    Serial.begin(9600);
    Wire.begin();  // master
  }
  void loop() {
    Wire.beginTransmission(LM75_ADDR);
    Wire.write(0x00);  // register suhu
    Wire.endTransmission();
    Wire.requestFrom(LM75_ADDR, (uint8_t)2);
    uint8_t msb = Wire.read();
    uint8_t lsb = Wire.read();
    int16_t raw = (msb << 8) | lsb;
    float suhu = (raw >> 7) * 0.5;  // resolusi 0,5 °C
    Serial.println(suhu);
    delay(1000);
  }
  ```

### 5. SPI (gambar: diagram 4 jalur + register geser slide 10)

- Empat jalur, cepat, full-duplex: SCLK (clock dari master), MOSI/COPI (master→slave), MISO/CIPO (slave→master), CS/SS aktif LOW (satu pin per slave, tanpa alamat).
- Tiap clock: 1 bit keluar di MOSI dan 1 bit masuk di MISO bersamaan (pertukaran register geser).
- Mode 0–3: kombinasi CPOL + CPHA; harus sesuai datasheet slave.
- Contoh kode Arduino (baca ADC MCP3008 — 10-bit 8 kanal, 1 pembacaan = 3 byte; Uno SCK 13, MISO 12, MOSI 11, CS 10):
  ```c
  #include <SPI.h>
  const uint8_t PIN_CS = 10;
  SPISettings cfg(1000000, MSBFIRST, SPI_MODE0);
  int bacaMCP3008(uint8_t ch) {
    SPI.beginTransaction(cfg);
    digitalWrite(PIN_CS, LOW);
    SPI.transfer(0x01);                       // start bit
    uint8_t hi = SPI.transfer(0x80 | (ch << 4));  // mode single
    uint8_t lo = SPI.transfer(0x00);
    digitalWrite(PIN_CS, HIGH);
    SPI.endTransaction();
    return ((hi & 0x03) << 8) | lo;  // hasil 10-bit
  }
  void setup() {
    Serial.begin(9600);
    pinMode(PIN_CS, OUTPUT);
    digitalWrite(PIN_CS, HIGH);
    SPI.begin();
  }
  void loop() {
    Serial.println(bacaMCP3008(0));  // kanal 0
    delay(500);
  }
  ```

### 6. CAN (gambar: frame standar + tabel arbitrase slide 12-13)

- Bus tangguh kendaraan/industri: sinyal diferensial (CAN_H + CAN_L, baca selisihnya → tahan derau); terminasi 120 Ω tiap ujung bus; berbasis pesan (ID, bukan alamat tujuan).
- Frame data standar (CAN klasik): SOF 1 + ID 11 + RTR 1 + Kontrol 6 + Data 0–64 + CRC 16 + ACK 2 + EOF 7 bit.
- Pakai: ECU mobil (mesin, ABS, airbag), mesin industri, robotika. Hingga 1 Mbit/s; CAN FD lebih besar + cepat.
- Arbitrase: ID terkecil menang. Bit 0 = dominan, 1 = resesif. Contoh: A 0x100 vs B 0x0F0 — pada bit 8, A kirim 1 tapi baca 0 di bus → A kalah dan berhenti; B lanjut tanpa data rusak. ID kecil = prioritas tinggi (rem, airbag).
- Contoh Arduino + MCP2515:
  ```c
  #include <mcp_can.h>
  MCP_CAN CAN(10);  // CS di pin 10
  void setup() {
    CAN.begin(MCP_ANY, CAN_500KBPS, MCP_8MHZ);
    CAN.setMode(MCP_NORMAL);
  }
  void loop() {
    byte data[2] = {0x01, 0xF4};  // mis. 500 rpm
    CAN.sendMsgBuf(0x0F0, 0, 2, data);
    delay(100);
  }
  ```

### 7. MQTT (gambar: diagram publish/subscribe + QoS slide 14)

- Publish/subscribe via broker (mis. Mosquitto port 1883). Publisher tidak kirim langsung ke subscriber.
- Alur (dari gambar): node sensor publish `lab/ruang1/suhu = 27,5` → broker cocokkan topik → dashboard web (`subscribe lab/+/suhu`, `+` = satu level) dan aplikasi HP (`subscribe lab/ruang1/#`, `#` = semua level bawah) terima pesan.
- QoS: 0 (paling banyak sekali — cepat, bisa hilang), 1 (minimal sekali — pasti sampai, bisa duplikat), 2 (tepat sekali — paling andal, paling berat).
- Contoh MicroPython ESP32 (kirim suhu tiap 10 detik; ganti SSID/sandi/alamat broker):
  ```python
  import network, time
  from umqtt.simple import MQTTClient
  wlan = network.WLAN(network.STA_IF)
  wlan.active(True)
  wlan.connect("[SSID]", "[PASSWORD]")
  while not wlan.isconnected():
      time.sleep(0.5)
  c = MQTTClient("node-suhu-01", "[ALAMAT_BROKER]")
  c.connect()
  while True:
      suhu = baca_suhu()  # fungsi sensor Anda
      c.publish(b"lab/ruang1/suhu", str(suhu))
      time.sleep(10)
  ```

### 8. Perbandingan UART vs I2C vs SPI vs CAN (tabel slide 16)

| Aspek | UART | I2C | SPI | CAN |
|---|---|---|---|---|
| Jalur sinyal | 2 (TX, RX) | 2 (SDA, SCL) | 4 (+1 CS per slave) | 2 (CAN_H, CAN_L) |
| Clock | Asinkron | Sinkron | Sinkron | Asinkron |
| Arah data | Full-duplex | Half-duplex | Full-duplex | Half-duplex |
| Perangkat | 2 (point-to-point) | Banyak via alamat | Banyak via pin CS | Banyak, multi-master |
| Kecepatan umum | 9600–115200 bps | 100 / 400 kHz | Hingga puluhan MHz | Hingga 1 Mbit/s |
| Cek error | Paritas (opsional) | ACK per byte | Tidak ada | CRC, ACK |

### 9. Ringkasan Praktis: Kebutuhan Menentukan Protokol

- Debug ke PC / GPS / Bluetooth / GSM → UART.
- Banyak sensor lambat satu board → I2C.
- Data cepat (layar, kartu SD, ADC) → SPI.
- Jaringan antar-ECU tahan gangguan → CAN.
- Kirim data ke internet/dashboard → MQTT.

---

## Contoh (dengan perhitungan)

### Waktu kirim 1 baris 32 byte via UART 9600 bps 8N1 (tugas slide 18)

Tiap byte = 10 bit (8 data + start + stop). Total $32 \times 10 = 320$ bit. Waktu $= 320 / 9600 \approx 0{,}0333\text{ s} \approx 33{,}3\text{ ms}$.

### Tugas studi kasus: stasiun cuaca mini (slide 18)

Mikrokontroler baca sensor suhu (I2C), simpan log ke kartu SD (SPI), debug ke PC (UART), kirim dashboard (MQTT). Tugas: 1) gambar blok + protokol tiap koneksi, 2) hitung waktu kirim (contoh di atas), 3) baca sensor I2C + kirim via MQTT, 4) rekam frame UART/I2C dengan logic analyzer (tandai START, alamat, ACK).

---

## Keterkaitan

- Prasyarat: [[aplikasi-sistem-tertanam/materi/3 - jenis-development-board-dan-komunikasi-data|Jenis Development Board dan Komunikasi Data]] (SPI/I2C/UART board-level, LoRa/WiFi/BT), [[aplikasi-sistem-tertanam/materi/2 - bahasa-pemrograman-pada-sistem-tertanam|Bahasa Pemrograman pada Sistem Tertanam]] (C, register, GPIO)
- Lanjutan: platform IoT (belum ada halaman)

## Referensi (dari slide 19)

- NXP, UM10204: I2C-bus Specification and User Manual
- Bosch, CAN Specification 2.0; ISO 11898
- OASIS, MQTT Version 3.1.1 dan 5.0
- Arduino Reference: Serial, Wire, SPI
- Datasheet LM75, MCP3008, dan MCP2515
