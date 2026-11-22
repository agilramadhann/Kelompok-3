# REST API Specification (Initial Design)
## Smart Classroom Digital Twin System

> **Status Dokumen:** Rancangan Awal (*Sprint 1 – System Analysis and Design*). Seluruh endpoint di bawah ini merupakan spesifikasi kontrak API untuk diimplementasikan pada **Sprint 2 dan Sprint 3**, dan **belum berstatus berjalan/terimplementasi** pada tahap Sprint 1.

---

## 1. Informasi Umum & Konvensi API

- **Base URL (Planned):** `/api/v1`
- **Protokol & Format Data:** HTTPS / RESTful JSON (`Content-Type: application/json`)
- **Mekanisme Autentikasi:** Bearer Token menggunakan *JSON Web Token* (`Authorization: Bearer <access_token>`)
- **Konvensi Format Respons Standar:**
  - **Sukses:**
    ```json
    {
      "success": true,
      "message": "Deskripsi singkat hasil operasi",
      "data": {}
    }
    ```
  - **Gagal / Error:**
    ```json
    {
      "success": false,
      "message": "Pesan kesalahan utama",
      "errors": [
        {
          "field": "nama_field",
          "issue": "Penjelasan detail kesalahan"
        }
      ]
    }
    ```

---

## 2. Authentication API

### 2.1 Login Pengguna
- **HTTP Method:** `POST`
- **Endpoint URL:** `/api/v1/auth/login`
- **Description:** Mengautentikasi pengguna (*Administrator*, *Dosen*, atau *Pengelola Fasilitas Kampus*) dan mengembalikan token JWT beserta profil ringkas pengguna.
- **Request Parameters:** Tidak ada (*Public endpoint*).
- **Request Body:**
  ```json
  {
    "email": "admin@campus.ac.id",
    "password": "PasswordKuat123!"
  }
  ```
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Login successful",
    "data": {
      "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
      "tokenType": "Bearer",
      "expiresIn": 3600,
      "user": {
        "id": "usr-001",
        "fullName": "Dr. Budi Santoso",
        "email": "admin@campus.ac.id",
        "role": "ADMIN"
      }
    }
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Autentikasi berhasil.
  - `400 Bad Request` – Format email atau password tidak valid.
  - `401 Unauthorized` – Email atau password salah.

### 2.2 Logout Pengguna
- **HTTP Method:** `POST`
- **Endpoint URL:** `/api/v1/auth/logout`
- **Description:** Mengakhiri sesi pengguna dan menandai token akses saat ini sebagai tidak berlaku (*revoked*).
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
- **Request Body:** Tidak diperlukan.
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Logout successful",
    "data": null
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Logout berhasil.
  - `401 Unauthorized` – Token tidak ditemukan atau sudah kedaluwarsa.

### 2.3 Ambil Profil Pengguna Aktif
- **HTTP Method:** `GET`
- **Endpoint URL:** `/api/v1/auth/me`
- **Description:** Mengambil detail profil pengguna yang sedang terautentikasi.
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
- **Request Body:** Tidak diperlukan.
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "User profile retrieved",
    "data": {
      "id": "usr-002",
      "fullName": "Prof. Siti Aminah, M.Kom.",
      "email": "siti.aminah@campus.ac.id",
      "role": "DOSEN",
      "department": "Teknik Informatika",
      "phone": "081234567890"
    }
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Data profil berhasil diambil.
  - `401 Unauthorized` – Token tidak valid.

### 2.4 Perbarui Profil Pengguna
- **HTTP Method:** `PUT`
- **Endpoint URL:** `/api/v1/auth/profile`
- **Description:** Memperbarui informasi profil pengguna yang sedang login.
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
- **Request Body:**
  ```json
  {
    "fullName": "Prof. Siti Aminah, Ph.D.",
    "department": "Teknik Informatika",
    "phone": "081298765432"
  }
  ```
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Profile updated successfully",
    "data": {
      "id": "usr-002",
      "fullName": "Prof. Siti Aminah, Ph.D.",
      "department": "Teknik Informatika",
      "phone": "081298765432"
    }
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Profil berhasil diperbarui.
  - `400 Bad Request` – Data input tidak valid.
  - `401 Unauthorized` – Token tidak valid.

