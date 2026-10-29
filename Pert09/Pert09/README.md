# Sistem Perpustakaan — Panduan Membangun dan Menjalankan Proyek

Proyek ini merupakan implementasi Sistem Perpustakaan Universitas Pancasila untuk **Praktikum Pemrograman Berorientasi Objek (Sesi 9 — Package, Namespace, dan Build Tool)**. Proyek ini mendemonstrasikan restrukturisasi arsitektur kode dari skema modular datar menjadi struktur baku berbasis build tool:
- **Java (Maven)** dengan konvensi domain terbalik dan pengelolaan pustaka melalui `pom.xml`.
- **PHP (Composer)** dengan autoload standar **PSR-4** dan pengelolaan dependensi melalui `composer.json` serta `composer.lock`.

---

## 1. Struktur Folder Proyek

```
done/
├── .gitignore                      <- Aturan pengabaian berkas biner/artefak build
├── dependency-tree.txt             <- Hasil pohon dependensi Maven (mvn dependency:tree)
├── percobaan.md                    <- Catatan pengujian error pemetaan package/namespace
├── README.md                       <- Dokumentasi cara membangun dan menjalankan
│
├── java/                           <- Proyek Java (Apache Maven)
│   ├── pom.xml                     <- Konfigurasi koordinat proyek & dependensi eksternal
│   └── src/
│       └── main/
│           └── java/
│               └── id/ac/univpancasila/perpustakaan/
│                   ├── App.java                   <- Titik masuk utama program
│                   ├── model/
│                   │   ├── Anggota.java           <- Entitas Anggota perpustakaan
│                   │   ├── Buku.java              <- Entitas Buku dengan Bab (Komposisi)
│                   │   └── Peminjaman.java        <- Entitas Transaksi Peminjaman
│                   ├── service/
│                   │   └── PerpustakaanService.java <- Logika bisnis perpustakaan
│                   └── util/
│                       └── Format.java            <- Format mata uang & tanggal
│
└── php/                            <- Proyek PHP (Composer)
    ├── composer.json               <- Konfigurasi proyek, dependensi & PSR-4 autoload
    ├── composer.lock               <- Penguncian versi deterministik pustaka
    ├── main.php                    <- Titik masuk utama program (single require autoload)
    └── src/
        ├── Model/
        │   ├── Anggota.php
        │   ├── Bab.php
        │   ├── Buku.php
        │   └── Peminjaman.php
        ├── Service/
        │   └── PerpustakaanService.php
        └── Util/
            └── Format.php
```

---

## 2. Prasyarat Perangkat Lunak

Sebelum menjalankan proyek, pastikan perangkat Anda telah terpasang:
1. **Java Development Kit (JDK):** Versi 25 LTS (atau 21+).  
   Periksa dengan perintah: `java -version`
2. **Apache Maven:** Versi 3.9+.  
   Periksa dengan perintah: `mvn -v`
3. **PHP CLI:** Versi 8.2+ / 8.4+.  
   Periksa dengan perintah: `php -v` (pastikan ekstensi `zip` aktif di `php.ini`)
4. **Composer:** Versi 2.x.  
   Periksa dengan perintah: `composer -v`

---

## 3. Menjalankan Proyek Java (Maven)

Semua perintah dijalankan di dalam direktori `done/java/`.

### Langkah 1: Masuk ke direktori Java
```bash
cd java
```

### Langkah 2: Kompilasi Proyek
Maven akan mengunduh dependensi `ascii-table` dan mengompilasi kode program ke dalam folder `target/classes/`:
```bash
mvn clean compile
```
*Pastikan terminal menampilkan status `BUILD SUCCESS`.*

### Langkah 3: Menjalankan Program Utama
Jalankan kelas titik masuk `App` menggunakan plugin `exec-maven-plugin`:
- **Pada Windows PowerShell:**
  ```powershell
  mvn exec:java "-Dexec.mainClass=id.ac.univpancasila.perpustakaan.App"
  ```
- **Pada Bash / Command Prompt:**
  ```bash
  mvn exec:java -Dexec.mainClass=id.ac.univpancasila.perpustakaan.App
  ```

### Langkah 4: Memeriksa Pohon Dependensi
Untuk melihat seluruh daftar pustaka eksternal beserta dependensi transitifnya:
```bash
mvn dependency:tree
```

---

## 4. Menjalankan Proyek PHP (Composer)

Semua perintah dijalankan di dalam direktori `done/php/`.

### Langkah 1: Masuk ke direktori PHP
```bash
cd ../php
```

### Langkah 2: Menginstal Dependensi & Membangun Peta Autoload
Composer akan membaca `composer.json` atau `composer.lock`, mengunduh pustaka `nesbot/carbon` ke folder `vendor/`, dan membuat berkas `vendor/autoload.php`:
```bash
composer install
```

### Langkah 3: Menjalankan Program Utama
Jalankan program melalui CLI PHP:
```bash
php main.php
```
Program akan mengeksekusi simulasi peminjaman dan mencetak katalog buku serta rincian transaksi peminjaman.

### Catatan Tambahan (Membangun Ulang Autoload):
Jika Anda menambahkan kelas baru di dalam folder `src/`, cukup jalankan perintah:
```bash
composer dump-autoload
```

---

## 5. Fitur & Penggunaan Dependensi Eksternal

1. **Java — `com.github.freva:ascii-table` (v1.8.0):**  
   Digunakan di dalam [App.java](file:///D:/FilePerkuliahan/AsprakPBO-A/modul%20praktikum/modul/09-package-namespace-dan-build-tool/done/java/src/main/java/id/ac/univpancasila/perpustakaan/App.java) untuk merender tabel data ASCII pada terminal CLI secara rapi dan simetris (tabel koleksi buku dan transaksi peminjaman).
2. **PHP — `nesbot/carbon`:**  
   Digunakan di dalam [Peminjaman.php](file:///D:/FilePerkuliahan/AsprakPBO-A/modul%20praktikum/modul/09-package-namespace-dan-build-tool/done/php/src/Model/Peminjaman.php) dan [PerpustakaanService.php](file:///D:/FilePerkuliahan/AsprakPBO-A/modul%20praktikum/modul/09-package-namespace-dan-build-tool/done/php/src/Service/PerpustakaanService.php) untuk mengelola tanggal pinjam, penambahan hari jatuh tempo (`addDays()`), serta penghitungan selisih hari keterlambatan (`diffInDays()`) untuk kalkulasi denda.
