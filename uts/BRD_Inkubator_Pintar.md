# Business Requirements Document (BRD)
## Sistem Pintar Monitoring Kondisi Inkubasi dan Deteksi Fertilitas Mesin Tetas Telur Berbasis IoT dan Aplikasi Mobile

**Fakultas Ilmu Komputer (FASILKOM) – Universitas Esa Unggul**  
**Penyusun:** Business Analyst Senior FASILKOM Universitas Esa Unggul  
**Proyek:** Tugas Akhir / Skripsi S1 Teknik Informatika  
**Versi Dokumen:** 1.0 (Final Baseline)  
**Tanggal:** 28 Oktober 2026  

---

## 1. Executive Summary

### 1.1 Latar Bisnis
Sektor peternakan unggas, khususnya pembibitan ayam lokal dan ayam ras skala mikro, kecil, dan menengah (UMKM), memegang peranan krusial dalam rantai pasok ketahanan pangan. Salah satu tahapan paling kritis yang menentukan produktivitas peternak adalah efisiensi proses penetasan telur (*egg hatching*). Keberhasilan penetasan sangat bergantung pada stabilitas mikroklimat ruang inkubator (suhu dan kelembapan) serta identifikasi dini terhadap fertilitas embrio telur.

### 1.2 Permasalahan Nyata (*Pain Points*)
Berdasarkan observasi lapangan dan studi literatur yang relevan, peternak konvensional menghadapi kendala-kendala utama berikut:
1. **Fluktuasi Mikroklimat Non-Presisi:** Penggunaan termostat mekanik/bimetal analog sering kali lambat merespons perubahan suhu lingkungan, menyebabkan fluktuasi ekstrem di luar batas toleransi biologis ($37.0^\circ\text{C} - 39.0^\circ\text{C}$, Aziz Jabbar dkk., 2024), yang berakibat fatal pada kematian embrio di dalam cangkang (*dead-in-shell*).
2. **Peneropongan (*Candling*) Manual & Subjektif:** Proses seleksi telur fertil dan infertil dilakukan secara manual menggunakan senter satu per satu. Metode ini rentan terhadap kesalahan interpretasi visual subjektif (tingkat galat manual mencapai $\approx 14.0\%$, Anggraini dkk., 2024), menyita waktu, dan menurunkan suhu inkubator secara drastis saat tutup boks dibuka terlalu lama.
3. **Risiko Kontaminasi Telur Busuk (Exploding Eggs):** Ketiadaan pencatatan slot telur menyebabkan telur infertil atau embrio mati terlambat disingkirkan. Telur yang membusuk pada suhu inkubasi berisiko meledak, melepaskan gas berbahaya, dan menularkan bakteri ke telur sehat lainnya dalam satu rak.
4. **Ketiadaan Pengawasan Jarak Jauh & Peringatan Dini:** Peternak tidak dapat memantau kondisi inkubator secara *real-time* saat berada di luar lokasi, sehingga kegagalan perangkat (lampu pemanas putus, kipas macet, atau listrik padam) baru disadari setelah seluruh embrio mati.

### 1.3 Tujuan Sistem (*System Purpose*)
Menghadirkan sistem terpadu **Smart Egg Incubator** berbasis *Internet of Things (IoT)* dan *Mobile Application (Flutter)* yang:
- Mengotomatisasi pengendalian mikroklimat menggunakan termostat histeresis presisi pada mikrokontroler ESP32 dengan sensor DHT22.
- Mengintegrasikan mekanisme deteksi fertilitas telur berbasis fotometri optoelektronik (High-Power LED dan sensor LDR) secara semi-otomatis pada tahapan kritis H-7 dan H-14.
- Memetakan 30 slot rak telur secara digital di Cloud Firestore dengan visualisasi status (*Fertil*, *Afkir*, *Kosong*).
- Menyediakan saluran pemantauan jarak jauh dan notifikasi peringatan darurat seketika melalui *Firebase Cloud Messaging* (FCM).

---

## 2. Business Objectives & Success Metrics (SMART)

