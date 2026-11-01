# Laporan Praktikum PBO — Pertemuan 9

**Package, Namespace, dan Build Tool**

| | |
|---|---|
| **Nama** | Harisa Malfa Liya |
| **NPM** | 4525210027 |
| **Mata Kuliah** | Pemrograman Berorientasi Objek |

---

## 1. Deskripsi

Proyek ini adalah implementasi **Sistem Perpustakaan Universitas Pancasila** yang strukturnya diubah dari susunan modular datar menjadi struktur baku berbasis *build tool*:

- **Java (Maven)** dengan konvensi nama domain terbalik dan pengelolaan pustaka melalui `pom.xml`.
- **PHP (Composer)** dengan *autoload* standar **PSR-4** dan pengelolaan dependensi melalui `composer.json` serta `composer.lock`.

## 2. Tujuan Praktikum

- Mengorganisasi kelas ke dalam *package* (Java) dan *namespace* (PHP) sesuai konvensi.
- Memahami pemetaan antara nama package/namespace dan struktur folder.
- Membangun dan menjalankan proyek Java memakai Maven.
- Membangun dan menjalankan proyek PHP memakai Composer dengan autoload PSR-4.
- Mengelola dependensi eksternal beserta dependensi transitifnya.
- Memahami berkas yang harus dan tidak boleh masuk ke repositori (`.gitignore`).

## 3. Konsep yang Digunakan

| Konsep | Penjelasan |
|---|---|
| **Package (Java)** | Pengelompokan kelas dengan nama domain terbalik, yaitu `id.ac.univpancasila.perpustakaan`. Nama package harus sama dengan jalur folder. |
| **Namespace (PHP)** | Padanan package di PHP. Kelas di folder `src/Model/` berada pada namespace yang sesuai dengan jalurnya. |
| **Apache Maven** | Build tool Java. `pom.xml` memuat koordinat proyek dan dependensi eksternal, dan hasil kompilasi disimpan di `target/`. |
| **Composer** | Pengelola dependensi PHP. `composer.json` mendeklarasikan dependensi dan peta autoload. |
| **PSR-4 autoload** | Standar pemetaan namespace ke folder, sehingga `main.php` cukup melakukan satu `require` ke `vendor/autoload.php`. |
| **`composer.lock`** | Mengunci versi pustaka agar instalasi di perangkat lain menghasilkan versi yang persis sama (deterministik). |
| **Dependensi transitif** | Pustaka yang ikut terunduh karena dibutuhkan pustaka yang kita deklarasikan. Di Maven dilihat dengan `mvn dependency:tree`. |
| **`.gitignore`** | Mengecualikan berkas hasil build dan unduhan (`target/`, `vendor/`) dari repositori. |

## 4. Struktur Folder Proyek

