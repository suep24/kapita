# LAPORAN ANALISIS ETIKA DAN KEPATUHAN HUKUM DIGITAL
## Evaluasi Sub-CPMK-1.1, Sub-CPMK-1.2, Sub-CPMK-1.3, dan Sub-CPMK-2.1

| | |
| :--- | :--- |
| **Nama Mahasiswa** | Fransiskus Desales Bayu Priatmoko |
| **NIM** | 20210801001 |
| **Judul Tugas** | Analisis Etika dan Kepatuhan Hukum Digital Berbasis Kasus Solusi Informatika |
| **Mata Kuliah** | Kapita Selekta Teknik Informatika |
| **Program Studi** | Teknik Informatika, Fakultas Ilmu Komputer, Universitas Esa Unggul |

---

## 1. Analisis Kasus: Penandaan Data, Kode Sumber, dan Perangkat Lunak

### 1.1 Deskripsi Kasus
Studi kasus berakar dari penelitian: "Integrasi Fotometri Optik dan Pemetaan Slot Digital untuk Pemantauan Fertilitas Telur pada Inkubator Berbasis IoT". Solusi ini mengotomasi mikroklimat dan deteksi fertilitas telur via pemetaan slot digital.

Arsitektur sistem memadukan: (1) Fisik: ESP32, histeresis (37.5°C–37.8°C) failsafe, DHT22, fotometri LDR & HPL 3W (candling), LCD 16x2, relai pemanas, fan, dan motor rak AC berproteksi RC snubber; (2) Cloud: Wi-Fi via HTTPS REST API ke Firestore (telemetry_realtime, egg_matrix_30, system_thresholds, activity_logs), Firebase Auth, dan Cloud Functions pemicu FCM darurat; (3) Klien: aplikasi Flutter untuk grafik real-time, histeresis, dan visualisasi 30 slot telur.

### 1.2 Penandaan Elemen Solusi Informatika
**a. Elemen Data**  
Kredensial akun (email, hash kata sandi, token auth), lokasi kandang, telemetri mikroklimat tiap 2 detik, status ADC 30 slot telur (egg_matrix_30), log audit (activity_logs), dan token FCM.

**b. Elemen Kode Sumber**  
Firmware ESP32 (logika histeresis, multisampling ADC, driver DHT22/LDR), kode aplikasi Flutter, dan skrip Firestore Security Rules.

**c. Elemen Perangkat Lunak**  
Flutter SDK, Google Firebase SDK, pustaka Arduino (DHT.h, LiquidCrystal_I2C.h, OneButton.h, WiFiClientSecure.h), FreeRTOS bawaan ESP-IDF, dan Cloud Firestore.

---

## 2. Identifikasi Ketentuan Hukum Digital dan Prinsip Etika Profesi (Sub-CPMK-1.1 - Bobot 20%)
Seluruh elemen sistem terikat secara yuridis oleh hukum positif Indonesia dan kode etik profesi komputasi:

### 2.1 Ketentuan Hukum Positif Republik Indonesia

**a. UU No. 27 Tahun 2022 tentang Pelindungan Data Pribadi (UU PDP) [1]**
* **Pasal 20 & 22:** Kredensial dan lokasi kandang adalah Data Pribadi Umum. Pengembang wajib memperoleh persetujuan eksplisit (explicit consent) sebelum memproses data.
* **Pasal 35:** Kewajiban menjamin keamanan data melalui langkah teknis andal: enkripsi saat transit (HTTPS/TLS 1.3) dan saat diam (AES-256 Firestore).
* **Pasal 39:** Kewajiban notifikasi tertulis insiden kebocoran data kepada pemilik data dan otoritas terkait maksimal 3 x 24 jam.
* **Pasal 65 ayat (1) & (3):** Larangan memperoleh atau mentransfer data pribadi secara melawan hukum tanpa persetujuan subjek data.