Tujuan bisnis dirumuskan secara terukur menggunakan kerangka SMART (*Specific, Measurable, Achievable, Relevant, Time-bound*):

| ID | Sasaran Bisnis (Business Objective) | Indikator Keberhasilan (Success Metric) | Target Terukur | Rujukan Literatur / Basis Data |
| :--- | :--- | :--- | :--- | :--- |
| **BO-01** | Menjaga kestabilan mikroklimat ruang inkubasi pada rentang suhu optimal. | Deviasi rata-rata suhu (*Mean Absolute Error* / MAE) dan tingkat *error* pembacaan sensor. | Suhu terjaga pada $37.5^\circ\text{C} - 37.8^\circ\text{C}$; margin error sensor DHT22 $\le 0.55\%$. | Aziz Jabbar dkk. (2024); Rahayu & Styawati (2025). |
| **BO-02** | Meningkatkan akurasi klasifikasi fertilitas telur (Fertil vs. Afkir). | Nilai akurasi klasifikasi berbasis nilai intensitas cahaya tembus LDR. | Akurasi klasifikasi $\ge 85.0\%$. | Anggraini dkk. (2024) [baseline akurasi 86.0%]; Sopiana dkk. (2026). |
| **BO-03** | Mengurangi risiko pembusukan dan kontaminasi silang di dalam inkubator. | Persentase telur afkir (infertil/mati) yang berhasil diidentifikasi dan dievakuasi pada fase H-7 & H-14. | $100\%$ telur afkir terdeteksi dan tercatat sebelum memasuki fase *lockdown* (H-18). | Standar Biologis Penetasan Unggas; Subandi & Hidayat (2026). |
| **BO-04** | Mempercepat deteksi anomali kritis melalui pengiriman notifikasi ke smartphone. | Jeda waktu pengantaran (*latency*) push notifikasi darurat FCM dari saat anomali terjadi hingga diterima di HP. | Latensi notifikasi $\le 3.0\text{ detik}$ pada kondisi jaringan internet stabil. | May Aryanto dkk. (2026); FCM Quality of Service Standard. |
| **BO-05** | Memastikan penerimaan dan kemudahan penggunaan aplikasi oleh peternak. | Skor kepuasan antarmuka pengguna menggunakan metode *System Usability Scale* (SUS). | Skor SUS $\ge 75.0$ (kategori *Good / Acceptable*). | Standar Evaluasi Usability ISO 9241-11; Darmawan dkk. (2026). |

---

## 3. Scope of Project

### 3.1 In-Scope (Eksplisit Dikerjakan)
1. **Perangkat Keras & Firmware:**
   - Unit mikrokontroler ESP32 Dev Module sebagai unit pemrosesan lokal.
   - Sensor suhu & kelembapan digital DHT22 untuk pemantauan mikroklimat.
   - Sistem candling optik menggunakan High-Power LED (HPL 3W–5W) dan modul sensor cahaya LDR (LM393) untuk mengukur persentase tembus cahaya cangkang telur.
   - Modul Relay pengendali 3 beban: Lampu Pemanas, Kipas Sirkulasi/Exhaust, dan Motor Pemutar Rak (TYD49 AC), dilengkapi rangkaian proteksi peredam transien tegangan induktif (*RC snubber circuit*) paralel pada kontak relai beban AC guna mengeliminasi busur api listrik (*electrical arcing*) dan interferensi elektromagnetik (*EMI*).
   - Pengaturan termostat histeresis mandiri (*failsafe offline*) dan tombol fisik antarmuka LCD 16x2.
2. **Kapasitas & Rak Telur:**
   - Rak telur fisik dan pemetaan logis kapasitas tetap **30 butir telur** (grid 5x6, Slot 01 s/d Slot 30).
3. **Cloud & Layanan Backend:**
   - Basis data Google Cloud Firestore untuk penyimpanan data *real-time*: telemetri mikroklimat, profil siklus (*batch*), status 30 slot telur, dan log riwayat aktivitas.
   - Layanan Firebase Cloud Messaging (FCM) untuk pengiriman *push notification* darurat dan pengingat jadwal tahapan (H-7, H-14, H-21).
