# Function Point Analysis (FPA)
## Smart Classroom Digital Twin System

Dokumen ini menyajikan estimasi ukuran fungsional perangkat lunak **Smart Classroom Digital Twin System** menggunakan metode **IFPUG Function Point Analysis (FPA)**. Seluruh fungsi yang dihitung diturunkan langsung dari ruang lingkup kebutuhan fungsional pada `USER-STORY.md` dan kontrak layanan pada `API-SPEC.md`.

---

## 1. Asumsi dan Batasan Estimasi

1. **Lingkup Fungsional:** Perhitungan mencakup seluruh modul yang direncanakan pada versi rilis awal (Sprint 1–Sprint 4), yaitu: Autentikasi & Profil, Manajemen Ruang Kelas, Manajemen Jadwal, Simulasi Okupansi, Visualisasi *Digital Twin Overview*, dan Analitik Utilisasi.
2. **Arsitektur *Software-Only*:** Karena sistem tidak terhubung dengan perangkat keras IoT fisik maupun sistem akademik eksternal (SIAKAD universitas) pada tahap pengembangan saat ini, seluruh data dikelola secara mandiri di dalam basis data aplikasi (*Internal Logical Files*). Dengan demikian, komponen **External Interface Files (EIF)** bernilai **0**.
3. **Standar Bobot IFPUG:** Penilaian kompleksitas (*Low*, *Average*, *High*) ditentukan berdasarkan perkiraan jumlah *Data Element Types* (DET), *Record Element Types* (RET), dan *File Types Referenced* (FTR) pada masing-masing transaksi.

---

## 2. Tabel Bobot Standar Komponen Function Point

| Komponen Function Point | Low | Average | High |
| :--- | :---: | :---: | :---: |
| **External Input (EI)** | 3 | 4 | 6 |
| **External Output (EO)** | 4 | 5 | 7 |
| **External Inquiry (EQ)** | 3 | 4 | 6 |
| **Internal Logical File (ILF)** | 7 | 10 | 15 |
| **External Interface File (EIF)** | 5 | 7 | 10 |

---

## 3. Identifikasi dan Klasifikasi Fungsi Sistem

### 3.1 External Input (EI)
*External Input* adalah proses elementer yang memproses data atau sinyal kontrol dari luar batas sistem untuk menambah, mengubah, atau menghapus data pada ILF.

| No | Nama Fungsi (EI) | Deskripsi & Kompleksitas (FTR / DET) | Tingkat Kompleksitas | Bobot |
| :---: | :--- | :--- | :---: | :---: |
| 1 | Login Pengguna | Validasi kredensial & pembuatan sesi (1 FTR, ~4 DET) | Low | 3 |
| 2 | Pembaruan Profil Pengguna | Mengubah data profil akun aktif (1 FTR, ~5 DET) | Low | 3 |
| 3 | Tambah Data Ruang Kelas | Menambah ruang kelas & inisialisasi twin (2 FTR, ~10 DET) | Average | 4 |
| 4 | Ubah Data Ruang Kelas | Memperbarui atribut & fasilitas ruang (2 FTR, ~10 DET) | Average | 4 |
| 5 | Hapus / Nonaktifkan Ruang Kelas | Validasi jadwal & penonaktifan ruang (2 FTR, ~3 DET) | Low | 3 |
| 6 | Pengaturan Kapasitas Ruangan | Mengubah kapasitas & recalibrate rasio (2 FTR, ~4 DET) | Low | 3 |
| 7 | Tambah Jadwal Perkuliahan | Simpan jadwal + cek bentrok ruang, dosen & kapasitas (3 FTR, ~11 DET) | High | 6 |
| 8 | Ubah Jadwal Perkuliahan | Perbarui jadwal + validasi ulang konflik waktu (3 FTR, ~11 DET) | High | 6 |
| 9 | Hapus Jadwal Perkuliahan | Menghapus entri jadwal perkuliahan (1 FTR, ~2 DET) | Low | 3 |
| 10 | Simulasi Okupansi Ruang Kelas | Input jumlah penghuni simulasi & log riwayat (2 FTR, ~7 DET) | Average | 4 |
| 11 | Pembaruan State Kondisi Digital Twin | Ubah state virtual suhu, AC, lampu, proyektor (2 FTR, ~8 DET) | Average | 4 |

- **Rekapitulasi EI:**
  - Low: $5 \times 3 = 15$
  - Average: $4 \times 4 = 16$
  - High: $2 \times 6 = 12$
  - **Total EI = $15 + 16 + 12 = 43$**

### 3.2 External Output (EO)
*External Output* adalah proses elementer yang menyajikan data ke pengguna dengan melibatkan kalkulasi matematis, data turunan (*derived data*), agregasi, atau representasi grafis.

