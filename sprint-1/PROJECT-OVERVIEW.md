
# GAMBARAN UMUM PROYEK
## Smart Classroom Digital Twin System

### 1. Latar Belakang Proyek

Pengelolaan ruang kelas merupakan salah satu aspek penting dalam menciptakan lingkungan pembelajaran yang efektif dan nyaman. Namun, pengelolaan ruang kelas secara konvensional masih menghadapi beberapa permasalahan, seperti keterbatasan pemantauan kondisi lingkungan kelas, penggunaan energi listrik yang kurang efisien, serta belum tersedianya sistem pengelolaan ruang kelas yang terintegrasi.

Untuk mengatasi permasalahan tersebut, dikembangkan **Smart Classroom Digital Twin System**, yaitu sebuah sistem pengelolaan ruang kelas pintar berbasis Internet of Things (IoT) yang mengintegrasikan kondisi ruang kelas fisik dengan representasi virtual dalam bentuk 3D.

Sistem ini memanfaatkan sensor IoT untuk mengumpulkan data kondisi lingkungan kelas dan menyinkronkan informasi tersebut dengan model ruang kelas virtual. Melalui integrasi ini, pengguna dapat memantau kondisi ruang kelas, menganalisis penggunaan energi, melakukan simulasi perubahan kondisi lingkungan, serta mengendalikan perangkat kelas melalui satu dashboard terpusat.

### 2. Identifikasi Masalah

Beberapa permasalahan yang ditemukan dalam pengelolaan ruang kelas secara konvensional adalah:

1. Terbatasnya kemampuan untuk memantau kondisi lingkungan ruang kelas secara real-time.
2. Penggunaan energi listrik yang kurang efisien karena lampu dan kipas tetap menyala ketika ruang kelas tidak digunakan.
3. Belum tersedianya sistem pemantauan fasilitas kelas secara terpusat.
4. Kesulitan dalam mengetahui perubahan kondisi lingkungan ruang kelas.
5. Belum adanya representasi virtual interaktif yang menggambarkan kondisi ruang kelas fisik.

### 3. Tujuan Proyek

Tujuan pengembangan sistem ini adalah:

1. Mengembangkan representasi virtual ruang kelas dalam bentuk 3D.
2. Memantau kondisi lingkungan ruang kelas secara real-time.
3. Mendeteksi keberadaan orang di dalam ruang kelas menggunakan sensor IoT.
4. Memantau penggunaan energi listrik.
5. Menyediakan fitur pengendalian perangkat kelas dari jarak jauh.
6. Melakukan simulasi perubahan kondisi lingkungan ruang kelas melalui lingkungan virtual.
7. Mendukung pengelolaan ruang kelas yang lebih efisien melalui analisis data.

### 4. Ruang Lingkup Proyek

Sistem ini mencakup beberapa fungsi utama sebagai berikut:

**A. Pemantauan Kondisi Lingkungan**
- Pemantauan suhu ruangan.
- Pemantauan kelembapan udara.
- Pemantauan intensitas cahaya.
- Deteksi keberadaan orang di dalam kelas.

**B. Visualisasi Digital Twin**
- Representasi ruang kelas virtual dalam bentuk 3D.
- Sinkronisasi data sensor IoT dengan model virtual secara real-time.
- Visualisasi kondisi perangkat kelas.
- Representasi perubahan kondisi lingkungan ruang kelas.

**C. Pengelolaan Perangkat Pintar**
- Pengendalian lampu dari jarak jauh.
- Pengendalian kipas angin.
- Pemantauan status perangkat yang terhubung.

**D. Pemantauan Energi**
- Pemantauan penggunaan energi listrik.
- Penyajian statistik konsumsi energi.
- Penyimpanan data historis penggunaan energi.

**E. Sistem Simulasi**
- Simulasi perubahan suhu ruang kelas.
- Simulasi kondisi keberadaan orang di dalam kelas.
- Simulasi perubahan status perangkat kelas.