4. **Aplikasi Mobile (Flutter):**
   - Halaman **Dasbor**: Gauge suhu & kelembapan, status perangkat IoT, penghitung hari inkubasi, dan visualisasi 30 slot telur.
   - Halaman **Slot Telur**: Rekapitulasi (Total, Fertil, Afkir, Kosong), pemilihan slot spesifik, dan pemicu tombol *"Periksa Telur"*.
   - Halaman **Hasil Peneropongan**: Tampilan persentase intensitas cahaya, penetapan status (Fertil/Afkir), dan tombol simpan ke database.
   - Halaman **Kontrol Perangkat**: Saklar mode Otomatis vs. Manual (*remote override* lampu, kipas, motor).
   - Halaman **Riwayat**: Catatan kronologis telemetri dan log aktivitas peneropongan.
   - Halaman **Profil Pengguna**: Manajemen identitas peternak dan toggle izin notifikasi *drift* inkubator.

### 3.2 Out-of-Scope (Eksplisit Ditiadakan)
1. Penggunaan sensor timbangan berat (*Load Cell* + HX711) ditiadakan untuk menjaga efisiensi mekanik dan keandalan perputaran rak.
2. Pengolahan citra kecerdasan buatan (*Computer Vision / Deep Learning / CNN*) via kamera ditiadakan; klasifikasi murni berbasis pengukuran fotometri sensor optik LDR.
3. Otomasi penambahan air aktif menggunakan *ultrasonic mist maker / humidifier* (kelembapan diatur secara pasif menggunakan wadah air dan sirkulasi kipas).
4. Sistem daya cadangan terintegrasi (*Automatic Transfer Switch / UPS internal*) saat terjadi pemadaman listrik PLN total.
5. Manajemen *multi-tenant* untuk industri peternakan skala raksasa berkapasitas ribuan butir (fokus pada 1 unit inkubator kapasitas 30 butir untuk peternak UMKM).

---

## 4. Stakeholder Analysis

| Stakeholder | Peran / Jabatan | Kepentingan Utama (Interests) | Pengaruh (Influence) | Tingkat Keterlibatan |
| :--- | :--- | :--- | :--- | :--- |
| **Peternak Unggas (End-User)** | Pengguna Utama / Operator Mesin | Memperoleh kemudahan pemantauan mikroklimat tanpa harus membuka boks, mendapatkan peringatan dini saat suhu abnormal, serta memisahkan telur infertil secara akurat. | **High** | Menguji aplikasi secara langsung dan mengisi kuesioner evaluasi kegunaan (*usability*). |
| **Mahasiswa Peneliti (Author)** | Pengembang Sistem & Analis | Merancang hardware, menulis firmware ESP32, mengembangkan aplikasi Flutter, mengonfigurasi Firebase, serta menyusun laporan skripsi. | **High** | Mengambil seluruh keputusan teknis dan implementasi arsitektur. |
| **Dosen Pembimbing Skripsi** | Penasihat Akademik & Teknis | Memastikan metodologi penelitian valid, arsitektur sistem kokoh, data pengujian dapat dipertanggungjawabkan, serta keterikatan sumber pustaka sahih. | **High** | Memvalidasi kelayakan naskah skripsi dan prototipe alat sebelum sidang. |
| **Dosen Penguji Sidang** | Evaluator Kelayakan Karya | Menguji orisinalitas, validitas pengujian alat, kesesuaian diagram perancangan (UML/SRS), serta ketepatan metodologi ilmiah. | **High** | Menentukan kelulusan karya pada Seminar Proposal dan Sidang Yudisium. |
| **Fakultas Ilmu Komputer (UEU)** | Institusi Akademik | Memastikan karya tugas akhir berkontribusi pada pengembangan iptek terapan, memiliki standar publikasi ilmiah, dan mengangkat nama universitas. | **Medium** | Menyediakan standar pedoman skripsi dan sarana pengujian laboratorium. |

