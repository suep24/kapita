# JAWABAN UJIAN TENGAH SEMESTER (UTS)
## MATA KULIAH: KAPITA SELEKTA TEKNIK INFORMATIKA

---

| Identitas | Keterangan |
| :--- | :--- |
| **Nama Mahasiswa** | **Fransiskus Desales Bayu Priatmoko** |
| **NIM** | **20230801109** |
| **Program Studi** | Teknik Informatika |
| **Bentuk Evaluasi** | UTS – Essay (A1–A5) & Studi Kasus (B1–B3) |

---

## Bagian A – Essay

### A1. Ketentuan Hukum Digital dan Prinsip Etika Profesi Terkait Pengumpulan Data & Lisensi Modul (Bobot: 8)

> **Kasus:** Sebuah tim proyek mengembangkan aplikasi antrean fasilitas publik dengan mengumpulkan data identitas dan riwayat kunjungan pengguna, serta menyalin modul dari repositori perangkat lunak terbuka tanpa mencantumkan informasi lisensinya.

#### 1. Ketentuan Hukum Digital yang Relevan:
* **Undang-Undang No. 27 Tahun 2022 tentang Pelindungan Data Pribadi (UU PDP):**  
  Sangat relevan dengan tindakan tim dalam menghimpun data identitas dan riwayat kunjungan pengguna. Berdasarkan **Pasal 20 jo. Pasal 21 UU PDP**, pengendali data wajib memiliki dasar pemrosesan data pribadi yang sah, salah satunya melalui persetujuan eksplisit (*explicit consent*) dari subjek data. Selain itu, **Pasal 35** mewajibkan tim menjamin keamanan dan kerahasiaan data tersebut dari risiko kebocoran data atau pemanfaatan di luar tujuan awal layanan antrean.
* **Undang-Undang No. 28 Tahun 2014 tentang Hak Cipta & UU ITE:**  
  Sangat relevan dengan tindakan menyalin modul dari repositori terbuka tanpa mencantumkan informasi lisensinya. Berdasarkan **Pasal 40 ayat (1) huruf s UU Hak Cipta**, program komputer diakui secara hukum sebagai karya cipta yang dilindungi. Perangkat lunak *open-source* tetap memiliki hak cipta (*copyright*); penggunaan modul tanpa menyertakan lisensi dan atribusi melanggar hak moral dan hak ekonomi pencipta asli.

#### 2. Prinsip Etika Profesi (ACM/IEEE Code of Ethics):
* **Menghormati Privasi (*Respect Privacy* - ACM Code 1.6):**  
  Profesional informatika berkewajiban melindungi informasi personal pengguna fasilitas publik, memastikan transparansi penuh mengenai data apa saja yang dicatat, serta menjamin riwayat kunjungan tidak diekspos, diperjualbelikan, atau disalahgunakan tanpa izin.
* **Menghormati Hak Milik dan Kekayaan Intelektual (*Honor Property Rights* - ACM Code 1.5):**  
  Prinsip ini melarang segala bentuk plagiarisme perangkat lunak. Seorang insinyur perangkat lunak wajib menjunjung integritas intelektual dengan mencantumkan atribusi, mematuhi syarat lisensi *open-source*, dan tidak mengklaim kode pihak lain sebagai karya mandiri tim.

---

### A2. Analisis Pelanggaran IDOR dan Penggunaan Dataset Komersial AI (Bobot: 8)

#### Kasus 1: Perubahan Parameter URL pada Sistem Informasi Kelurahan (IDOR)
* **Potensi Pelanggaran:**  
  Akses tidak sah terhadap data pribadi warga negara (pelanggaran **UU ITE Pasal 30** tentang akses ilegal terhadap sistem komputer) serta pelanggaran asas kerahasiaan data pribadi (**UU PDP**).
