# System Flowchart Documentation
## Smart Classroom Digital Twin System

Dokumen ini menyajikan diagram alir (*Flowchart*) yang menggambarkan logika eksekusi utama **Smart Classroom Digital Twin System** dari sisi alur interaksi pengguna hingga pemrosesan di sisi sistem, serta diagram alir terpisah untuk proses pengelolaan ruang kelas.

---

## 1. Flowchart Alur Utama Sistem

Flowchart berikut memodelkan alur lengkap mulai dari pengguna membuka aplikasi, proses autentikasi, navigasi menu Dashboard, pemrosesan permintaan, penyajian informasi ruang kelas, hingga mengakhiri sesi (*logout*).

```mermaid
flowchart TD
    Start(["Mulai: User Membuka Aplikasi"]) --> ShowLogin["Sistem Menampilkan Halaman Login"]
    ShowLogin --> InputCred["User Memasukkan Email dan Password"]
    InputCred --> ValidateAuth{"Validasi Autentikasi\ndi Backend?"}

    ValidateAuth -- "Tidak Valid" --> ShowAuthErr["Sistem Menampilkan Pesan\nAutentikasi Gagal"]
    ShowAuthErr --> ShowLogin

    ValidateAuth -- "Valid" --> GenToken["Sistem Menerbitkan JWT\n& Memuat Hak Akses (RBAC)"]
    GenToken --> ShowDash["Pengguna Masuk ke Dashboard\n(Ringkasan KPI & Navigasi Sidebar)"]

    ShowDash --> SelectMenu{"Pengguna Memilih Menu"}

    SelectMenu -- "1. Manajemen Ruang Kelas" --> ProcessRoom["Sistem Memproses Permintaan\nData & Pengelolaan Ruang Kelas"]
    SelectMenu -- "2. Manajemen Jadwal" --> ProcessSched["Sistem Memproses Permintaan\nJadwal & Validasi Bentrok"]
    SelectMenu -- "3. Digital Twin Overview" --> ProcessDT["Sistem Memuat State Simulasi\n& Render Visual Digital Twin"]
    SelectMenu -- "4. Analitik & Penggunaan" --> ProcessAnalytics["Sistem Mengagregasi Data\nOkupansi & Utilisasi Ruangan"]
    SelectMenu -- "5. Profil Pengguna" --> ProcessProfile["Sistem Memproses Informasi\nProfil Akun Pengguna"]
    SelectMenu -- "6. Logout" --> EndSession["Pengguna Mengakhiri Sesi\n(Invalidasi Token & Clear State)"]

    ProcessRoom --> DisplayInfo["Sistem Menampilkan Informasi\nRuang Kelas / Hasil Pemrosesan"]
    ProcessSched --> DisplayInfo
    ProcessDT --> DisplayInfo
    ProcessAnalytics --> DisplayInfo
    ProcessProfile --> DisplayInfo

    DisplayInfo --> ContinueCheck{"Lanjutkan Aktivitas\nLain?"}
    ContinueCheck -- "Ya" --> ShowDash
    ContinueCheck -- "Tidak" --> EndSession

    EndSession --> RedirectLogin["Sistem Mengarahkan Kembali\nke Halaman Login"]
    RedirectLogin --> Stop(["Selesai"])
```

### Penjelasan Alur Utama Sistem
1. **Inisialisasi & Login:** Ketika pengguna mengakses URL aplikasi web, sistem memeriksa token sesi. Jika belum terautentikasi, sistem menampilkan halaman Login.
2. **Validasi Autentikasi:** Sistem mencocokkan kredensial pada layanan autentikasi. Jika gagal, pengguna menerima pesan kesalahan dan tetap berada di halaman Login. Jika berhasil, pengguna diarahkan ke **Dashboard**.
3. **Pemilihan Menu:** Melalui *sidebar navigation* pada Dashboard, pengguna memilih fitur sesuai hak aksesnya (Manajemen Ruang Kelas, Manajemen Jadwal, *Digital Twin Overview*, Analitik Penggunaan, atau Profil Pengguna).
4. **Pemrosesan & Tampilan Informasi:** Sistem menjalankan logika bisnis (pengambilan data, validasi, simulasi, atau agregasi analitik) melalui REST API dan menampilkan informasi ruang kelas atau hasil transaksi pada antarmuka pengguna.
5. **Pengakhiran Sesi:** Ketika pengguna memilih *Logout*, sistem membersihkan token autentikasi, mengakhiri sesi, dan mengembalikan tampilan ke halaman Login.

