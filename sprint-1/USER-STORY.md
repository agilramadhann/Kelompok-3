# User Story Documentation
## Smart Classroom Digital Twin System

Dokumen ini mendefinisikan kebutuhan fungsional **Smart Classroom Digital Twin System** dari sudut pandang pengguna menggunakan format *User Story*. Seluruh *User Story* disusun berdasarkan batasan sistem yang bersifat *software-only* (menggunakan representasi digital dan data simulasi tanpa perangkat keras IoT fisik) serta menjadi acuan utama bagi perancangan diagram alir, spesifikasi API, estimasi *Function Point*, dan rancangan antarmuka UI/UX.

---

## 1. Definisi Aktor Sistem

Sistem memiliki tiga aktor utama dengan hak akses berbasis peran (*Role-Based Access Control* / RBAC):

| Aktor | Kode Peran | Deskripsi Tanggung Jawab |
| :--- | :--- | :--- |
| **Administrator** | `ADMIN` | Mengelola akun pengguna dan hak akses, mengelola master data ruang kelas, mengatur jadwal perkuliahan secara menyeluruh, serta memantau seluruh aktivitas dan analitik sistem. |
| **Dosen** | `DOSEN` | Mengakses jadwal mengajar, memantau kondisi dan ketersediaan ruang kelas melalui Digital Twin, memperbarui status simulasi sesi perkuliahan/okupansi kelas yang diampu, serta melihat informasi penggunaan ruang kelas. |
| **Pengelola Fasilitas Kampus** | `FACILITY_MANAGER` | Mengelola data fisik virtual ruang kelas, mengatur kapasitas dan kelengkapan fasilitas ruangan, memantau kondisi ruangan melalui Digital Twin, serta menganalisis laporan utilisasi ruangan kampus. |

---

## 2. Pengelompokan User Story Berdasarkan Fitur Sistem

### A. Fitur Autentikasi dan Manajemen Profil

#### US-01: Login Pengguna ke Sistem
- **User Story ID:** `US-01`
- **Actor:** Administrator, Dosen, Pengelola Fasilitas Kampus
- **User Story:**
  As a registered user,
  I want to log into the Smart Classroom Digital Twin System using my email and password,
  So that I can access system features securely according to my assigned role.
- **Priority:** High
- **Acceptance Criteria:**
  1. Sistem menyediakan halaman login dengan input *email* dan *password*.
  2. Sistem memvalidasi kredensial pengguna terhadap basis data dan menerbitkan token autentikasi (JWT).
  3. Jika kredensial valid, pengguna diarahkan ke halaman Dashboard sesuai perannya.
  4. Jika kredensial tidak valid, sistem menampilkan pesan kesalahan yang jelas tanpa membocorkan detail sensitif akun.

#### US-02: Logout dari Sistem
- **User Story ID:** `US-02`
- **Actor:** Administrator, Dosen, Pengelola Fasilitas Kampus
- **User Story:**
  As a logged-in user,
  I want to log out of my active session,
  So that my account remains protected from unauthorized access when I finish using the application.
- **Priority:** High
- **Acceptance Criteria:**
  1. Tombol logout tersedia pada panel navigasi *sidebar* dan menu profil pengguna.
  2. Saat pengguna memilih logout, sesi aktif/token di sisi klien dihapus dan sesi pada server diinvalidasi.
  3. Pengguna diarahkan kembali ke halaman login dan tidak dapat mengakses halaman terproteksi sebelum melakukan login ulang.

#### US-03: Pengelolaan Profil Pengguna
- **User Story ID:** `US-03`
- **Actor:** Administrator, Dosen, Pengelola Fasilitas Kampus
- **User Story:**
  As a logged-in user,
  I want to view and update my personal profile information and password,
  So that my account details stay accurate and secure.