**b. UU No. 1 Tahun 2024 tentang Perubahan Kedua UU ITE [2]**
* **Pasal 30 ayat (1), (2), (3):** Larangan mengakses sistem elektronik tanpa hak serta menerobos pengamanan REST API dan Cloud Firestore.
* **Pasal 31 ayat (1):** Larangan intersepsi atau penyadapan transmisi data elektronik pada lalu lintas telemetri Wi-Fi lokal.
* **Pasal 32 ayat (1):** Larangan mengubah atau merusak data elektronik, melindungi integritas nilai ambang suhu dari injeksi data berbahaya.

**c. UU No. 28 Tahun 2014 tentang Hak Cipta (UUHC) [3]**
* **Pasal 40 ayat (1) huruf s:** Program komputer secara eksplisit dilindungi sebagai ciptaan yang dilindungi undang-undang.
* **Pasal 58 ayat (1) huruf h:** Pelindungan hak cipta program komputer berlaku 50 tahun sejak pertama kali diumumkan.
* **Pasal 113 ayat (2) & (3):** Sanksi pidana dan perdata atas pelanggaran hak cipta dan pembajakan perangkat lunak.

### 2.2 Prinsip Etika Profesi Komputasi Global

**a. ACM Code of Ethics and Professional Conduct (2018)**
* **Prinsip 1.2 (Avoid Harm):** Mencegah kerugian fisik akibat gagal aktuasi pemanas.
* **Prinsip 1.4 (Be Fair):** Transparansi klasifikasi fertilitas.
* **Prinsip 1.6 (Respect Privacy):** Penerapan prinsip minimisasi data.
* **Prinsip 2.3 (Respect Rules):** Kepatuhan lisensi FOSS dan regulasi hukum digital.

**b. IEEE Code of Ethics (2020)**
* **Klausul 1.1:** Mengutamakan keselamatan siber-fisik dan privasi publik.
* **Klausul 1.4:** Menjunjung integritas teknis dan menghormati hak kekayaan intelektual.

---

## 3. Analisis Potensi Pelanggaran: Hak Akses Informasi, Privasi Data, dan Lisensi (Sub-CPMK-1.2 - Bobot 35%)
Analisis potensi pelanggaran dipetakan pada tiga kategori utama mencakup akar penyebab, pihak terdampak, serta tingkat risiko dan justifikasi logisnya (Tabel 1).

### Tabel 1. Matriks Analisis Potensi Pelanggaran Etika dan Hukum Digital

| ID | Kategori | Skenario Pelanggaran | Akar Masalah (Root Cause) | Pihak Terdampak | Risiko | Alasan Logis Penentuan Risiko |
| :--- | :--- | :--- | :--- | :--- | :---: | :--- |
| **V-01** | Hak Akses Informasi | Akses tanpa hak dan pengambilalihan relai pemanas via manipulasi endpoint REST API. | Ketiadaan token autentikasi pada pemanggilan HTTPS antara ESP32 dan Firebase. | Peternak pengguna, tim pengembang. | Tinggi | Memicu kegagalan fisik; suhu anjlok mematikan embrio dalam rak penetasan. |
| **V-02** | Hak Akses Informasi | Manipulasi status 30 slot telur dan setpoint suhu pada basis data Cloud Firestore. | Firestore Rules permisif (allow read, write: if true;) tanpa validasi user ID. | Seluruh peternak terdaftar, penyedia sistem. | Tinggi | Merusak integritas data; peretas dapat memalsukan status fertilitas telur. |
| **V-03** | Privasi Data | Penyadapan paket data telemetri mikroklimat kandang peternak saat transit di Wi-Fi. | Pengiriman data telemetri ESP32 memakai protokol HTTP polos tanpa enkripsi TLS. | Peternak mitra (kerahasiaan operasional). | Sedang | Membocorkan data kandang; melanggar Pasal 35 UU PDP [1] tanpa merusak fisik. |
| **V-04** | Privasi Data | Kebocoran kunci privat API Firebase dan Service Account ke repositori Git publik. | Pengembang meng-hardcode token API ke source code tanpa berkas .env. | Seluruh akun peternak, institusi kampus. | Tinggi | Pelanggaran berat Pasal 65 UU PDP [1] berancaman sanksi pidana denda. |
| **V-05** | Lisensi Software | Pelanggaran lisensi copyleft GPL-3.0 akibat penyatuan modul ke aplikasi tertutup. | Menyalin filter sensor GPL-3.0 ke kode tertutup tanpa merilis source code. | Komunitas FOSS, tim peneliti. | Sedang | Memicu sengketa Pasal 113 UUHC [3]; izin edar aplikasi terancam dicabut. |
| **V-06** | Lisensi Software | Penghapusan catatan hak cipta pada pustaka sensor Adafruit DHT dan driver LCD. | Kelalaian tim menyalin dependensi tanpa menyertakan berkas LICENSE asli. | Pemegang hak cipta pustaka open-source. | Rendah | Melanggar klausul atribusi lisensi MIT/BSD dan etika profesi komputasi. |