> **Catatan Lokasi & Operasional Penelitian:** Pengujian empiris operasional mesin penetas dan pengambilan data biologis siklus penetasan 21 hari dilaksanakan secara mandiri di peternakan ayam milik pribadi peneliti, didukung evaluasi kepuasan antarmuka aplikasi seluler oleh responden peternak unggas sekitar.

---

## 5. Functional Requirements (FR)

Kebutuhan fungsional diklasifikasikan menggunakan prioritas **MoSCoW** (*Must have, Should have, Could have*):

| ID | Modul / Kategori | Deskripsi Kebutuhan Fungsional | Prioritas | Aktor / Pemicu |
| :--- | :--- | :--- | :--- | :--- |
| **FR-01** | Telemetri Sensor | Sistem ESP32 harus membaca data suhu (°C) dan kelembapan relatif (% RH) dari sensor DHT22 setiap 2 detik. | **Must** | ESP32 Timer |
| **FR-02** | Kontrol Pemanas | Sistem harus menyalakan pemanas jika suhu $< 37.5^\circ\text{C}$ dan mematikannya jika suhu $\ge 37.8^\circ\text{C}$ (logika histeresis). | **Must** | ESP32 Controller |
| **FR-03** | Proteksi Overheat | Sistem harus menyalakan kipas *exhaust* pendingin secara otomatis jika suhu melampaui batas aman ($> 38.3^\circ\text{C}$). | **Must** | ESP32 Controller |
| **FR-04** | Pemutaran Rak Otomatis | Sistem harus memutar rak telur otomatis menggunakan motor TYD49 dengan interval rotasi setiap 4 jam selama durasi 3 detik. | **Must** | ESP32 Scheduler |
| **FR-05** | Failsafe Sensor Error | Sistem harus mematikan relay pemanas secara otomatis jika pembacaan sensor DHT22 mengalami galat (*NaN*) sebanyak 3 kali berturut-turut. | **Must** | ESP32 Safety Logic |
| **FR-06** | Candling Fotometri | Sistem harus mengaktifkan LED HPL, membaca nilai ADC sensor LDR, dan mengonversinya menjadi persentase intensitas cahaya tembus (0–100%). | **Must** | Pengguna / Tombol |
| **FR-07** | Klasifikasi Fertilitas | Sistem harus menetapkan status telur menjadi **Fertil** jika intensitas cahaya $< 50\%$ (terhalang embrio) dan **Afkir** jika $\ge 50\%$. | **Must** | ESP32 / Cloud Logic |
| **FR-08** | Isolasi Cahaya Candling | Sistem harus menonaktifkan lampu pemanas sementara waktu saat proses candling berlangsung agar tidak terjadi bias optik pada sensor LDR. | **Must** | ESP32 State Machine |
| **FR-09** | Pemetaan 30 Slot Digital | Sistem mobile harus memvisualisasikan kondisi rak telur dalam grid 30 slot (Slot 01–30) lengkap dengan status warna (Fertil, Afkir, Kosong). | **Must** | Aplikasi Flutter |
| **FR-10** | Pemicu Periksa Telur | Pengguna dapat memilih nomor slot telur tertentu di aplikasi dan menekan tombol *"Periksa Telur"* untuk menampilkan data hasil peneropongan. | **Must** | Peternak (Aplikasi) |
| **FR-11** | Sinkronisasi Cloud | Sistem harus menyinkronkan data pembacaan mikroklimat dan status slot telur secara *real-time* ke basis data Google Cloud Firestore. | **Must** | ESP32 / Firebase |
| **FR-12** | Notifikasi Bahaya (FCM) | Sistem harus mengirimkan *push notification* darurat ke smartphone pengguna saat terjadi deviasi suhu kritis ($< 36.5^\circ\text{C}$ atau $> 38.5^\circ\text{C}$). | **Must** | Cloud Function / FCM |
| **FR-13** | Notifikasi Jadwal Siklus | Sistem harus mengirimkan notifikasi pengingat jadwal kegiatan inkubasi pada hari ke-7 (Candling 1), hari ke-14 (Candling 2), dan hari ke-21 (Tetas). | **Should** | Cloud Function / FCM |
| **FR-14** | Override Mode Manual | Pengguna dapat mengalihkan sistem ke Mode Manual dari aplikasi Flutter untuk menghidupkan/mematikan lampu, kipas, atau motor secara langsung. | **Should** | Peternak (Aplikasi) |
| **FR-15** | Countdown Hari Inkubasi | Aplikasi harus menampilkan sisa waktu hari penetasan (*countdown counter*) yang dihitung otomatis sejak tanggal awal siklus (*start date*). | **Should** | Aplikasi Flutter |
| **FR-16** | Pencatatan Log Riwayat | Sistem harus mencatat setiap kejadian aktivitas (perubahan status slot, waktu peneropongan, aksi manual) ke dalam koleksi riwayat di Firestore. | **Should** | Cloud Firestore |
| **FR-17** | Indikator Heartbeat | Aplikasi harus menampilkan status konektivitas jaringan (*pill* indikator "TERHUBUNG") yang diperbarui berdasarkan sinyal detak aktif dari ESP32. | **Should** | Aplikasi Flutter |
| **FR-18** | Manajemen Profil Peternak | Pengguna dapat melihat identitas akun peternak dan mengaktifkan/menonaktifkan izin pengiriman notifikasi peringatan inkubator. | **Could** | Peternak (Aplikasi) |