- **Priority:** Low
- **Acceptance Criteria:**
  1. Pengguna dapat melihat informasi nama lengkap, email, peran (*role*), serta fakultas/unit kerja.
  2. Pengguna dapat mengubah nama tampilan, nomor kontak internal, dan kata sandi melalui validasi kata sandi lama.
  3. Sistem menyimpan perubahan ke basis data dan menampilkan notifikasi konfirmasi keberhasilan.

---

### B. Fitur Manajemen Ruang Kelas (*Classroom Management*)

#### US-04: Penambahan Data Ruang Kelas Baru
- **User Story ID:** `US-04`
- **Actor:** Administrator, Pengelola Fasilitas Kampus
- **User Story:**
  As an Administrator or Campus Facility Manager,
  I want to add new classroom records including room code, building, floor, and virtual facility attributes,
  So that every physical classroom has an accurate digital representation in the system.
- **Priority:** High
- **Acceptance Criteria:**
  1. Formulir penambahan ruang kelas mencakup kode ruang (unik), nama ruang, gedung, lantai, kapasitas maksimal, tipe ruang, dan daftar fasilitas (proyektor, AC, papan tulis, tata letak kursi).
  2. Sistem menolak penyimpanan apabila kode ruang sudah terdaftar atau kolom wajib kosong.
  3. Ruang kelas yang berhasil ditambahkan langsung muncul pada daftar ruang kelas dan *Digital Twin Overview*.

#### US-05: Pengaturan Kapasitas dan Fasilitas Ruang Kelas
- **User Story ID:** `US-05`
- **Actor:** Pengelola Fasilitas Kampus, Administrator
- **User Story:**
  As a Campus Facility Manager,
  I want to configure and update classroom seating capacity and facility attributes,
  So that room allocation and occupancy percentage calculations reflect current campus standards.
- **Priority:** High
- **Acceptance Criteria:**
  1. Pengguna dapat mengubah kapasitas maksimal kursi (bilangan bulat positif) dan status ketersediaan fasilitas virtual pada ruang kelas tertentu.
  2. Perubahan kapasitas secara otomatis memperbarui batas perhitungan rasio okupansi pada *Digital Twin Overview* dan modul analitik.
  3. Sistem mencatat waktu pembaruan terakhir data ruangan.

#### US-06: Pembaruan dan Penonaktifan Data Ruang Kelas
- **User Story ID:** `US-06`
- **Actor:** Administrator, Pengelola Fasilitas Kampus
- **User Story:**
  As an Administrator or Campus Facility Manager,
  I want to edit classroom details or mark a classroom as under maintenance/inactive,
  So that unavailable rooms are not scheduled for lectures.
- **Priority:** Medium
- **Acceptance Criteria:**
  1. Pengguna dapat memperbarui informasi ruang kelas maupun mengubah status operasional ruangan (*Available*, *Maintenance*, *Inactive*).
  2. Ruang kelas berstatus *Maintenance* atau *Inactive* tidak dapat dipilih saat pembuatan jadwal perkuliahan baru.
  3. Penghapusan ruang kelas hanya diizinkan atau dialihkan ke *soft-delete* apabila tidak memiliki jadwal aktif yang sedang berjalan.

#### US-07: Pencarian dan Penyaringan Daftar Ruang Kelas
- **User Story ID:** `US-07`
- **Actor:** Administrator, Dosen, Pengelola Fasilitas Kampus
- **User Story:**
  As a system user,
  I want to search and filter classrooms by building, floor, minimum capacity, or operational status,
  So that I can quickly find rooms that meet specific academic requirements.
- **Priority:** Medium
- **Acceptance Criteria:**
  1. Halaman manajemen ruang kelas menyediakan kolom pencarian berdasarkan kode/nama ruang serta filter gedung, kapasitas minimum, dan status ruangan.
  2. Daftar ruang kelas diperbarui secara dinamis sesuai kriteria filter yang dipilih.
  3. Informasi kapasitas, gedung, fasilitas utama, dan status terkini ditampilkan secara ringkas pada tabel atau kartu ruangan.

---

### C. Fitur Manajemen Jadwal Perkuliahan (*Schedule Management*)