### 5. Pengguna Sistem

Sistem ini dirancang untuk digunakan oleh beberapa jenis pengguna berikut:

| Pengguna | Tanggung Jawab |
|---|---|
| Administrator | Mengelola sistem, pengguna, dan perangkat ruang kelas. |
| Guru | Memantau kondisi ruang kelas dan mengendalikan perangkat yang tersedia. |
| Manajemen Sekolah | Melihat kondisi ruang kelas dan laporan penggunaan energi. |

### 6. Gambaran Teknologi yang Digunakan

Teknologi yang direncanakan dalam pengembangan sistem ini meliputi:

| Komponen | Teknologi |
|---|---|
| Frontend | React.js |
| Visualisasi 3D | Three.js |
| Backend | Node.js dan Express.js |
| Database | MySQL / PostgreSQL |
| Mikrokontroler | ESP32 |
| Protokol Komunikasi | MQTT |
| Sensor Suhu dan Kelembapan | DHT22 |
| Sensor Deteksi Gerakan | PIR Sensor |
| Sensor Intensitas Cahaya | LDR Sensor |
| Pemantauan Energi | PZEM-004T |

### 7. Gambaran Arsitektur Sistem

Sistem ini terdiri dari beberapa komponen utama:

1. **Ruang Kelas Fisik:** Ruang kelas sebenarnya yang dilengkapi dengan sensor dan perangkat elektronik yang terhubung.
2. **Lapisan IoT:** Mengumpulkan data kondisi lingkungan melalui ESP32.
3. **Lapisan Komunikasi:** Mengirimkan data sensor menggunakan protokol MQTT.
4. **Lapisan Backend:** Memproses data sensor dan permintaan pengendalian perangkat.
5. **Lapisan Database:** Menyimpan data sensor, informasi perangkat, dan riwayat pemantauan.
6. **Lapisan Digital Twin:** Merepresentasikan kondisi ruang kelas fisik ke dalam lingkungan virtual 3D.
7. **Antarmuka Pengguna:** Menyediakan dashboard terpusat untuk memantau dan mengendalikan sistem.

### 8. Hasil yang Diharapkan

Hasil yang diharapkan dari pengembangan proyek ini meliputi:

- Dashboard pengelolaan ruang kelas berbasis web.
- Model ruang kelas virtual 3D yang tersinkronisasi dengan kondisi ruang kelas fisik.
- Fitur pemantauan kondisi lingkungan secara real-time.
- Pengendalian perangkat kelas secara jarak jauh.
- Pemantauan dan pelaporan konsumsi energi listrik.
- Lingkungan simulasi untuk menganalisis perubahan kondisi ruang kelas.

### 9. Batasan Proyek

Pengembangan awal sistem difokuskan pada lingkungan ruang kelas dengan cakupan terbatas.

Adapun batasan dalam pengembangan sistem ini adalah:

1. Implementasi awal difokuskan pada satu ruang kelas fisik.
2. Kemampuan pemantauan bergantung pada ketersediaan dan tingkat akurasi sensor IoT.
3. Pengendalian perangkat hanya berlaku untuk perangkat keras yang telah terhubung dan didukung sistem.
4. Model virtual 3D hanya merepresentasikan objek dan kondisi ruang kelas yang telah ditentukan.
5. Fitur analisis prediktif dan kecerdasan buatan tingkat lanjut direncanakan sebagai pengembangan berikutnya.

### 10. Kesimpulan Proyek

Smart Classroom Digital Twin System dikembangkan dengan tujuan mengubah pengelolaan ruang kelas konvensional menjadi lingkungan yang lebih cerdas dan terintegrasi.

Dengan menggabungkan teknologi Internet of Things (IoT), Digital Twin, dan aplikasi berbasis web, sistem ini diharapkan mampu menyediakan pemantauan kondisi ruang kelas secara real-time, simulasi lingkungan virtual, serta mendukung pengelolaan ruang kelas yang lebih efektif dan efisien.