---

## 3. Classroom Management API

### 3.1 Daftar Ruang Kelas
- **HTTP Method:** `GET`
- **Endpoint URL:** `/api/v1/classrooms`
- **Description:** Mengambil daftar seluruh ruang kelas dengan dukungan pencarian, filter gedung, status, serta kapasitas minimum.
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
  - Query `search` (optional, string): Kata kunci kode atau nama ruang.
  - Query `building` (optional, string): Filter nama gedung.
  - Query `status` (optional, enum: `AVAILABLE`, `MAINTENANCE`, `INACTIVE`): Filter status ruangan.
  - Query `minCapacity` (optional, integer): Filter kapasitas minimum.
  - Query `page` (optional, integer, default: `1`), `limit` (optional, integer, default: `10`).
- **Request Body:** Tidak diperlukan.
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Classrooms retrieved successfully",
    "data": {
      "items": [
        {
          "id": "cls-101",
          "code": "GD-A-301",
          "name": "Ruang Kuliah A301",
          "building": "Gedung A",
          "floor": 3,
          "capacity": 60,
          "operationalStatus": "AVAILABLE",
          "facilities": ["Projector", "AC", "Smart Whiteboard"]
        }
      ],
      "pagination": {
        "page": 1,
        "limit": 10,
        "totalItems": 24,
        "totalPages": 3
      }
    }
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Daftar ruang kelas berhasil diambil.
  - `401 Unauthorized` – Token tidak valid.

### 3.2 Detail Ruang Kelas
- **HTTP Method:** `GET`
- **Endpoint URL:** `/api/v1/classrooms/{id}`
- **Description:** Mengambil detail lengkap satu ruang kelas beserta konfigurasi fasilitasnya.
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
  - Path `id` (required, string): ID unik ruang kelas.
- **Request Body:** Tidak diperlukan.
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Classroom detail retrieved",
    "data": {
      "id": "cls-101",
      "code": "GD-A-301",
      "name": "Ruang Kuliah A301",
      "building": "Gedung A",
      "floor": 3,
      "capacity": 60,
      "roomType": "LECTURE_HALL",
      "operationalStatus": "AVAILABLE",
      "facilities": ["Projector", "AC", "Smart Whiteboard", "Audio System"],
      "updatedAt": "2026-11-22T10:00:00Z"
    }
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Detail ditemukan.
  - `401 Unauthorized` – Token tidak valid.
  - `404 Not Found` – Ruang kelas tidak ditemukan.

### 3.3 Tambah Ruang Kelas Baru
- **HTTP Method:** `POST`
- **Endpoint URL:** `/api/v1/classrooms`
- **Description:** Menambahkan ruang kelas baru dan menginisialisasi entitas Digital Twin bawaan (khusus `ADMIN` dan `FACILITY_MANAGER`).
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
- **Request Body:**
  ```json
  {
    "code": "GD-B-204",
    "name": "Ruang Kuliah B204",
    "building": "Gedung B",
    "floor": 2,
    "capacity": 45,
    "roomType": "STANDARD_CLASSROOM",
    "facilities": ["Projector", "AC", "Whiteboard"]
  }
  ```
- **Response Example (`201 Created`):**
  ```json
  {
    "success": true,
    "message": "Classroom created successfully",
    "data": {
      "id": "cls-102",
      "code": "GD-B-204",
      "name": "Ruang Kuliah B204",
      "building": "Gedung B",
      "floor": 2,
      "capacity": 45,
      "operationalStatus": "AVAILABLE"
    }
  }
  ```
- **HTTP Status Code:**
  - `201 Created` – Ruang kelas berhasil dibuat.
  - `400 Bad Request` – Input tidak valid.
  - `403 Forbidden` – Peran pengguna tidak memiliki izin.
  - `409 Conflict` – Kode ruang sudah terdaftar.

### 3.4 Perbarui Data Ruang Kelas
- **HTTP Method:** `PUT`
- **Endpoint URL:** `/api/v1/classrooms/{id}`
- **Description:** Memperbarui data, fasilitas, atau status operasional ruang kelas (khusus `ADMIN` dan `FACILITY_MANAGER`).
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
  - Path `id` (required, string): ID ruang kelas.