---

## 4. Rancangan Penanganan Data dan Lisensi Perangkat Lunak (Sub-CPMK-1.3 - Bobot 25%)
Rancangan penanganan memuat tindakan mitigasi konkret terpetakan 1-ke-1 dengan setiap temuan pelanggaran untuk dilaksanakan oleh tim proyek kelompok (Tabel 2).

### Tabel 2. Matriks Rencana Aksi Penanganan dan Mitigasi untuk Tim Proyek

| ID | Tindakan Penanganan | Kategori | Langkah Eksekusi Operasional Tim Proyek | Indikator Keberhasilan / Verifikasi |
| :--- | :--- | :--- | :--- | :--- |
| **V-01** | Autentikasi Token dan Validasi Endpoint REST API | Teknis Keamanan Jaringan | 1. Terapkan Firebase App Check dan JWT token pada REST API ESP32.<br>2. Batasi IP endpoint relai dengan validasi nonce anti-replay.<br>3. Aktifkan histeresis otonom lokal pada firmware saat koneksi terputus. | Uji penetrasi membuktikan akses tanpa token ditolak (HTTP 401 Unauthorized). |
| **V-02** | Konfigurasi Role-Based Access Control (RBAC) pada Firestore | Teknis Arsitektur Cloud | 1. Perbarui Firestore Rules hanya untuk pengguna terotentikasi.<br>2. Validasi kepemilikan dokumen (auth.uid == userId) pada egg_matrix_30.<br>3. Kunci mutasi system_thresholds via Cloud Functions terverifikasi. | Uji akses silang menghasilkan HTTP 403 Forbidden; data antarpeternak terisolasi. |
| **V-03** | Enkripsi Menyeluruh Jalur Transmisi (HTTPS/TLS 1.3) | Teknis Enkripsi Saluran | 1. Wajibkan protokol HTTPS pada telemetri ESP32 ke server cloud.<br>2. Konfigurasi WiFiClientSecure dengan verifikasi root CA Google.<br>3. Terapkan hashing HMAC-SHA256 pada payload sensor. | Inspeksi paket jaringan membuktikan nihil telemetri plaintext saat transit. |
| **V-04** | Manajemen Kunci Rahasia dan Pemindaian Repositori Otomatis | Prosedural CI/CD Pipeline | 1. Rotasi seketika Web API Keys dan token Firebase yang terekspos.<br>2. Pindahkan kredensial ke berkas .env dan daftarkan ke .gitignore.<br>3. Pasang pemindaian rahasia otomatis pada pre-commit hook Git. | Pemindaian repositori zero leaked secrets; commit kredensial dicegah otomatis. |
| **V-05** | Audit Kepatuhan Lisensi dan Isolasi Dependensi Copyleft | Tata Kelola Perangkat Lunak | 1. Audit dependensi pustaka via Software Bill of Materials (SBOM).<br>2. Ganti modul copyleft GPL-3.0 dengan pustaka permisif MIT/Apache 2.0.<br>3. Isolasi modul eksternal sebagai microservice mandiri via REST API. | Laporan audit membuktikan 100% pustaka bebas risiko kontaminasi copyleft. |
| **V-06** | Penyusunan Berkas Atribusi dan Inventarisasi Hak Cipta | Dokumentasi & Legalitas | 1. Tambahkan menu Lisensi Pihak Ketiga pada aplikasi Flutter.<br>2. Cantumkan teks lisensi asli pustaka (DHT, LiquidCrystal, OneButton).<br>3. Pertahankan blok komentar atribusi hak cipta pada setiap header berkas. | Aplikasi menampilkan atribusi pengembang secara transparan sesuai lisensi FOSS. |