---

## 6. Non-Functional Requirements (NFR)

### 6.1 Performance (Kinerja)
- **Latensi Telemetri:** Waktu transmisi data dari mikrokontroler ESP32 hingga terbarui pada tampilan layar aplikasi Flutter tidak boleh melebihi $2.0\text{ detik}$ pada kondisi bandwidth normal.
- **Responsivitas Antarmuka:** Aplikasi mobile harus beroperasi mulus dengan performa rendering antarmuka minimal 60 frame per second (FPS) tanpa *lag* transisi.
- **Efisiensi LCD Lokal:** Pembaruan teks LCD 16x2 lokal dibatasi pada interval $350\text{ ms}$ untuk mengeliminasi efek kedipan layar (*flickering*).

### 6.2 Security (Keamanan)
- **Enkripsi Saluran Komunikasi:** Seluruh lalu lintas data antara perangkat ESP32, Cloud Firestore, dan aplikasi Flutter wajib menggunakan protokol terenkripsi Transport Layer Security (TLS/HTTPS).
- **Kontrol Akses Basis Data:** Hak akses pembacaan dan penulisan dokumen Firestore dibatasi melalui konfigurasi *Firestore Security Rules* berbasis *Authentication UID* pengguna yang sah.

### 6.3 Usability (Kebergunaan & Aksesibilitas)
- **Hierarki Visual Jelas:** Menggunakan skema warna status standar: Hijau (`#10B981`) untuk kondisi Fertil/Normal, Merah (`#EF4444`) untuk kondisi Afkir/Bahaya, dan Kuning (`#FACC15`) untuk elemen interaktif terpilih.
- **Standar Evaluasi:** Sistem antarmuka harus mencapai skor kemudahan penggunaan $\ge 75$ berdasarkan instrumen kuesioner *System Usability Scale* (SUS).

### 6.4 Reliability & Safety (Keandalan & Keselamatan Operasional)
- **Operasional Berkelanjutan:** Perangkat keras harus mampu beroperasi secara terus-menerus (*24/7 continuous operation*) selama siklus penuh 21 hari tanpa mengalami *hang* atau *memory leak* (mengaktifkan *Hardware Watchdog Timer* pada ESP32).
- **Termal Failsafe:** Jika koneksi jaringan internet atau modul WiFi terputus, mikrokontroler ESP32 harus tetap menjalankan regulasi suhu otomatis secara lokal (*autonomous offline mode*).
- **Proteksi Transien Induktif & EMI (RC Snubber):** Penggunaan rangkaian *RC snubber* (resistor daya non-induktif dan kapasitor film metalisasi Class X2 AC) terpasang paralel pada kontak relai beban induktif AC (motor pembalik rak TYD49 dan elemen pemanas) guna menekan lonjakan transien tegangan balik (*back-EMF*), memadamkan busur api listrik (*arcing*), meniadakan interferensi elektromagnetik (*EMI*) yang dapat mereset mikrokontroler ESP32, serta memperpanjang masa pakai mekanis relai.