#### US-08: Pembuatan Jadwal Perkuliahan dengan Validasi Bentrok
- **User Story ID:** `US-08`
- **Actor:** Administrator
- **User Story:**
  As an Administrator,
  I want to create lecture schedules by assigning a course, lecturer, classroom, day, and time slot,
  So that classroom usage is well-organized without overlapping allocations.
- **Priority:** High
- **Acceptance Criteria:**
  1. Formulir jadwal mencakup mata kuliah, kode kelas, dosen pengampu, ruang kelas, hari/tanggal, jam mulai, jam selesai, dan perkiraan jumlah mahasiswa.
  2. Sistem secara otomatis memvalidasi agar tidak terjadi bentrok waktu pada ruang kelas yang sama maupun dosen yang sama.
  3. Sistem memberikan peringatan apabila perkiraan jumlah mahasiswa melebihi kapasitas ruang kelas yang dipilih.

#### US-09: Perubahan dan Pembatalan Jadwal Perkuliahan
- **User Story ID:** `US-09`
- **Actor:** Administrator
- **User Story:**
  As an Administrator,
  I want to modify or delete existing lecture schedules,
  So that schedule adjustments during the academic semester can be managed accurately.
- **Priority:** Medium
- **Acceptance Criteria:**
  1. Administrator dapat mengubah ruang kelas, waktu pelaksanaan, atau dosen pengampu pada jadwal yang ada dengan tetap menjalankan validasi bentrok.
  2. Administrator dapat menghapus atau membatalkan jadwal setelah konfirmasi dialog sistem.
  3. Perubahan jadwal langsung terefleksi pada tampilan jadwal dan *Digital Twin Overview*.

#### US-10: Melihat Jadwal Perkuliahan Ruang Kelas
- **User Story ID:** `US-10`
- **Actor:** Dosen, Administrator, Pengelola Fasilitas Kampus
- **User Story:**
  As a Lecturer,
  I want to view my teaching schedules and check classroom availability across time slots,
  So that I know when and where my lectures take place and can locate alternative rooms if needed.
- **Priority:** High
- **Acceptance Criteria:**
  1. Pengguna dapat melihat jadwal perkuliahan dalam tampilan tabel terstruktur berdasarkan hari, ruang kelas, atau nama dosen.
  2. Dosen dapat memfilter jadwal khusus untuk kelas yang diampunya sendiri.
  3. Setiap item jadwal menampilkan kode ruang, mata kuliah, rentang waktu, dan status pelaksanaan sesi.

---

### D. Fitur Pemantauan Okupansi dan Simulasi Kondisi Ruang Kelas

#### US-11: Pembaruan Status Okupansi dan Simulasi Kondisi Ruangan
- **User Story ID:** `US-11`
- **Actor:** Dosen, Administrator, Pengelola Fasilitas Kampus
- **User Story:**
  As a Lecturer or Facility Manager,
  I want to update simulated classroom occupancy counts and virtual room condition states (such as active class status, simulated temperature, lighting, and projector state),
  So that the Digital Twin accurately reflects ongoing classroom activity without requiring physical IoT hardware.
- **Priority:** High
- **Acceptance Criteria:**
  1. Pengguna berwenang dapat memasukkan jumlah peserta simulasi (0 hingga kapasitas maksimal ruangan), status ruangan (*Vacant*, *Occupied*, *Reserved*), serta status perangkat virtual (AC, Lampu, Proyektor).
  2. Sistem memvalidasi bahwa jumlah okupansi simulasi tidak bernilai negatif dan menghitung persentase keterisian ruangan secara otomatis.
  3. Setiap pembaruan tercatat ke dalam log riwayat okupansi dengan stempel waktu (*timestamp*) dan identitas pengguna pengubah.

#### US-12: Melihat Informasi Penggunaan dan Riwayat Okupansi Ruangan
- **User Story ID:** `US-12`
- **Actor:** Pengelola Fasilitas Kampus, Administrator, Dosen
- **User Story:**
  As a Campus Facility Manager,
  I want to view current occupancy status and historical usage records of each classroom,
  So that I can evaluate space utilization efficiency across the campus.