- **Request Body:**
  ```json
  {
    "name": "Ruang Kuliah B204 (Renovasi)",
    "building": "Gedung B",
    "floor": 2,
    "capacity": 50,
    "operationalStatus": "MAINTENANCE",
    "facilities": ["Projector", "AC", "Smart Whiteboard"]
  }
  ```
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Classroom updated successfully",
    "data": {
      "id": "cls-102",
      "code": "GD-B-204",
      "name": "Ruang Kuliah B204 (Renovasi)",
      "capacity": 50,
      "operationalStatus": "MAINTENANCE"
    }
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Pembaruan berhasil.
  - `400 Bad Request` – Data tidak valid.
  - `403 Forbidden` – Tidak memiliki hak akses.
  - `404 Not Found` – Ruang kelas tidak ditemukan.

### 3.5 Pengaturan Kapasitas Ruang Kelas
- **HTTP Method:** `PATCH`
- **Endpoint URL:** `/api/v1/classrooms/{id}/capacity`
- **Description:** Memperbarui kapasitas kursi maksimal ruangan dan menghitung ulang rasio okupansi Digital Twin (khusus `ADMIN` dan `FACILITY_MANAGER`).
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
  - Path `id` (required, string): ID ruang kelas.
- **Request Body:**
  ```json
  {
    "capacity": 70
  }
  ```
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Classroom capacity updated",
    "data": {
      "id": "cls-101",
      "code": "GD-A-301",
      "previousCapacity": 60,
      "newCapacity": 70
    }
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Kapasitas diperbarui.
  - `400 Bad Request` – Kapasitas harus bernilai lebih dari 0.
  - `403 Forbidden` – Akses ditolak.
  - `404 Not Found` – Ruang kelas tidak ditemukan.

### 3.6 Hapus Ruang Kelas
- **HTTP Method:** `DELETE`
- **Endpoint URL:** `/api/v1/classrooms/{id}`
- **Description:** Menghapus atau menonaktifkan ruang kelas jika tidak memiliki jadwal aktif (khusus `ADMIN` dan `FACILITY_MANAGER`).
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
  - Path `id` (required, string): ID ruang kelas.
- **Request Body:** Tidak diperlukan.
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Classroom deleted successfully",
    "data": null
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Berhasil dihapus.
  - `403 Forbidden` – Akses ditolak.
  - `404 Not Found` – Ruang kelas tidak ditemukan.
  - `409 Conflict` – Ruang kelas masih memiliki jadwal perkuliahan aktif.

---

## 4. Schedule Management API

### 4.1 Daftar Jadwal Perkuliahan
- **HTTP Method:** `GET`
- **Endpoint URL:** `/api/v1/schedules`
- **Description:** Mengambil daftar jadwal perkuliahan berdasarkan filter hari, ruang kelas, atau dosen pengampu.
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
  - Query `classroomId` (optional, string): Filter berdasarkan ID ruang kelas.
  - Query `lecturerId` (optional, string): Filter berdasarkan ID dosen.
  - Query `dayOfWeek` (optional, enum: `MONDAY`..`SATURDAY`): Filter hari.
- **Request Body:** Tidak diperlukan.
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Schedules retrieved successfully",
    "data": [
      {
        "id": "sch-501",
        "courseCode": "IF302",
        "courseName": "Rekayasa Perangkat Lunak",
        "classroomId": "cls-101",
        "classroomCode": "GD-A-301",
        "lecturerId": "usr-002",
        "lecturerName": "Prof. Siti Aminah, M.Kom.",
        "dayOfWeek": "MONDAY",
        "startTime": "08:00",
        "endTime": "10:30",
        "expectedStudents": 48
      }
    ]
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Jadwal berhasil diambil.
  - `401 Unauthorized` – Token tidak valid.

### 4.2 Tambah Jadwal Perkuliahan
- **HTTP Method:** `POST`
- **Endpoint URL:** `/api/v1/schedules`
- **Description:** Membuat jadwal perkuliahan baru dengan validasi bentrok waktu, bentrok dosen, status ruangan, dan kapasitas (khusus `ADMIN`).
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
- **Request Body:**
  ```json
  {
    "courseCode": "IF405",
    "courseName": "Arsitektur Perangkat Lunak",
    "classroomId": "cls-101",
    "lecturerId": "usr-002",
    "dayOfWeek": "TUESDAY",
    "startTime": "13:00",
    "endTime": "15:30",
    "expectedStudents": 50
  }
  ```