---

## 2. Flowchart Proses Pengelolaan Ruang Kelas (*Classroom Management*)

Flowchart terpisah ini menjelaskan secara rinci proses bisnis pada modul **Pengelolaan Ruang Kelas** yang digunakan oleh Administrator dan Pengelola Fasilitas Kampus (`US-04` sampai `US-07`).

```mermaid
flowchart TD
    StartRM(["Mulai: Masuk Menu Classroom Management"]) --> LoadRooms["Sistem Mengambil & Menampilkan\nDaftar Ruang Kelas"]
    LoadRooms --> ActionChoice{"Pilih Aksi\nPengelolaan"}

    ActionChoice -- "Cari / Filter" --> ApplyFilter["Input Kata Kunci / Filter Gedung,\nKapasitas & Status"]
    ApplyFilter --> QueryFiltered["Sistem Menyaring Data dari Database"]
    QueryFiltered --> LoadRooms

    ActionChoice -- "Tambah Ruang Baru" --> FormAdd["Input Kode Ruang, Nama, Gedung,\nLantai, Kapasitas & Fasilitas"]
    FormAdd --> CheckUnique{"Kode Ruang Unik &\nInput Valid?"}
    CheckUnique -- "Tidak" --> ErrAdd["Tampilkan Pesan Validasi Error"]
    ErrAdd --> FormAdd
    CheckUnique -- "Ya" --> InsertDB["Simpan Ruang Kelas Baru &\nBuat State Awal Digital Twin"]
    InsertDB --> NotifyOK["Tampilkan Notifikasi Sukses"]
    NotifyOK --> LoadRooms

    ActionChoice -- "Ubah Data / Atur Kapasitas" --> SelectEdit["Pilih Ruang Kelas & Edit Atribut\natau Kapasitas Ruangan"]
    SelectEdit --> CheckCap{"Kapasitas > 0 &\nFormat Valid?"}
    CheckCap -- "Tidak" --> ErrEdit["Tampilkan Pesan Error Kapasitas"]
    ErrEdit --> SelectEdit
    CheckCap -- "Ya" --> UpdateDB["Perbarui Data Ruang & Hitung Ulang\nRasio Okupansi Digital Twin"]
    UpdateDB --> NotifyOK

    ActionChoice -- "Hapus / Nonaktifkan" --> CheckActiveSched{"Ada Jadwal Aktif\npada Ruangan Ini?"}
    CheckActiveSched -- "Ya" --> WarnSched["Tolak Hapus Permanen;\nSarankan Ubah Status ke Maintenance"]
    WarnSched --> LoadRooms
    CheckActiveSched -- "Tidak" --> SoftDelete["Nonaktifkan / Hapus Data Ruang\ndari Daftar Aktif"]
    SoftDelete --> NotifyOK

    ActionChoice -- "Kembali ke Dashboard" --> EndRM(["Selesai"])
```

### Penjelasan Alur Pengelolaan Ruang Kelas
1. **Pemuatan & Penyaringan Data:** Saat modul dibuka, sistem menampilkan daftar ruang kelas lengkap dengan informasi gedung, kapasitas, dan status operasional. Pengguna dapat melakukan pencarian atau penyaringan dinamis.
2. **Penambahan Ruang Kelas:** Sistem memvalidasi bahwa kode ruang belum digunakan dan seluruh kolom wajib terisi. Setelah tersimpan, sistem secara otomatis membuat representasi node *Digital Twin* awal dengan status standar *Vacant*.
3. **Pengubahan Data & Pengaturan Kapasitas:** Sistem memverifikasi agar nilai kapasitas bernilai positif sebelum memperbarui basis data dan menyesuaikan perhitungan persentase keterisian ruangan.
4. **Penghapusan/Penonaktifan Ruang Kelas:** Sistem memeriksa keterkaitan dengan jadwal perkuliahan aktif. Jika masih terdapat jadwal aktif, penghapusan diblokir untuk menjaga integritas data dan pengguna diarahkan mengubah status ruangan menjadi *Maintenance*.