* **Penyebab Teknis:**  
  Terjadi kerentanan ***Insecure Direct Object References* (IDOR)** atau ***Broken Access Control*** (kategori teratas OWASP Top 10 A01:2021). Sistem mempercayai parameter input di URL (misalnya: `?id=123`) dan langsung memprosesnya tanpa melakukan validasi otorisasi di sisi *backend* apakah sesi (*session*) pengguna yang sedang aktif berhak melihat data objek tersebut.
* **Usulan Mitigasi:**  
  1. Menerapkan kontrol otorisasi berbasis peran yang ketat di sisi *backend* (*Role-Based Access Control* / RBAC) yang memverifikasi kecocokan antara *session token* pemohon dengan data target pada setiap *endpoint*.
  2. Menggunakan pengaburan *identifier* (*Indirect Reference Maps*) atau UUID v4 alih-alih ID sekuensial integer yang mudah ditebak (*enumeration attack*).

#### Kasus 2: Pelatihan Model AI Komersial dengan Dataset Nonkomersial
* **Potensi Pelanggaran:**  
  Pelanggaran hak cipta dan wanprestasi lisensi (*breach of contract*) dataset terbuka (misalnya lisensi *Creative Commons Non-Commercial* / CC BY-NC). Pelanggaran ini dapat berakibat pada gugatan ganti rugi perdata serta tuntutan penarikan produk AI komersial dari peredaran.
* **Penyebab Prosedural:**  
  Ketiadaan proses *due diligence* (uji kelayakan) serta absennya audit kepatuhan lisensi (*license compliance audit*) dalam *pipeline* pengembangan sebelum aset data pihak ketiga dimasukkan ke dalam produk komersial.
* **Usulan Mitigasi:**  
  1. Membentuk prosedur audit lisensi data (*data compliance governance*) sebelum *training* model AI.
  2. Mengganti dataset dengan dataset berlisensi komersial yang sah (misal: CC-BY, MIT, CDLA) atau dataset sintetis.
  3. Menghubungi pemilik dataset untuk mengajukan permohonan lisensi komersial resmi (*dual licensing*).

---

### A3. Rancangan Penanganan Data dan Lisensi Proyek Layanan Kesehatan (Bobot: 8)

Dalam proyek pendataan kesehatan komunitas dengan mitra eksternal dan ketergantungan modul multi-lisensi, disusun rancangan tata kelola sebagai berikut:

#### 1. Klasifikasi Data (Berdasarkan UU PDP Pasal 4):
* **Data Pribadi Bersifat Umum:** Nama lengkap, jenis kelamin, alamat domisili, dan nomor telepon kontak.
* **Data Pribadi Bersifat Spesifik/Sensitif:** Data rekam medis, riwayat kesehatan, diagnosis penyakit, serta data biometrik/genetik yang memerlukan perlindungan enkripsi tingkat tinggi (*AES-256 at rest & TLS 1.3 in transit*).

#### 2. Dasar Persetujuan dan Pembatasan Akses:
* **Persetujuan Eksplisit (*Informed Consent*):** Subjek data menandatangani persetujuan (digital/tertulis) yang merinci secara transparan tujuan survei, batasan pemrosesan, dan pihak eksternal penerima data.
* **Pembatasan Akses (*Least Privilege*):** Hak akses berbasis peran ketat (RBAC). Data mentah sensitif hanya dapat diakses oleh petugas medis berwenang dengan prinsip *need-to-know*.
* **Berbagi dengan Mitra Eksternal:** Data yang dibagikan wajib melalui proses de-identifikasi atau anonimisasi (*k-anonymity* / diferensial privasi) sehingga tidak dapat diatribusikan kembali ke individu, disertai penandatanganan perjanjian legal *Data Sharing Agreement* (DSA).

#### 3. Pengelolaan Lisensi Pustaka Pihak Ketiga:
* ***Software Bill of Materials* (SBOM):** Mendokumentasikan seluruh pustaka, versi dependensi, dan jenis lisensi secara otomatis dalam *pipeline* CI/CD.
* **Pemeriksaan Kompatibilitas Lisensi:** Mencegah terjadinya benturan hukum antara lisensi *copyleft* kuat (seperti GPLv3) dengan arsitektur modul proyek jika berstatus *proprietary* atau *permissive*.
* **Dokumentasi Atribusi:** Menyediakan berkas `LICENSE` dan `NOTICE` yang memuat nama pemegang hak cipta seluruh pustaka pihak ketiga.