- **Response Example (`201 Created`):**
  ```json
  {
    "success": true,
    "message": "Schedule created successfully",
    "data": {
      "id": "sch-502",
      "courseCode": "IF405",
      "courseName": "Arsitektur Perangkat Lunak",
      "classroomId": "cls-101",
      "dayOfWeek": "TUESDAY",
      "startTime": "13:00",
      "endTime": "15:30",
      "capacityWarning": false
    }
  }
  ```
- **HTTP Status Code:**
  - `201 Created` – Jadwal berhasil dibuat.
  - `400 Bad Request` – Rentang waktu tidak valid (`endTime <= startTime`) atau ruang berstatus `MAINTENANCE`.
  - `403 Forbidden` – Hanya Administrator yang berwenang.
  - `409 Conflict` – Terdeteksi bentrok jadwal pada ruang kelas atau dosen yang sama.

### 4.3 Perbarui Jadwal Perkuliahan
- **HTTP Method:** `PUT`
- **Endpoint URL:** `/api/v1/schedules/{id}`
- **Description:** Memperbarui jadwal perkuliahan dengan validasi bentrok otomatis (khusus `ADMIN`).
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
  - Path `id` (required, string): ID jadwal.
- **Request Body:**
  ```json
  {
    "classroomId": "cls-101",
    "lecturerId": "usr-002",
    "dayOfWeek": "WEDNESDAY",
    "startTime": "09:00",
    "endTime": "11:30",
    "expectedStudents": 52
  }
  ```
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Schedule updated successfully",
    "data": {
      "id": "sch-502",
      "dayOfWeek": "WEDNESDAY",
      "startTime": "09:00",
      "endTime": "11:30"
    }
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Jadwal diperbarui.
  - `403 Forbidden` – Akses ditolak.
  - `404 Not Found` – Jadwal tidak ditemukan.
  - `409 Conflict` – Bentrok jadwal terdeteksi.

### 4.4 Hapus Jadwal Perkuliahan
- **HTTP Method:** `DELETE`
- **Endpoint URL:** `/api/v1/schedules/{id}`
- **Description:** Menghapus jadwal perkuliahan tertentu (khusus `ADMIN`).
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
  - Path `id` (required, string): ID jadwal.
- **Request Body:** Tidak diperlukan.
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Schedule deleted successfully",
    "data": null
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Jadwal berhasil dihapus.
  - `403 Forbidden` – Akses ditolak.
  - `404 Not Found` – Jadwal tidak ditemukan.

---

## 5. Classroom Occupancy API

### 5.1 Ambil Data Okupansi Ruang Kelas
- **HTTP Method:** `GET`
- **Endpoint URL:** `/api/v1/occupancy`
- **Description:** Mengambil status okupansi simulasi terkini dan riwayat penggunaan seluruh atau spesifik ruang kelas.
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
  - Query `classroomId` (optional, string): Filter berdasarkan ID ruang kelas.
  - Query `date` (optional, format `YYYY-MM-DD`): Filter tanggal log okupansi.
- **Request Body:** Tidak diperlukan.
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Occupancy data retrieved",
    "data": [
      {
        "classroomId": "cls-101",
        "classroomCode": "GD-A-301",
        "status": "OCCUPIED",
        "simulatedOccupants": 48,
        "capacity": 60,
        "occupancyRate": 80.0,
        "recordedAt": "2026-11-22T09:15:00Z",
        "updatedBy": "Prof. Siti Aminah, M.Kom."
      }
    ]
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Data okupansi berhasil diambil.
  - `401 Unauthorized` – Token tidak valid.

### 5.2 Perbarui / Simulasikan Okupansi Ruang Kelas
- **HTTP Method:** `POST`
- **Endpoint URL:** `/api/v1/occupancy/simulate`
- **Description:** Mencatat jumlah penghuni simulasi dan status keterisian ruangan (oleh `ADMIN`, `FACILITY_MANAGER`, atau `DOSEN` pada sesi kelasnya) untuk memperbarui model Digital Twin tanpa sensor fisik.
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
- **Request Body:**
  ```json
  {
    "classroomId": "cls-101",
    "status": "OCCUPIED",
    "simulatedOccupants": 52,
    "notes": "Perkuliahan RPL Sesi Pagi (Data Simulasi)"
  }
  ```
