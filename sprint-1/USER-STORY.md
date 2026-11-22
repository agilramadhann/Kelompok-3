
# USER STORY
## Smart Classroom Digital Twin System

### 1. Pendahuluan

User Story merupakan deskripsi kebutuhan sistem berdasarkan sudut pandang pengguna. Dokumen ini digunakan untuk mengidentifikasi fitur yang perlu dikembangkan pada Smart Classroom Digital Twin System.

Sistem ini menggunakan pendekatan **Simulation-Based Digital Twin**, yaitu representasi ruang kelas virtual berbasis 3D yang diperbarui menggunakan data hasil simulasi perangkat lunak.

### 2. Identifikasi Aktor

| Aktor | Deskripsi |
|---|---|
| Administrator | Pengguna yang mengelola sistem dan konfigurasi simulasi. |
| Guru | Pengguna yang melihat kondisi ruang kelas virtual dan mengontrol perangkat virtual. |
| Manajemen Sekolah | Pengguna yang melihat laporan dan hasil simulasi penggunaan energi serta kondisi ruang kelas. |

### 3. Daftar User Story

#### A. Administrator

| ID | User Story | Prioritas |
|---|---|---|
| US-01 | Sebagai administrator, saya ingin masuk ke dalam sistem agar dapat mengakses fitur pengelolaan. | High |
| US-02 | Sebagai administrator, saya ingin mengelola data pengguna agar akses sistem dapat diatur. | Medium |
| US-03 | Sebagai administrator, saya ingin mengatur parameter simulasi agar kondisi ruang kelas virtual dapat disesuaikan. | High |
| US-04 | Sebagai administrator, saya ingin mengatur skenario simulasi agar sistem dapat menampilkan berbagai kondisi ruang kelas. | High |

#### B. Guru

| ID | User Story | Prioritas |
|---|---|---|
| US-05 | Sebagai guru, saya ingin melihat visualisasi ruang kelas dalam bentuk 3D agar dapat memahami kondisi ruang kelas virtual. | High |
| US-06 | Sebagai guru, saya ingin melihat informasi suhu dan kelembapan hasil simulasi agar dapat mengetahui kondisi lingkungan ruang kelas virtual. | High |
| US-07 | Sebagai guru, saya ingin melihat intensitas cahaya hasil simulasi agar dapat mengetahui tingkat pencahayaan ruang kelas virtual. | Medium |
| US-08 | Sebagai guru, saya ingin mengatur jumlah penghuni melalui simulasi agar dapat melihat perubahan kondisi ruang kelas. | High |
| US-09 | Sebagai guru, saya ingin mengontrol perangkat virtual seperti lampu dan kipas agar dapat melihat perubahan status perangkat pada ruang kelas 3D. | High |
| US-10 | Sebagai guru, saya ingin melihat perubahan visualisasi ruang kelas berdasarkan data simulasi agar dapat memahami hubungan antara parameter lingkungan dan perangkat virtual. | High |

#### C. Manajemen Sekolah

| ID | User Story | Prioritas |
|---|---|---|
| US-11 | Sebagai manajemen sekolah, saya ingin melihat estimasi konsumsi energi hasil simulasi agar dapat memahami penggunaan energi pada ruang kelas virtual. | High |
| US-12 | Sebagai manajemen sekolah, saya ingin melihat grafik perubahan parameter simulasi agar dapat menganalisis kondisi ruang kelas. | Medium |
| US-13 | Sebagai manajemen sekolah, saya ingin melihat riwayat simulasi agar dapat membandingkan beberapa skenario kondisi ruang kelas. | Medium |
| US-14 | Sebagai manajemen sekolah, saya ingin melihat ringkasan hasil simulasi agar dapat mengevaluasi penggunaan perangkat virtual dan energi. | Medium |

### 4. Acceptance Criteria

Acceptance Criteria merupakan kriteria yang harus dipenuhi agar suatu User Story dapat dinyatakan selesai.

| ID | Acceptance Criteria |
|---|---|
| US-01 | Sistem menyediakan halaman login dan memvalidasi data akun pengguna. |
| US-02 | Administrator dapat melihat, menambahkan, mengubah, dan menghapus data pengguna. |
| US-03 | Administrator dapat mengubah parameter simulasi yang tersedia. |
| US-04 | Administrator dapat memilih atau menjalankan skenario simulasi. |
| US-05 | Sistem menampilkan model ruang kelas dalam bentuk 3D. |
| US-06 | Sistem menampilkan nilai suhu dan kelembapan dari simulasi. |
| US-07 | Sistem menampilkan nilai intensitas cahaya dari simulasi. |
| US-08 | Pengguna dapat mengubah jumlah penghuni pada simulasi. |
| US-09 | Pengguna dapat mengaktifkan atau menonaktifkan perangkat virtual. |
| US-10 | Perubahan data simulasi ditampilkan pada visualisasi ruang kelas 3D. |
| US-11 | Sistem menampilkan estimasi konsumsi energi berdasarkan simulasi. |
| US-12 | Sistem menampilkan grafik parameter simulasi. |
| US-13 | Sistem menyimpan dan menampilkan riwayat simulasi. |
| US-14 | Sistem menampilkan ringkasan hasil simulasi dalam dashboard. |

### 5. Batasan User Story

1. Seluruh data suhu, kelembapan, intensitas cahaya, jumlah penghuni, dan konsumsi energi dihasilkan melalui simulasi perangkat lunak.
2. Sistem tidak menggunakan sensor atau perangkat IoT fisik.
3. Kontrol lampu dan kipas hanya mengubah status perangkat virtual.
4. Estimasi konsumsi energi bukan hasil pengukuran langsung dari perangkat listrik.
5. Visualisasi 3D merepresentasikan ruang kelas virtual, bukan kondisi ruang kelas fisik secara langsung.

### 6. Kesimpulan

User Story ini menjadi dasar dalam proses perancangan dan pengembangan Smart Classroom Digital Twin System. Setiap kebutuhan pengguna akan digunakan sebagai acuan untuk menentukan fitur, alur sistem, serta rancangan antarmuka aplikasi.

