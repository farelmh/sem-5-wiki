---
matkul: aplikasi-sistem-tertanam
topik: Platform IoT dari Webserver Lokal hingga Public Cloud
urutan: 5
tanggal: 2026-10-12
tags: [aplikasi-sistem-tertanam, iot, platform, esp32, blynk, thingspeak, thingsboard, aws]
status: selesai
sumber:
  - "aplikasi-sistem-tertanam/resource/kampus/Materi_Minggu_ke_7-platform_IoT.pdf"
---

# Platform IoT dari Webserver Lokal hingga Public Cloud

## Ringkasan

Materi minggu 7: spektrum Offline → Edge → Cloud untuk monitoring & control yang skalabel. Tujuan: memahami kapan memilih arsitektur lokal, platform siap pakai, middleware enterprise, atau cloud skala besar. Semakin ke kanan (lokal → public cloud), kebutuhan internet, skalabilitas, integrasi, dan kompleksitas umumnya meningkat. Prinsip pilih: mulai dari kebutuhan sistem, bukan nama platform — latensi → konektivitas → histori → integrasi → skala → biaya.

---

## Konsep Kunci

### 1. Peta Besar: 5 Tingkat Platform IoT (gambar: diagram 5 kartu slide 2)

| # | Tingkat | Label | Isi |
|---|---|---|---|
| 1 | LOKAL / EDGE | Offline First | ESP32 sebagai web server di LAN |
| 2 | RAPID PaaS | Fast Deployment | Dashboard visual + prototyping cepat |
| 3 | MONITORING | Time-Series | Telemetri, histori, analisis data |
| 4 | ENTERPRISE | Rule Engine | Interoperabilitas, aturan, multi-tenant |
| 5 | PUBLIC CLOUD | Massive Scale | Skalabilitas tinggi + ekosistem cloud |

Lima parameter memilih (dari gambar): Latensi (seberapa cepat kontrol merespons?) → Konektivitas (harus jalan tanpa internet?) → Histori (telemetri disimpan/dianalisis?) → Skala (berapa device/pengguna?) → Integrasi (perlu database/API/AI/service cloud?).

### 2. Tingkat 1 — Lokal & Edge-Based (gambar: arsitektur 3 kotak slide 3)

- Fokus: sistem tetap berfungsi tanpa cloud, respons cepat, offline first, latensi rendah, privasi lokal.
- Arsitektur: Laptop/HP buka browser `192.168.x.x` → WiFi/LAN (tidak wajib internet) → ESP32 webserver (HTML/CSS/JS, kontrol GPIO, baca sensor).
- Kelebihan: 100% tanpa internet, sangat responsif, privasi terjaga, tanpa biaya bandwidth cloud.
- Keterbatasan: memori/penyimpanan mikro terbatas, histori skala besar tidak ideal, akses jarak jauh perlu VPN/tunneling.
- Skenario: kontrol sakelar ruangan, alat ukur laboratorium. Inti: edge cocok saat keputusan harus dekat perangkat dan internet tidak boleh jadi single point of failure.

### 3. Tingkat 2 — Visual & Rapid Prototyping (gambar: 2 kartu + diagram alir slide 4-5)

- Cocok mahasiswa/prototipe: dashboard mobile/web dengan waktu pengembangan singkat.
- **Blynk IoT**: dashboard web & mobile, datastream/virtual pins (abstraksi data, bukan GPIO fisik), alur Device ↔ Cloud ↔ App. Kelebihan: rapid deployment, PoC, integrasi hardware cepat. Alur: ESP32 → Blynk Cloud → App.
- **Arduino IoT Cloud**: variabel tersinkronisasi dua arah, dashboard cloud, automatic code generation. Kelebihan: dekat ekosistem Arduino, prototipe/smart home. Catatan: ketergantungan library Arduino.
- Arsitektur komunikasi (diagram, sumber CircuitState): IoT Endpoints (sensor/aktuator/interface/mesh) ↔ IoT & Edge Nodes ↔ Gateways & Networks (seluler/AP/router) ↔ Internet ↔ IoT Cloud. Contoh dashboard: widget cuaca live (rain, pressure 998,54 hPa, suhu 35,22 °C, humidity 24 %, UV, wind, lightning, soil moisture) dengan tab 7D/1D/1H/LIVE.

### 4. Tingkat 3 — Monitoring & Analisis Data (gambar: 2 kartu slide 6)

- Fokus bergeser dari kontrol ke telemetri → time-series → insight (contoh data: suhu, kelembapan, energi, curah hujan, getaran).
- **ThingSpeak (MathWorks)**: model Channel → Field → Timestamp, cocok telemetri/time-series. Kekuatan: analisis MATLAB, filtering/statistik. Pertimbangan: rate-limit memengaruhi frekuensi kirim.
- **Thinger.io**: model resource-centric + Data Buckets untuk time-series. Kekuatan: efisien data, histori bisa diekspor, opsi self-hosting. Pertimbangan: dashboard lebih sederhana.

### 5. Tingkat 4 — Middleware Enterprise (gambar: 2 panel + hierarki/rule chain slide 7)

- Fungsi: lapisan integrasi, pemrosesan aturan, manajemen perangkat, kontrol akses.
- **Antares (Telkom Indonesia)**: pendekatan oneM2M; hierarki Application → Device → Container → Content Instance; API/middleware oriented. Kekuatan: integrasi IoT/LoRaWAN, webhook + server kustom. Cocok interoperabilitas + pipa data terstandar.
- **ThingsBoard (open-source enterprise)**: rule engine visual; device & asset management; RBAC. Kekuatan: fleksibel transformasi telemetri, hierarki aset/tenant, self-hosting. Pertimbangan: perlu paham server/database/deployment.
- Rule chain: data masuk (IN) → disaring (FILTER) → diolah (TRANSFORM) → ditindaklanjuti (ACT).