- **Response Example (`201 Created`):**
  ```json
  {
    "success": true,
    "message": "Classroom occupancy simulation recorded",
    "data": {
      "logId": "occ-901",
      "classroomId": "cls-101",
      "status": "OCCUPIED",
      "simulatedOccupants": 52,
      "capacity": 60,
      "occupancyRate": 86.67,
      "recordedAt": "2026-11-22T09:30:00Z"
    }
  }
  ```
- **HTTP Status Code:**
  - `201 Created` – Simulasi okupansi berhasil disimpan.
  - `400 Bad Request` – Nilai okupansi negatif atau melebihi batas wajar validasi.
  - `403 Forbidden` – Pengguna tidak berwenang mengubah okupansi ruangan ini.
  - `404 Not Found` – Ruang kelas tidak ditemukan.

---

## 6. Digital Twin Data API

### 6.1 Digital Twin Overview Seluruh Kampus
- **HTTP Method:** `GET`
- **Endpoint URL:** `/api/v1/digital-twin/overview`
- **Description:** Mengambil representasi digital menyeluruh dari seluruh ruang kelas (status visual, jadwal aktif saat ini, rasio okupansi, dan kondisi virtual ruangan) untuk dirender pada halaman *Digital Twin Overview*.
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
  - Query `building` (optional, string): Filter berdasarkan gedung.
  - Query `floor` (optional, integer): Filter berdasarkan lantai.
- **Request Body:** Tidak diperlukan.
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Digital twin overview retrieved",
    "data": {
      "timestamp": "2026-11-22T09:35:00Z",
      "summary": {
        "totalRooms": 24,
        "vacant": 10,
        "occupied": 12,
        "maintenance": 2
      },
      "rooms": [
        {
          "classroomId": "cls-101",
          "code": "GD-A-301",
          "building": "Gedung A",
          "floor": 3,
          "twinStatus": "OCCUPIED",
          "occupancy": {
            "current": 52,
            "capacity": 60,
            "percentage": 86.67
          },
          "activeSchedule": {
            "courseCode": "IF302",
            "courseName": "Rekayasa Perangkat Lunak",
            "lecturer": "Prof. Siti Aminah, M.Kom.",
            "endTime": "10:30"
          },
          "virtualEnvironment": {
            "simulatedTempC": 23.5,
            "lightingState": "ON",
            "acState": "ON",
            "projectorState": "ON"
          }
        }
      ]
    }
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Data Digital Twin berhasil dimuat.
  - `401 Unauthorized` – Token tidak valid.

### 6.2 Detail Digital Twin per Ruang Kelas
- **HTTP Method:** `GET`
- **Endpoint URL:** `/api/v1/digital-twin/classrooms/{id}`
- **Description:** Mengambil model representasi digital detail untuk satu ruang kelas termasuk tata letak zona kursi simulasi, jadwal hari ini, dan parameter kondisi virtual.
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
  - Path `id` (required, string): ID ruang kelas.
- **Request Body:** Tidak diperlukan.
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Classroom digital twin detail retrieved",
    "data": {
      "classroomId": "cls-101",
      "code": "GD-A-301",
      "name": "Ruang Kuliah A301",
      "twinStatus": "OCCUPIED",
      "capacity": 60,
      "simulatedOccupants": 52,
      "layoutConfig": {
        "rows": 6,
        "columns": 10
      },
      "virtualEnvironment": {
        "simulatedTempC": 23.5,
        "lightingState": "ON",
        "acState": "ON",
        "projectorState": "ON"
      },
      "todaySchedules": [
        {
          "id": "sch-501",
          "courseName": "Rekayasa Perangkat Lunak",
          "startTime": "08:00",
          "endTime": "10:30"
        }
      ]
    }
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Detail Digital Twin ditemukan.
  - `404 Not Found` – Ruang kelas tidak ditemukan.