| No | Nama Fungsi (EO) | Deskripsi & Kompleksitas (FTR / DET) | Tingkat Kompleksitas | Bobot |
| :---: | :--- | :--- | :---: | :---: |
| 1 | Ringkasan KPI Dashboard | Agregasi total ruang, kelas aktif, rata-rata okupansi (3 FTR, ~6 DET) | Average | 5 |
| 2 | Laporan & Grafik Utilisasi Ruang | Kalkulasi persentase utilisasi per gedung & periode (4 FTR, ~12 DET) | High | 7 |
| 3 | Analisis Jam Sibuk (*Peak Hours*) | Agregasi kepadatan kelas per slot waktu harian (3 FTR, ~10 DET) | High | 7 |
| 4 | Visualisasi Digital Twin Overview | Pemetaan status visual, rasio kapasitas & jadwal aktif (4 FTR, ~15 DET) | High | 7 |
| 5 | Ringkasan Okupansi & Penggunaan | Perhitungan persentase okupansi historis per kelas (2 FTR, ~8 DET) | Average | 5 |

- **Rekapitulasi EO:**
  - Low: $0 \times 4 = 0$
  - Average: $2 \times 5 = 10$
  - High: $3 \times 7 = 21$
  - **Total EO = $0 + 10 + 21 = 31$**

### 3.3 External Inquiry (EQ)
*External Inquiry* adalah proses pengambilan dan penyajian data langsung tanpa melibatkan kalkulasi kompleks atau pembaruan basis data.

| No | Nama Fungsi (EQ) | Deskripsi & Kompleksitas (FTR / DET) | Tingkat Kompleksitas | Bobot |
| :---: | :--- | :--- | :---: | :---: |
| 1 | Lihat Profil Pengguna Aktif | Mengambil data pengguna terautentikasi (1 FTR, ~6 DET) | Low | 3 |
| 2 | Daftar & Filter Ruang Kelas | Pencarian ruang dengan multi-filter gedung/status (2 FTR, ~10 DET) | Average | 4 |
| 3 | Detail Informasi Ruang Kelas | Menampilkan atribut satu ruang kelas (1 FTR, ~9 DET) | Low | 3 |
| 4 | Daftar & Filter Jadwal Perkuliahan | Pencarian jadwal berdasar ruang, hari & dosen (3 FTR, ~10 DET) | Average | 4 |
| 5 | Detail Digital Twin per Ruang Kelas | Menampilkan layout, fasilitas & jadwal harian ruang (3 FTR, ~12 DET) | Average | 4 |
| 6 | Lihat Status Okupansi Terkini | Mengambil status okupansi ruang saat ini (2 FTR, ~7 DET) | Low | 3 |

- **Rekapitulasi EQ:**
  - Low: $3 \times 3 = 9$
  - Average: $3 \times 4 = 12$
  - High: $0 \times 6 = 0$
  - **Total EQ = $9 + 12 + 0 = 21$**

### 3.4 Internal Logical Files (ILF)
*Internal Logical Files* adalah kelompok data logis yang dikelola dan dipelihara di dalam batas aplikasi (basis data PostgreSQL).

| No | Nama File Logis (ILF) | Deskripsi & Kompleksitas (RET / DET) | Tingkat Kompleksitas | Bobot |
| :---: | :--- | :--- | :---: | :---: |
| 1 | `Users & Roles` | Data pengguna, kredensial & peran RBAC (1 RET, ~8 DET) | Low | 7 |
| 2 | `Classrooms & Facilities` | Data master ruang kelas, kapasitas & fasilitas (2 RET, ~14 DET) | Average | 10 |
| 3 | `Lecture Schedules` | Data jadwal perkuliahan, mata kuliah & waktu (1 RET, ~10 DET) | Low | 7 |
| 4 | `Occupancy & Simulation Logs` | Riwayat log simulasi okupansi & stempel waktu (2 RET, ~12 DET) | Average | 10 |
| 5 | `Digital Twin Room States` | State virtual ruangan, suhu simulasi & tata letak (2 RET, ~14 DET) | Average | 10 |

- **Rekapitulasi ILF:**
  - Low: $2 \times 7 = 14$
  - Average: $3 \times 10 = 30$
  - High: $0 \times 15 = 0$
  - **Total ILF = $14 + 30 + 0 = 44$**

### 3.5 External Interface Files (EIF)
*External Interface Files* adalah kelompok data eksternal yang hanya dibaca oleh sistem.

| No | Nama File (EIF) | Keterangan | Tingkat Kompleksitas | Bobot |
| :---: | :--- | :--- | :---: | :---: |
| – | Tidak Ada | Sistem bersifat mandiri (*software-only*) tanpa integrasi sistem eksternal atau perangkat IoT pada fase ini. | – | 0 |

- **Total EIF = 0**

---

## 4. Perhitungan Unadjusted Function Point (UFP)