---

## 7. Business Process As-Is vs To-Be

| Dimensi Proses | Proses Saat Ini (*As-Is / Konvensional*) | Proses Usulan (*To-Be / Smart System*) | Nilai Tambah Bisnis (*Business Value*) |
| :--- | :--- | :--- | :--- |
| **Kontrol Suhu** | Manual; mengamati termometer dinding dan menyetel sekrup bimetal secara berulang. | Otomatis tertutup (*closed-loop* histeresis) oleh ESP32 dengan sensor DHT22. | Menghilangkan kelalaian manusia; menjaga suhu di rentang optimal $37.5^\circ\text{C}-37.8^\circ\text{C}$. |
| **Pemutaran Telur** | Peternak harus mendatangi inkubator 3–4 kali sehari untuk membalik telur manual. | Otomatis digerakkan oleh motor sinkron TYD49 setiap 4 jam secara presisi. | Menghemat waktu peternak hingga 90%; mencegah embrio lengket pada cangkang. |
| **Metode Candling** | Menyorotkan senter genggam di ruangan gelap; interpretasi visual manual tanpa angka ukur. | Pengukuran fotometri optoelektronik (HPL + LDR) dengan hasil terukur (0–100% intensitas). | Menghilangkan bias subjektif; akurasi terukur $\ge 85\%$. |
| **Dokumentasi Slot** | Catatan manual di buku kertas atau cangkang ditandai spidol; rentan terhapus/hilang. | Grid digital 30 slot tersimpan terpusat di Cloud Firestore dan tampil di aplikasi mobile. | Pelacakan kondisi riwayat per butir telur transparan dan terdata rapi. |
| **Respons Bahaya** | Pasif; peternak baru mengetahui lampu mati saat memeriksa fisik boks beberapa jam kemudian. | Proaktif; sistem mengirimkan *push notification* FCM seketika saat terdeteksi suhu kritis. | Mencegah kematian massal embrio (*zero casualty*) akibat kegagalan perangkat. |

---

## 8. Assumptions & Constraints

### 8.1 Asumsi (Assumptions)
1. Lokasi penempatan mesin tetas telur memiliki jangkauan sinyal jaringan Wi-Fi 2.4 GHz yang terhubung ke internet.
2. Telur tetas yang dimasukkan peternak adalah telur segar dengan umur simpan pasca-bertelur tidak lebih dari 7 hari.
3. Catu daya listrik utama berasal dari jala-jala listrik PLN yang disuplai melalui adaptor switching 12V 3A–5A yang stabil.

### 8.2 Batasan (Constraints)
1. **Keterbatasan Memori Mikrokontroler:** Kapasitas RAM (~520 KB) dan Flash Memory (4 MB) ESP32 membatasi pemrosesan komputasi berat, sehingga pemrosesan grafis tidak dilakukan di mikrokontroler.
2. **Karakteristik Modul Relay:** Modul relay fisik yang digunakan berjenis *Active LOW*, sehingga logika pemicuan firmware harus disesuaikan secara konsisten.
3. **Batas Kuota Layanan Cloud (Firebase Spark Plan):** Frekuensi penulisan telemetri dibatasi maksimal 1 kali per menit agar akumulasi penulisan data harian ($\approx 1.440$ penulisan/hari) tidak melebihi kuota gratis Firestore (50.000 penulisan/hari).
4. **Non-Linearitas ADC ESP32:** Pembacaan pin analog (ADC1 GPIO 26) memiliki keterbatasan non-linearitas di bawah 0.1V dan di atas 3.2V, sehingga kalibrasi sensor LDR difokuskan pada rentang linier tegangan tengah.