### 6.3 Perbarui State Kondisi Virtual Digital Twin
- **HTTP Method:** `PUT`
- **Endpoint URL:** `/api/v1/digital-twin/classrooms/{id}/state`
- **Description:** Memperbarui parameter kondisi virtual ruang kelas berbasis simulasi perangkat lunak (status lampu, AC, proyektor, dan suhu ruangan simulasi).
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
  - Path `id` (required, string): ID ruang kelas.
- **Request Body:**
  ```json
  {
    "twinStatus": "OCCUPIED",
    "simulatedTempC": 22.0,
    "lightingState": "ON",
    "acState": "ON",
    "projectorState": "ON"
  }
  ```
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Digital twin state updated",
    "data": {
      "classroomId": "cls-101",
      "twinStatus": "OCCUPIED",
      "virtualEnvironment": {
        "simulatedTempC": 22.0,
        "lightingState": "ON",
        "acState": "ON",
        "projectorState": "ON"
      },
      "updatedAt": "2026-11-22T09:40:00Z"
    }
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – State Digital Twin berhasil diperbarui.
  - `400 Bad Request` – Nilai parameter simulasi di luar rentang valid.
  - `403 Forbidden` – Tidak memiliki hak akses.
  - `404 Not Found` – Ruang kelas tidak ditemukan.

---

## 7. Analytics API

### 7.1 Ringkasan KPI Dashboard
- **HTTP Method:** `GET`
- **Endpoint URL:** `/api/v1/analytics/summary`
- **Description:** Mengambil metrik KPI utama untuk kartu ringkasan pada halaman Dashboard.
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
- **Request Body:** Tidak diperlukan.
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Analytics summary retrieved",
    "data": {
      "totalClassrooms": 24,
      "activeOccupiedRooms": 12,
      "averageOccupancyRate": 74.5,
      "todayScheduledSessions": 38
    }
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Ringkasan berhasil diambil.
  - `401 Unauthorized` – Token tidak valid.

### 7.2 Laporan Utilisasi Ruang Kelas
- **HTTP Method:** `GET`
- **Endpoint URL:** `/api/v1/analytics/utilization`
- **Description:** Menghasilkan statistik tingkat utilisasi ruang kelas per gedung atau per ruangan dalam rentang waktu tertentu.
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
  - Query `period` (optional, enum: `WEEKLY`, `MONTHLY`, default: `WEEKLY`)
  - Query `building` (optional, string)
- **Request Body:** Tidak diperlukan.
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Utilization analytics retrieved",
    "data": {
      "period": "WEEKLY",
      "byBuilding": [
        {
          "building": "Gedung A",
          "totalRooms": 10,
          "utilizationPercentage": 82.4,
          "averageOccupancyPercentage": 78.1
        },
        {
          "building": "Gedung B",
          "totalRooms": 14,
          "utilizationPercentage": 68.0,
          "averageOccupancyPercentage": 71.2
        }
      ]
    }
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Data utilisasi berhasil diambil.
  - `401 Unauthorized` – Token tidak valid.

### 7.3 Analisis Jam Sibuk Perkuliahan (*Peak Hours*)
- **HTTP Method:** `GET`
- **Endpoint URL:** `/api/v1/analytics/peak-hours`
- **Description:** Mengagregasi tingkat kepadatan penggunaan ruang kelas berdasarkan slot jam perkuliahan harian.
- **Request Parameters:**
  - Header: `Authorization: Bearer <access_token>` (Required)
  - Query `dayOfWeek` (optional, string): Filter hari tertentu.
- **Request Body:** Tidak diperlukan.
- **Response Example (`200 OK`):**
  ```json
  {
    "success": true,
    "message": "Peak hours analytics retrieved",
    "data": [
      { "hourSlot": "08:00 - 10:00", "activeRooms": 18, "utilizationRate": 75.0 },
      { "hourSlot": "10:00 - 12:00", "activeRooms": 21, "utilizationRate": 87.5 },
      { "hourSlot": "13:00 - 15:00", "activeRooms": 16, "utilizationRate": 66.7 },
      { "hourSlot": "15:00 - 17:00", "activeRooms": 9, "utilizationRate": 37.5 }
    ]
  }
  ```
- **HTTP Status Code:**
  - `200 OK` – Data jam sibuk berhasil diambil.
  - `401 Unauthorized` – Token tidak valid.