- **Priority:** Medium
- **Acceptance Criteria:**
  1. Sistem menampilkan jumlah penghuni simulasi dibandingkan kapasitas maksimal serta persentase okupansi untuk tiap ruang kelas.
  2. Pengguna dapat melihat log aktivitas penggunaan ruangan berdasarkan rentang tanggal atau ruang kelas tertentu.
  3. Data penggunaan disajikan secara konsisten antara tabel okupansi dan halaman analitik.

---

### E. Fitur Digital Twin Overview dan Analitik Perkuliahan

#### US-13: Visualisasi Digital Twin Ruang Kelas
- **User Story ID:** `US-13`
- **Actor:** Administrator, Dosen, Pengelola Fasilitas Kampus
- **User Story:**
  As a campus stakeholder,
  I want to view an interactive Digital Twin overview of classrooms displaying real-time simulated room status, capacity utilization, active lecture info, and virtual facility states,
  So that I can monitor classroom conditions intuitively in one unified visual interface.
- **Priority:** High
- **Acceptance Criteria:**
  1. Halaman *Digital Twin Overview* menampilkan peta/grid representasi digital seluruh ruang kelas yang dikelompokkan berdasarkan gedung dan lantai.
  2. Setiap representasi ruang kelas menggunakan kode warna status (*Hijau* = Tersedia/Vacant, *Biru/Oranye* = Digunakan/Occupied, *Abu-abu* = Perawatan/Maintenance).
  3. Memilih satu ruang kelas membuka panel inspeksi detail Digital Twin yang memuat tata letak kapasitas visual, jadwal aktif saat ini, persentase okupansi simulasi, serta indikator kondisi virtual (suhu simulasi, pencahayaan, proyektor, AC).

#### US-14: Dashboard Ringkasan dan Analitik Utilisasi Ruang Kelas
- **User Story ID:** `US-14`
- **Actor:** Pengelola Fasilitas Kampus, Administrator
- **User Story:**
  As a Campus Facility Manager or Administrator,
  I want to view analytical charts on classroom utilization rates, peak lecture hours, and capacity efficiency on the dashboard,
  So that I can make data-driven decisions regarding classroom allocation and facility planning.
- **Priority:** Medium
- **Acceptance Criteria:**
  1. Halaman Dashboard menampilkan kartu KPI ringkasan: Total Ruang Kelas, Ruang Aktif Terpakai, Rata-rata Tingkat Okupansi (%), dan Total Jadwal Hari Ini.
  2. Sistem menyajikan grafik batang/garis untuk tren utilisasi ruang kelas per gedung serta distribusi jam sibuk perkuliahan (*peak hours*).
  3. Pengguna dapat menyaring grafik analitik berdasarkan gedung atau rentang waktu mingguan/bulanan.

---

## 3. Matriks Keterkaitan Aktor dan User Story

| Fitur Sistem | Administrator | Dosen | Pengelola Fasilitas Kampus |
| :--- | :---: | :---: | :---: |
| Autentikasi & Profil (`US-01` – `US-03`) | ✓ | ✓ | ✓ |
| Manajemen Ruang Kelas (`US-04`, `US-06`, `US-07`) | ✓ | Baca/Filter (`US-07`) | ✓ |
| Pengaturan Kapasitas Ruangan (`US-05`) | ✓ | – | ✓ |
| Manajemen Jadwal (`US-08`, `US-09`, `US-10`) | ✓ | Lihat Jadwal (`US-10`) | Lihat Jadwal (`US-10`) |
| Simulasi Okupansi & Kondisi (`US-11`, `US-12`) | ✓ | ✓ (Kelas Diampu) | ✓ |
| Digital Twin Overview (`US-13`) | ✓ | ✓ | ✓ |
| Dashboard Analitik Utilisasi (`US-14`) | ✓ | Ringkasan Dasar | ✓ |