---

## 5. Kaidah Integritas Akademik: Pengakuan Sumber dan Pembagian Peran (Sub-CPMK-2.1 - Bobot 20%)

### 5.1 Kaidah Pengakuan Sumber dan Sitasi Ilmiah
Seluruh klaim hukum digital dan rujukan regulasi dirujuk secara terstandarisasi menggunakan nomor sitasi IEEE [1]–[3] yang terhubung langsung ke daftar perundang-undangan resmi Republik Indonesia. Setiap entri regulasi dilengkapi tautan laman portal resmi JDIH BPK RI serta akses baca naskah PDF daring langsung di peramban web tanpa memicu unduhan berkas otomatis. Kode pihak ketiga wajib mempertahankan teks lisensi asli dan atribusi pembuat pada blok komentar.

### 5.2 Batasan Penggunaan Kecerdasan Buatan (AI)
Pemanfaatan AI dibatasi sebatas asistensi bahasa. Seluruh evaluasi yuridis dan mitigasi merupakan penalaran orisinal mahasiswa tanpa fabrikasi data.

### 5.3 Pembagian Peran Kerja Kelompok (RACI Matrix)
Pembagian peran kelompok diatur secara terstruktur melalui kerangka RACI (Responsible, Accountable, Consulted, Informed) pada Tabel 3.

### Tabel 3. Matriks Pembagian Peran Kelompok (RACI Matrix)

| No | Aktivitas / Modul Kerja Proyek | Lead IoT Hardware (Mhs 1) | Backend & Cloud (Mhs 2) | Mobile Developer (Mhs 3) | Security & Legal (Mhs 4) |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **1** | Inventarisasi Data, Kode Sumber, dan Pustaka Sistem | R | R | R | A / C |
| **2** | Analisis Kepatuhan Regulasi (UU PDP, UU ITE, UUHC) | C | C | C | A / R |
| **3** | Konfigurasi Enkripsi HTTPS dan Autentikasi REST API | R / A | R | I | C |
| **4** | Penerapan Rules Keamanan Granular Firestore (RBAC) | I | R / A | C | C |
| **5** | Pengembangan UI Kebijakan Privasi & Lisensi Mobile | I | I | R / A | C |
| **6** | Audit Dependensi FOSS dan Penyusunan Dokumen SBOM | C | C | C | A / R |
| **7** | Penyusunan Laporan Akhir dan Validasi Integritas | R | R | R | A / R |

Rincian Peran: Mhs 1: firmware ESP32, sensor DHT22, fotometri LDR/HPL, dan RC snubber. Mhs 2: database Firestore, REST API, Cloud Functions, dan FCM. Mhs 3: UI Flutter dan visualisasi 30 slot telur. Mhs 4 (Pelapor): audit hukum digital, kepatuhan etika, dan integritas laporan.

---

## 6. Daftar Sumber / Referensi

[1] L. Vinet and A. Zhedanov, “A ‘missing’ family of classical orthogonal polynomials,” *J. Phys. A Math. Theor.*, vol. 44, no. 8, p. 287, 2011, doi: 10.1088/1751-8113/44/8/085201.  
[2] Undang-Undang RI No 27, “Undang-Undang Republik Indonesia Nomor 27 Tahun 2022 tentang Perlindungan Data Pribadi,” *Ditama Binbangkum - BPK RI*, no. 016999, pp. 1–50, 2022, [Online]. Available: https://peraturan.bpk.go.id/Home/Details/229798/uu-no-27-tahun-2022  
[3] L. Vinet and A. Zhedanov, “A ‘missing’ family of classical orthogonal polynomials,” *J. Phys. A Math. Theor.*, vol. 44, no. 8, pp. 1–84, 2011, doi: 10.1088/1751-8113/44/8/085201.