#### 4. Prosedur Penanganan Insiden Kebocoran Data (*Incident Response Plan*):
* **Tahap 1 (Isolasi & *Containment*):** Memutus akses jaringan pada modul yang terkompromi untuk menghentikan eksfiltrasi data lanjutan.
* **Tahap 2 (Investigasi Forensik):** Menganalisis *audit log* untuk mengidentifikasi cakupan data yang bocor dan vektor serangan.
* **Tahap 3 (Notifikasi Wajib 3 x 24 Jam):** Sesuai amanat **Pasal 46 UU PDP**, menyampaikan pemberitahuan tertulis resmi kepada lembaga pengawas pelindungan data (Kominfo/BSSN) dan subjek data terdampak selambat-lambatnya 3 x 24 jam.
* **Tahap 4 (Remediasi & *Post-Mortem*):** Menambal celah kerentanan, memperbarui enkripsi, dan menyusun laporan evaluasi pencegahan.

---

### A4. Kaidah Integritas Akademik dalam Kerja Kelompok Informatika (Bobot: 8)

* **Kaidah Integritas Akademik:**  
  Menjunjung tinggi prinsip kejujuran ilmiah, keadilan (*fairness*), akuntabilitas, serta penghormatan terhadap orisinalitas karya cipta. Setiap anggota bertanggung jawab atas orisinalitas hasil kerja kelompok dan wajib memberikan pengakuan eksplisit terhadap kontribusi pihak luar maupun pembagian kerja internal.

#### Contoh Penerapan Pengakuan Sumber dan Pembagian Peran yang Tepat:
1. **Pengakuan Sumber yang Tepat:**  
   Saat tim mengadopsi fungsi algoritma *sorting* atau modul autentikasi dari StackOverflow atau repositori GitHub, tim menyertakan blok komentar (*header comment*) yang memuat URL sumber asli, nama *author*, jenis lisensi, dan catatan adaptasi pada berkas kode sumber, serta mencantumkannya pada daftar pustaka laporan akhir.
2. **Pembagian Peran yang Tepat:**  
   Tim menyusun matriks tanggung jawab (RACI Matrix) dan papan *Kanban* digital (GitHub Projects/Jira). Misalnya: Anggota A sebagai *Backend Engineer* bertanggung jawab atas perancangan REST API dan skema basis data; Anggota B sebagai *Frontend Engineer* mengimplementasikan antarmuka antrean; Anggota C sebagai *QA & Technical Writer* merancang *test case* dan dokumentasi arsitektur. Setiap anggota memiliki riwayat *commit* dan tiket tugas yang dapat diverifikasi secara mandiri.

#### Contoh Pelanggaran Integritas Akademik & Prosedur Penanganannya:
* **Pelanggaran:**  
  Seorang anggota menyalin *template* arsitektur *frontend* utuh dari repositori publik dan mengklaimnya sebagai desain mandiri di depan dosen pengampu saat sesi asistensi.
* **Prosedur Penanganan:**  
  Tim melakukan teguran formal internal dan mewajibkan anggota tersebut mencantumkan sitasi serta merevisi kode/laporan. Jika menolak, tim melaporkan riwayat pembagian tugas dan bukti plagiasi secara transparan kepada dosen pembimbing agar penilaian dilakukan objektif dan sanksi akademik dikenakan secara individual kepada pelanggar.

---

### A5. Kerangka Penilaian Kontribusi Tiap Anggota Berbasis Bukti Objektif (Bobot: 8)

Untuk mengatasi ketidakseimbangan kontribusi dan fenomena *free-rider*, disusun kerangka evaluasi berbasis artefak kerja yang dapat diverifikasi:

#### 1. Kerangka Penilaian Berbasis Bukti Verifikasi:
* **Artefak Kode & Implementasi (Bobot 40%):**  
  Diverifikasi melalui log repositori Git (*Git commit history*, *Pull Requests*). Penilaian ditekankan pada kompleksitas logika, kerapian arsitektur modular, dan fungsionalitas fitur, bukan sekadar jumlah baris kode (*Lines of Code* / LOC).
* **Artefak Dokumentasi & Pengujian (Bobot 30%):**  
  Diverifikasi melalui riwayat revisi dokumen teknis (*Google Docs edit history*), diagram arsitektur sistem (UML/C4 model), serta dokumen skenario dan hasil uji perangkat lunak (*test scripts*).
* **Kolaborasi & Manajemen Proyek (Bobot 30%):**  
  Diverifikasi melalui keterlibatan aktif dalam *code review* / *PR review*, penyelesaian *issue* pada papan manajemen tugas, serta partisipasi dalam pertemuan koordinasi berkala.

#### 2. Prosedur Penanganan Anggota Pasif (*Free-Rider*) secara Objektif:
* **Peringatan Dini (*Early Warning*):** Memberikan teguran internal dan menetapkan tenggat waktu korektif di pertengahan fase proyek sebelum penilaian akhir.
* **Formulir *Peer-Evaluation* Tertutup:** Setiap anggota mengisi evaluasi anonim dan terukur terhadap kontribusi rekan satu tim.
* **Pelaporan Transparan kepada Dosen:** Jika anggota tidak memiliki bukti catatan *commit* maupun artefak dokumen, ketua tim melampirkan log kerja objektif dan mencantumkan "Kontribusi: 0%" pada catatan pembagian tugas. Dosen memberikan nilai individual yang proporsional sehingga anggota yang aktif terlindungi dari kerugian nilai.

---

## Bagian B – Studi Kasus (Koperasi Mahasiswa)

> **Skenario:** Pengembangan aplikasi pencatatan persediaan koperasi mahasiswa mengalami permasalahan integritas akademik: modul autentikasi disalin tanpa lisensi, peran kerja tidak tercatat, dua anggota mengklaim modul yang sama, dan ketua tim mengusulkan untuk menghapus jejak kode luar serta memanipulasi laporan agar kontribusi tampak rata.

### B1. Kaidah Pengakuan Sumber Kode Terbuka dan Unsur Informasi Wajib (Bobot: 10)

Meskipun kode sumber berstatus *open-source*, penggunaannya tetap tunduk pada ketentuan hukum hak cipta dan lisensi perangkat lunak. Tim wajib menerapkan kaidah atribusi dan kepatuhan lisensi (*license compliance*).

#### Unsur informasi yang wajib dicantumkan dalam kode sumber dan laporan akhir:
1. **Nama Pencipta / Pemegang Hak Cipta Asli:** Nama lengkap atau organisasi pembuat kode (misalnya: *Original Author: Jane Doe / OpenAuth Project*).
2. **Tautan (URL) Repositori Sumber:** Alamat repositori publik asli dan spesifik *commit hash* yang diacu untuk keperluan audit jejak digital.
3. **Pernyataan Hak Cipta (*Copyright Notice*):** Pernyataan hak cipta lengkap dari berkas asli (contoh: `Copyright (c) 2024 Jane Doe`).
4. **Teks / Jenis Lisensi Terbuka:** Nama lisensi yang digunakan (misalnya: MIT License, Apache License 2.0, atau GNU GPL) serta memastikan teks lisensi disertakan dalam repositori tim.
5. **Catatan Perubahan (*Modification / Change Notice*):** Keterangan eksplisit mengenai bagian apa saja yang telah dimodifikasi atau disesuaikan untuk integrasi dengan sistem koperasi mahasiswa (misal: *Modified JWT payload schema to match Kopma role structure*).

---

### B2. Peran Pembagian Tugas Tercatat dan Dampak Bila Tidak Terdokumentasi (Bobot: 10)