### 6. Tingkat 5 — Public Cloud (gambar: diagram 4 kotak slide 8)

- Untuk banyak perangkat, integrasi service cloud, keamanan, arsitektur sangat skalabel.
- **AWS IoT Core**: device gateway & message broker; MQTT / MQTT over WSS / HTTPS; TLS + autentikasi per perangkat. Kelebihan: ekosistem AWS, skala besar, teruskan data ke service lain. Tantangan: IAM/policy, sertifikat, provisioning, biaya pay-as-you-go.
- Alur: DEVICE (telemetri MQTT) → AWS IoT CORE (verifikasi TLS) → RULES (rutekan pesan) → DATA/COMPUTE (DynamoDB NoSQL, S3 data lake, Lambda serverless, CloudWatch monitoring).
- Prinsip desain: secure by default, least-privilege access, device identity, monitoring & cost control.

### 7. Matriks Komparasi (gambar: tabel 8 baris slide 9)

| Level | Platform | Fokus | Protokol | Deployment |
|---|---|---|---|---|
| Lokal | Webserver lokal | Kontrol offline, latensi rendah | HTTP / WebSockets | Standalone (mikro) |
| Rapid PaaS | Blynk IoT | UI cepat, mobile dashboard | Virtual Pins / WebSockets | Cloud SaaS |
| Rapid PaaS | Arduino IoT Cloud | Auto-code, smart home | MQTT / WebSockets | Cloud SaaS |
| Analisis | ThingSpeak | Komputasi + analisis MATLAB | HTTP REST / MQTT | Cloud SaaS |
| Analisis/self-host | Thinger.io | Resource + Data Buckets | Binary / HTTP | Cloud & self-hosted |
| Enterprise | Antares | oneM2M & LoRaWAN | HTTP / MQTT / CoAP | Cloud PaaS |
| Enterprise | ThingsBoard | Rule engine & asset | MQTT / HTTP / CoAP | Cloud & self-hosted |
| Public cloud | AWS IoT Core | Skala & serverless | MQTT / TLS / HTTPS | Managed public cloud |

Catatan slide: fitur/kuota/protokol/harga/deployment dapat berubah — selalu cek dokumentasi resmi.

### 8. Cara Memilih (gambar: alur 5 langkah slide 10)

1. Harus jalan tanpa internet? YA → Lokal/Edge.
2. Prioritas prototipe cepat + dashboard siap pakai? YA → Blynk / Arduino IoT Cloud.
3. Histori + analisis telemetri kebutuhan utama? YA → ThingSpeak / Thinger.io.
4. Perlu rule engine, hierarki aset, RBAC, self-hosting? YA → ThingsBoard / Antares.
5. Perangkat + integrasi cloud sangat besar? YA → AWS IoT Core / public cloud.

### 9. Contoh Kasus: Monitoring Suhu ESP32 (gambar: tabel 5 skenario slide 11)

| Skenario | Arsitektur | Kebutuhan | Pilihan |
|---|---|---|---|
| Lab kecil | ESP32 + sensor → webserver lokal | Tanpa internet, respons cepat | Lokal |
| Demo kelas/PoC | ESP32 → Blynk → HP | Dashboard cepat, kontrol HP | Blynk |
| Riset telemetri | ESP32 → ThingSpeak | Grafik histori + analisis | ThingSpeak |
| Gedung/aset banyak | Devices → ThingsBoard | Rule engine + asset mgmt | ThingsBoard |
| Produk skala besar | Devices → AWS IoT Core → services | Skala, security, integration | AWS |

Platform bukan sekadar tempat menampilkan data — pilihan menentukan lokasi pemrosesan, latensi, penyimpanan histori, keamanan, dan biaya operasional.

---

## Contoh (dengan perhitungan)

### Estimasi throughput telemetri vs rate-limit

Jika sensor kirim 1 pesan/menit (60/jam, 1440/hari) dan platform batasi 1 pesan/15 detik (ThingSpeak pola free-tier), margin aman $15\times$ — cocok. Jika butuh 1 pesan/detik (86400/hari), butuh tier berbayar atau self-host (Thinger.io/ThingsBoard) — pilih sesuai kebutuhan histori vs biaya.

---

## Keterkaitan

- Prasyarat: [[aplikasi-sistem-tertanam/materi/4 - protokol-komunikasi-dan-cara-kerjanya|Protokol Komunikasi dan Cara Kerjanya]] (MQTT, HTTP), [[aplikasi-sistem-tertanam/materi/3 - jenis-development-board-dan-komunikasi-data|Jenis Development Board dan Komunikasi Data]] (ESP32, komunikasi IoT)
- Lanjutan: praktikum ESP32 → WiFi → MQTT/HTTP → platform IoT → dashboard (tugas slide 13)

## Referensi (dari slide 13)

- Dokumentasi resmi: Blynk, Arduino / Arduino IoT Cloud, MathWorks ThingSpeak, Thinger.io, Antares / Telkom Indonesia, ThingsBoard, AWS IoT Core Developer Guide.
- Pesan: pilih arsitektur berdasar kebutuhan; pisahkan kontrol real-time vs analisis; pahami tradeoff kemudahan ↔ fleksibilitas ↔ skala ↔ biaya; verifikasi dokumentasi + paket layanan terbaru.