---

## 9. Risk Register

| ID | Deskripsi Risiko (Risk Event) | Probabilitas | Dampak | Tingkat Risiko | Rencana Mitigasi Terukur |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **R-01** | Koneksi Wi-Fi terputus di tengah siklus penetasan. | **Medium** | **High** | **High** | Firmware ESP32 dirancang memiliki logika kendali otonom (*offline mode*); regulasi suhu dan pemutaran rak tetap berjalan normal tanpa ketergantungan internet. |
| **R-02** | Kabel sensor DHT22 rusak atau lepas (*hardware disconnect*). | **Low** | **High** | **High** | Diterapkan algoritma *safety failsafe*: jika pembacaan menghasilkan nilai `NaN` $\ge 3$ kali berturut-turut, sistem otomatis memutus aliran listrik pemanas untuk mencegah kebakaran. |
| **R-03** | Pembacaan sensor LDR terdistorsi oleh cahaya lampu pemanas di dalam boks. | **High** | **Medium** | **Medium** | Dibuat tabung teropong isolasi cahaya (*optical shroud*) dan firmware secara otomatis mematikan lampu pemanas sesaat selama proses candling berlangsung. |
| **R-04** | Latensi pengiriman push notifikasi FCM tertunda oleh pembatasan baterai OS Android (*Doze Mode*). | **Medium** | **Medium** | **Medium** | Payload pesan FCM dikonfigurasi menggunakan atribut prioritas tinggi (`priority: "high"` dan *Android Notification Channel* dengan level *Importance Max*). |
| **R-05** | Motor pemutar rak macet atau mekanik rak terhambat. | **Low** | **Medium** | **Low** | Pembatasan durasi aktif motor (*run timeout*) maksimal 3 detik per siklus untuk mencegah motor terbakar atau *overheating*. |

---

## 10. Acceptance Criteria (Kriteria Keberterimaan)

Sistem dinyatakan memenuhi spesifikasi bisnis dan lulus uji kelayakan tugas akhir apabila memenuhi kriteria berikut:

1. **Pengujian Fungsionalitas Mikroklimat:**
   - [x] Pemanas menyala saat suhu terukur $< 37.5^\circ\text{C}$ dan mati saat $\ge 37.8^\circ\text{C}$.
   - [x] Kipas pendingin menyala saat suhu $> 38.3^\circ\text{C}$ dan mati saat $\le 37.8^\circ\text{C}$.
   - [x] Motor rak berputar otomatis selama 3 detik pada setiap siklus interval yang ditentukan.
2. **Pengujian Deteksi Fertilitas:**
   - [x] LED HPL menyala dan sensor LDR membaca intensitas saat tombol *"Periksa Telur"* ditekan.
   - [x] Sistem menampilkan persentase intensitas cahaya (0–100%) dan melabeli status "Fertil" atau "Afkir" secara konsisten sesuai ambang batas.
   - [x] Data hasil candling berhasil tersimpan dan terbarui pada nomor slot yang dipilih di grid 30 slot Firestore.
3. **Pengujian Aplikasi Mobile & Cloud:**
   - [x] Dashboard menampilkan nilai suhu dan kelembapan yang sinkron dengan tampilan LCD fisik mesin tetas.
   - [x] Notifikasi FCM berhasil diterima di smartphone pengguna dalam waktu $< 3.0\text{ detik}$ saat kondisi suhu kritis disimulasikan.
   - [x] Mode kontrol manual dari aplikasi Flutter berhasil mengubah status fisik aktuator (lampu/kipas/motor) secara langsung.
4. **Pengujian Kualitas Perangkat Lunak:**
   - [x] Evaluasi kemudahan penggunaan aplikasi mobile oleh pengguna menghasilkan skor SUS minimal 75.0.
   - [x] Firmware ESP32 beroperasi stabil tanpa restart mendadak selama minimal 72 jam pengujian tahap verifikasi.