#### 1. Peran Pembagian Tugas yang Tercatat sebagai Dasar Akuntabilitas:
* **Instrumen Akuntabilitas dan Audit Jejak (*Audit Trail*):** Memastikan setiap modul perangkat lunak memiliki penanggung jawab yang jelas secara teknis maupun etis.
* **Distribusi Beban Kerja yang Berkeadilan (*Fairness*):** Mencegah eksploitasi beban kerja pada sebagian anggota serta meniadakan ruang bagi *free-rider*.
* **Dasar Penilaian Objektif oleh Dosen:** Menjadi instrumen primer bagi dosen untuk mengevaluasi kompetensi individual mahasiswa di dalam tim proyek rekayasa perangkat lunak.

#### 2. Dampak Negatif Apabila Pembagian Peran Tidak Tercatat:
* **Timbulnya Klaim Ganda (*Conflicting Claims*):** Sebagaimana terjadi pada kasus, dua anggota mengklaim mengerjakan seluruh modul autentikasi tanpa bukti yang dapat diverifikasi.
* **Kerugian Kolektif (*Blame-Sharing*):** Jika terjadi pelanggaran hak cipta atau kegagalan fungsi fatal, seluruh tim akan dikenakan sanksi akademik karena kegagalan melokalisasi tanggung jawab individu.
* **Rusaknya Dinamika dan Motivasi Tim:** Anggota yang bekerja keras kehilangan motivasi karena pencapaiannya disamakan dengan anggota yang tidak berkontribusi.

---

### B3. Rekomendasi Langkah Tindakan Tim Memenuhi Integritas Akademik (Bobot: 10)

Usulan ketua tim untuk menghapus sumber luar dan merekayasa pembagian kontribusi rata merupakan bentuk penipuan akademik (*academic fraud*) dan plagiarisme terencana. Sebagai anggota tim yang menjunjung etika profesi, langkah tindakan yang wajib diambil adalah:

1. **Menolak Usulan Manipulasi Secara Tegas:**  
   Menolak rencana manipulasi laporan dan menjelaskan kepada ketua serta anggota tim bahwa menghapus referensi kode pihak ketiga adalah pelanggaran integritas akademik berat yang dapat mengakibatkan pembatalan mata kuliah. Kejujuran akademik jauh lebih berharga daripada manipulasi nilai sementara.
2. **Remediasi Kode Sumber dan Atribusi:**  
   Memasukkan kembali blok komentar atribusi lengkap (nama *author*, lisensi, link repositori, dan catatan modifikasi) pada berkas modul autentikasi, serta menyertakan salinan lisensi aslinya ke dalam repositori tim.
3. **Transparansi Penulisan Laporan Akhir:**  
   Mendokumentasikan secara terbuka pada Bab Metodologi dan Implementasi bahwa *"Modul autentikasi diadaptasi dari repositori terbuka [Nama Sumber] dengan modifikasi logika sesi untuk kebutuhan koperasi"*. Hal ini menunjukkan kemampuan mahasiswa dalam rekayasa perangkat lunak berbasis integrasi modul secara etis.
4. **Rekonsiliasi dan Penyusunan Matriks Kontribusi Faktual:**  
   Menyusun matriks kontribusi berdasarkan jejak verifikasi nyata (log *commit* Git, riwayat pengeditan dokumen, dan notulen koordinasi). Terkait klaim dua anggota yang mengaku mengerjakan seluruh modul, tim melakukan klarifikasi detail peran masing-masing (misal: satu anggota merancang *backend hashing* & token JWT, anggota lain merancang validasi *form frontend*) sehingga klaim tumpang tindih dapat diselesaikan secara objektif.
5. **Penyerahan Laporan Transparan kepada Dosen Pengampu:**  
   Menyerahkan catatan kontribusi faktual dan laporan akhir kepada dosen dengan itikad baik. Tindakan ini menjamin hasil analisis tim dapat diaudit, terhindar dari sanksi plagiarisme, serta menjadi fondasi metodologis yang solid dan jujur untuk penyusunan proposal Tugas Akhir.