```text
sesi-09/
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
│                   ├── App.java                     <- Titik masuk utama program
│                   ├── model/
│                   │   ├── Anggota.java             <- Entitas Anggota perpustakaan
│                   │   ├── Buku.java                <- Entitas Buku dengan Bab (Komposisi)
│                   │   └── Peminjaman.java          <- Entitas Transaksi Peminjaman
│                   ├── service/
│                   │   └── PerpustakaanService.java <- Logika bisnis perpustakaan
│                   └── util/
│                       └── Format.java              <- Format mata uang & tanggal
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

## 5. Prasyarat Perangkat Lunak

| Perangkat | Versi | Perintah pemeriksaan |
|---|---|---|
| Java Development Kit (JDK) | 25 LTS (atau 21+) | `java -version` |
| Apache Maven | 3.9+ | `mvn -v` |
| PHP CLI | 8.2+ / 8.4+ (ekstensi `zip` aktif di `php.ini`) | `php -v` |
| Composer | 2.x | `composer -v` |

## 6. Menjalankan Proyek Java (Maven)

Semua perintah dijalankan di dalam direktori `java/`.

### Langkah 1: Masuk ke direktori Java

```bash
cd java
```

### Langkah 2: Kompilasi proyek

Maven mengunduh dependensi `ascii-table` dan mengompilasi kode ke folder `target/classes/`:

```bash
mvn clean compile
```

Pastikan terminal menampilkan status `BUILD SUCCESS`.

### Langkah 3: Menjalankan program utama

Jalankan kelas titik masuk `App` dengan plugin `exec-maven-plugin`.

Pada **Windows PowerShell**:

```powershell
mvn exec:java "-Dexec.mainClass=id.ac.univpancasila.perpustakaan.App"
```

Pada **Bash / Command Prompt**:

```bash
mvn exec:java -Dexec.mainClass=id.ac.univpancasila.perpustakaan.App
```

### Langkah 4: Memeriksa pohon dependensi

Untuk melihat seluruh pustaka eksternal beserta dependensi transitifnya:

```bash
mvn dependency:tree
```

## 7. Menjalankan Proyek PHP (Composer)

Semua perintah dijalankan di dalam direktori `php/`.

### Langkah 1: Masuk ke direktori PHP

```bash
cd ../php
```

### Langkah 2: Menginstal dependensi dan membangun peta autoload

Composer membaca `composer.json` atau `composer.lock`, mengunduh pustaka `nesbot/carbon` ke folder `vendor/`, dan membuat `vendor/autoload.php`:

```bash
composer install
```

### Langkah 3: Menjalankan program utama

```bash
php main.php
```

Program menjalankan simulasi peminjaman, lalu mencetak katalog buku dan rincian transaksi peminjaman.

### Membangun ulang autoload

Jika menambahkan kelas baru di folder `src/`, jalankan:

```bash
composer dump-autoload
```

## 8. Dependensi Eksternal

| Bahasa | Pustaka | Dipakai di | Fungsi |
|---|---|---|---|
| Java | `com.github.freva:ascii-table` (v1.8.0) | `App.java` | Merender tabel ASCII pada terminal (tabel koleksi buku dan transaksi peminjaman). |
| PHP | `nesbot/carbon` | `Peminjaman.php`, `PerpustakaanService.php` | Mengelola tanggal pinjam, menambah hari jatuh tempo (`addDays()`), dan menghitung selisih hari keterlambatan (`diffInDays()`) untuk denda. |

## 9. Percobaan Error Pemetaan Package dan Namespace

Hasil lengkap ada di `percobaan.md`.

| Percobaan | Pesan kesalahan | Penjelasan |
|---|---|---|
| *(isi, misalnya nama package tidak sama dengan jalur folder)* | *(salin pesan error)* | *(isi)* |
| *(isi, misalnya namespace PHP tidak sesuai peta PSR-4)* | *(salin pesan error)* | *(isi)* |

## 10. Analisis

- **Mengapa nama package Java harus sama dengan jalur folder?** *(isi jawaban di sini)*
- **Apa peran `composer.lock`, dan mengapa dibawa ke repositori?** *(isi jawaban di sini)*
- **Mengapa `target/` dan `vendor/` dimasukkan ke `.gitignore`?** *(isi jawaban di sini)*
- **Apa keuntungan satu `require` ke `vendor/autoload.php` dibanding banyak `require_once` seperti pada pertemuan sebelumnya?** *(isi jawaban di sini)*

## 11. Kesimpulan

Praktikum ini menunjukkan bahwa kode yang sudah berjalan perlu diorganisasi agar mudah dibangun dan dirawat. Pada Java, kelas dikelompokkan dalam package berkonvensi domain terbalik (`model`, `service`, `util`) dan dibangun dengan Maven, yang mengunduh pustaka eksternal serta dependensi transitifnya melalui `pom.xml`. Pada PHP, namespace dipetakan ke folder dengan autoload PSR-4 dan dependensi dikelola Composer, sehingga `main.php` cukup melakukan satu `require`, sedangkan `composer.lock` mengunci versi pustaka agar hasil instalasi konsisten.

Dengan struktur ini, berkas hasil build dan unduhan tidak perlu disimpan di repositori, dan siapa pun dapat membangun ulang proyek hanya dengan `mvn clean compile` atau `composer install`.