$$\text{UFP} = \text{Total EI} + \text{Total EO} + \text{Total EQ} + \text{Total ILF} + \text{Total EIF}$$

| Komponen | Jumlah Low | Jumlah Average | Jumlah High | Total Skor Komponen |
| :--- | :---: | :---: | :---: | :---: |
| **External Input (EI)** | $5 \times 3 = 15$ | $4 \times 4 = 16$ | $2 \times 6 = 12$ | **43** |
| **External Output (EO)** | $0 \times 4 = 0$ | $2 \times 5 = 10$ | $3 \times 7 = 21$ | **31** |
| **External Inquiry (EQ)** | $3 \times 3 = 9$ | $3 \times 4 = 12$ | $0 \times 6 = 0$ | **21** |
| **Internal Logical File (ILF)** | $2 \times 7 = 14$ | $3 \times 10 = 30$ | $0 \times 15 = 0$ | **44** |
| **External Interface File (EIF)** | $0 \times 5 = 0$ | $0 \times 7 = 0$ | $0 \times 10 = 0$ | **0** |
| **Total Unadjusted Function Point (UFP)** | | | | **139** |

---

## 5. General System Characteristics (GSC) dan Value Adjustment Factor (VAF)

Penilaian 14 karakteristik umum sistem (*General System Characteristics*) menggunakan skala $0$ (tidak berpengaruh) hingga $5$ (berpengaruh kuat):

| No | General System Characteristics (GSC) | Nilai (0–5) | Justifikasi Teknis |
| :---: | :--- | :---: | :--- |
| 1 | Data Communications | 4 | Aplikasi web client-server menggunakan protokol HTTPS dan REST API JSON secara intensif. |
| 2 | Distributed Data Processing | 2 | Pemrosesan terpisah antara antarmuka klien dan server API, namun basis data terpusat. |
| 3 | Performance | 3 | Membutuhkan waktu respons cepat untuk memuat peta *Digital Twin Overview* dan grafik analitik. |
| 4 | Heavily Used Configuration | 3 | Berjalan pada infrastruktur server web kampus standar berbasis *container*. |
| 5 | Transaction Rate | 3 | Peningkatan transaksi terjadi secara berkala saat pergantian jam perkuliahan dan penyusunan jadwal. |
| 6 | Online Data Entry | 4 | Seluruh entri ruang, kapasitas, jadwal, dan simulasi okupansi dilakukan secara daring melalui web. |
| 7 | End-User Efficiency | 4 | Antarmuka dilengkapi *sidebar navigation*, filter cepat, kartu KPI, dan representasi visual intuitif. |
| 8 | Online Update | 4 | Setiap perubahan ruang, jadwal, dan state Digital Twin langsung memperbarui basis data secara *real-time*. |
| 9 | Complex Processing | 3 | Melibatkan algoritma deteksi konflik jadwal, kalkulasi rasio kapasitas, dan agregasi jam sibuk. |
| 10 | Reusability | 3 | Komponen UI React dan layanan REST API dirancang modular untuk pengembangan lanjutan. |
| 11 | Installation Ease | 4 | Didukung konfigurasi Docker Compose dan skrip migrasi otomatis untuk kemudahan instalasi. |
| 12 | Operational Ease | 3 | Dilengkapi manajemen log terpusat, otorisasi RBAC, dan kemudahan pemeliharaan server web. |
| 13 | Multiple Sites | 3 | Dapat memodelkan banyak gedung dan lantai ruang kuliah di lingkungan kampus universitas. |
| 14 | Facilitate Change | 4 | Dirancang menggunakan metodologi Agile Scrum agar mudah diperluas pada Sprint berikutnya. |
| | **Total Degree of Influence (TDI)** | **47** | |

### Perhitungan Value Adjustment Factor (VAF)

$$\text{VAF} = 0.65 + (0.01 \times \text{TDI})$$

$$\text{VAF} = 0.65 + (0.01 \times 47) = 0.65 + 0.47 = \mathbf{1.12}$$

---

## 6. Perhitungan Akhir Function Point (FP)

$$\text{Final Function Point (FP)} = \text{UFP} \times \text{VAF}$$

$$\text{FP} = 139 \times 1.12 = \mathbf{155.68 \text{ FP}}$$

Dibulatkan menjadi **156 Function Points**.

### Interpretasi Hasil Estimasi
Dengan ukuran fungsional sebesar **156 FP**, **Smart Classroom Digital Twin System** berada pada kategori aplikasi web berskala **kecil hingga menengah** (*small-to-medium web enterprise application*). Ukuran ini proporsional dengan rencana pembagian implementasi ke dalam tiga sprint pengembangan aktif (**Sprint 2**, **Sprint 3**, dan **Sprint 4**) setelah fondasi analisis diselesaikan pada **Sprint 1**.
