# Laporan Praktikum PBO — Pertemuan 4

| | |
|---|---|
| **Nama** | Harisa Malfa Liya |
| **NPM** | 4525210027 |
| **Mata Kuliah** | Pemrograman Berorientasi Objek |

---

## 1. Tujuan Praktikum

Praktikum pertemuan 4 bertujuan untuk memahami penerapan konsep pemrograman berorientasi objek dalam pengelolaan data pegawai dan perhitungan gaji menggunakan Java dan PHP.

## 2. Struktur Folder

- `src/` — menyimpan kode sumber Java.
- `php/` — menyimpan kode sumber PHP.
- `bin/` — menyimpan hasil kompilasi program Java.
- `README.md` — berisi laporan praktikum.

## 3. Proses Menjalankan Program

### A. Java

Perintah kompilasi:

```bash
javac -d bin src/*.java
```

Perintah menjalankan program:

```bash
java -cp bin Main
```

### B. PHP

Perintah menjalankan program:

```bash
php php/main.php
```

## 4. Code Sebelum Diperbarui

Kode awal (template) berisi `TODO` yang harus dikerjakan. Klik untuk membuka.

### Java

<details>
<summary><code>Main.java</code> (sebelum)</summary>

```java
public class Main {
    public static void main(String[] args) {

        // TODO Langkah 4: tambahkan Dosen dan PegawaiHarian ke daftar ini
        //                 setelah Anda membuat kelasnya.
        Pegawai[] daftar = {
            new PegawaiTetap("198701012010", "Ani Lestari",  6_000_000, 15),
            new PegawaiKontrak("K-2024-007",  "Budi Santoso", 5_000_000, 12)
        };

        System.out.println("=== Daftar Gaji ===");
        for (Pegawai p : daftar) {
            System.out.println("  " + p);
        }

        double total = 0;
        for (Pegawai p : daftar) total += p.hitungGaji();
        System.out.printf("%n  Total beban gaji: Rp%,.2f%n", total);

        System.out.println();
        System.out.println("Periksa: Ani (pokok 6.000.000, masa kerja 15 tahun)");
        System.out.println("  tunjangan 15 x 2% = 30%, jadi gaji seharusnya Rp7.800.000,00");

        // Percobaan Langkah 1: hapus komentar baris berikut, kompilasi, catat pesannya.
        // Pegawai langsung = new Pegawai("X", "Y", 1000) { public String jenis() { return "?"; } };
    }
}
```

</details>

<details>
<summary><code>Pegawai.java</code> (sebelum)</summary>

```java
/**
 * Sesi 4 — kelas induk.
 * Menampung apa yang BENAR-BENAR SAMA di semua jenis pegawai.
 *
 * Catatan: `abstract` dan `interface` dibahas tuntas di pertemuan 6.
 * Untuk sekarang cukup pahami: kelas abstract tidak bisa di-new langsung,
 * dan method abstract wajib dilengkapi turunannya.
 */
public abstract class Pegawai {

    // protected: turunan boleh membaca, dunia luar tidak.
    protected final String nip;
    protected final String nama;
    protected final double gajiPokok;

    protected Pegawai(String nip, String nama, double gajiPokok) {
        // TODO 1: tolak gaji pokok negatif.

        this.nip = nip;
        this.nama = nama;
        this.gajiPokok = gajiPokok;
    }

    /**
     * TODO 2: perilaku dasar — kembalikan gaji pokok apa adanya.
     *         Turunan akan MENAMBAH, bukan mengganti seluruhnya.
     */
    public double hitungGaji() {
        return 0;   // ganti
    }

    /** Turunan wajib menyebutkan jenisnya sendiri. */
    public abstract String jenis();

    public String getNama() { return nama; }
    public String getNip()  { return nip; }

    @Override
    public String toString() {
        return String.format("%-14s %-9s %-20s Rp%,.2f", nip, jenis(), nama, hitungGaji());
    }
}
```

</details>

<details>
<summary><code>PegawaiKontrak.java</code> (sebelum)</summary>

```java
public class PegawaiKontrak extends Pegawai {

    private final int bulanKontrak;

    public PegawaiKontrak(String nip, String nama, double gajiPokok, int bulanKontrak) {
        super(nip, nama, gajiPokok);
        this.bulanKontrak = bulanKontrak;
    }

    // TODO 2: pegawai kontrak TIDAK mendapat tunjangan masa kerja.
    //         Apakah method hitungGaji() perlu di-override di sini?
    //         Pikirkan dulu, lalu tuliskan alasannya di catatan.md.

    @Override
    public String jenis() { return "KONTRAK"; }

    public int getBulanKontrak() { return bulanKontrak; }
}
```

</details>

<details>
<summary><code>PegawaiTetap.java</code> (sebelum)</summary>

```java
public class PegawaiTetap extends Pegawai {

    /** Tunjangan masa kerja: 2% gaji pokok per tahun, maksimum 40%. */
    protected static final double TUNJANGAN_PER_TAHUN = 0.02;
    protected static final double TUNJANGAN_MAKSIMUM  = 0.40;

    private final int masaKerjaTahun;

    public PegawaiTetap(String nip, String nama, double gajiPokok, int masaKerjaTahun) {
        // Baris berikut WAJIB dan harus menjadi pernyataan pertama.
        // TODO 1 (Langkah 3): hapus sementara baris ini, kompilasi,
        //         salin pesan kesalahannya ke catatan.md, lalu kembalikan.
        super(nip, nama, gajiPokok);

        this.masaKerjaTahun = masaKerjaTahun;
    }

    /**
     * TODO 2: hitung gaji = gaji dasar induk + tunjangan masa kerja.
     *
     * PENTING: panggil super.hitungGaji() untuk memperoleh gaji dasar.
     *          JANGAN menyalin rumus induk ke sini — itu yang dinilai.
     */
    @Override
    public double hitungGaji() {
        return 0;
    }

    @Override
    public String jenis() { return "TETAP"; }

    protected int getMasaKerjaTahun() { return masaKerjaTahun; }
}
```

</details>

## 5. Implementasi Java

### `Main.java`

```java
public class Main {
    public static void main(String[] args) {

        // TODO Langkah 4: tambahkan Dosen dan PegawaiHarian ke daftar ini
        //                 setelah Anda membuat kelasnya.
        Pegawai[] daftar = {
            new PegawaiTetap("198701012010", "Ani Lestari",  6_000_000, 15),
            new PegawaiKontrak("K-2024-007",  "Budi Santoso", 5_000_000, 12),
            new Dosen("197803122005", "Dr. Hendra", 7_500_000, 10, 2_000_000),
            new PegawaiHarian("H-2024-001", "Citra Dewi", 150_000, 22)
        };

        System.out.println("=== Daftar Gaji ===");
        for (Pegawai p : daftar) {
            System.out.println("  " + p);
        }

        double total = 0;
        for (Pegawai p : daftar) total += p.hitungGaji();
        System.out.printf("%n  Total beban gaji: Rp%,.2f%n", total);

        System.out.println();
        System.out.println("Periksa: Ani (pokok 6.000.000, masa kerja 15 tahun)");
        System.out.println("  tunjangan 15 x 2% = 30%, jadi gaji seharusnya Rp7.800.000,00");
    }
}
```

### `Pegawai.java`

```java
/**
 * Sesi 4 — kelas induk.
 * Menampung apa yang BENAR-BENAR SAMA di semua jenis pegawai.
 *
 * Catatan: `abstract` dan `interface` dibahas tuntas di pertemuan 6.
 * Untuk sekarang cukup pahami: kelas abstract tidak bisa di-new langsung,
 * dan method abstract wajib dilengkapi turunannya.
 */
public abstract class Pegawai {

    // protected: turunan boleh membaca, dunia luar tidak.
    protected final String nip;
    protected final String nama;
    protected final double gajiPokok;

    protected Pegawai(String nip, String nama, double gajiPokok) {
        // TODO 1: tolak gaji pokok negatif.
        if (gajiPokok < 0) {
            throw new IllegalArgumentException("Gaji pokok tidak boleh bernilai negatif. ");
        }

        this.nip = nip;
        this.nama = nama;
        this.gajiPokok = gajiPokok;
    }

    /**
     * TODO 2: perilaku dasar — kembalikan gaji pokok apa adanya.
     *         Turunan akan MENAMBAH, bukan mengganti seluruhnya.
     */
    public double hitungGaji() {
        return this.gajiPokok;  // ganti
    }

    /** Turunan wajib menyebutkan jenisnya sendiri. */
    public abstract String jenis();

    public String getNama() { return nama; }
    public String getNip()  { return nip; }

    @Override
    public String toString() {
        return String.format("%-14s %-9s %-20s Rp%,.2f", nip, jenis(), nama, hitungGaji());
    }
}
```

### `PegawaiKontrak.java`

```java
public class PegawaiKontrak extends Pegawai {

    private final int bulanKontrak;

    public PegawaiKontrak(String nip, String nama, double gajiPokok, int bulanKontrak) {
        super(nip, nama, gajiPokok);
        this.bulanKontrak = bulanKontrak;
    }

    // TODO 2: pegawai kontrak TIDAK mendapat tunjangan masa kerja.
    //         Apakah method hitungGaji() perlu di-override di sini?
    //         Tidak perlu di-override, karena implementasi default pada superclass Pegawai
    //         sudah mengembalikan gajiPokok apa adanya tanpa tunjangan.

    @Override
    public String jenis() { return "KONTRAK"; }

    public int getBulanKontrak() { return bulanKontrak; }
}
```

### `PegawaiTetap.java`

```java
public class PegawaiTetap extends Pegawai {

    /** Tunjangan masa kerja: 2% gaji pokok per tahun, maksimum 40%. */
    protected static final double TUNJANGAN_PER_TAHUN = 0.02;
    protected static final double TUNJANGAN_MAKSIMUM  = 0.40;

    private final int masaKerjaTahun;

    public PegawaiTetap(String nip, String nama, double gajiPokok, int masaKerjaTahun) {
        // Baris berikut WAJIB dan harus menjadi pernyataan pertama.
        // TODO 1 (Langkah 3): hapus sementara baris ini, kompilasi,
        //         salin pesan kesalahannya ke catatan.md, lalu kembalikan.
        super(nip, nama, gajiPokok);

        this.masaKerjaTahun = masaKerjaTahun;
    }

    /**
     * TODO 2: hitung gaji = gaji dasar induk + tunjangan masa kerja.
     *
     * PENTING: panggil super.hitungGaji() untuk memperoleh gaji dasar.
     *          JANGAN menyalin rumus induk ke sini — itu yang dinilai.
     */
    @Override
    public double hitungGaji() {
        double gajiDasar = super.hitungGaji();
        double persenTunjangan = Math.min(masaKerjaTahun * TUNJANGAN_PER_TAHUN, TUNJANGAN_MAKSIMUM);
        return gajiDasar + (gajiDasar * persenTunjangan);
    }

    @Override
    public String jenis() { return "TETAP"; }

    protected int getMasaKerjaTahun() { return masaKerjaTahun; }
}
```

### `Dosen.java`

```java
public class Dosen extends PegawaiTetap {

    private final double tunjanganFungsional;

    public Dosen(String nip, String nama, double gajiPokok, int masaKerjaTahun, double tunjanganFungsional) {
        super(nip, nama, gajiPokok, masaKerjaTahun);

        if (tunjanganFungsional < 0) {
            throw new IllegalArgumentException("Tunjangan fungsional tidak boleh bernilai negatif.");
        }
        this.tunjanganFungsional = tunjanganFungsional;
    }

    @Override
    public double hitungGaji() {
        // Gaji dasar + tunjangan masa kerja (dari PegawaiTetap) + tunjangan fungsional
        return super.hitungGaji() + tunjanganFungsional;
    }

    @Override
    public String jenis() {
        return "DOSEN";
    }

    public double getTunjanganFungsional() {
        return tunjanganFungsional;
    }
}
```

### `PegawaiHarian.java`

```java
public class PegawaiHarian extends Pegawai {

    private final int hariKerja;

    public PegawaiHarian(String nip, String nama, double upahPerHari, int hariKerja) {
        super(nip, nama, upahPerHari);

        if (hariKerja < 0) {
            throw new IllegalArgumentException("Hari kerja tidak boleh bernilai negatif.");
        }
        this.hariKerja = hariKerja;
    }

    @Override
    public double hitungGaji() {
        return this.gajiPokok * this.hariKerja;
    }

    @Override
    public String jenis() {
        return "HARIAN";
    }

    public int getHariKerja() {
        return hariKerja;
    }
}
```


### Hasil Running Java

<img width="518" height="154" alt="Screenshot 2026-10-10 at 23 51 54" src="https://github.com/user-attachments/assets/e721431e-9e17-4949-9a45-467a9ea644eb" />



| Pegawai | Status | Gaji |
|---|---|---:|
| Ani Lestari | Tetap | Rp7.800.000,00 |
| Budi Santoso | Kontrak | Rp5.000.000,00 |
| Dr. Hendra | Dosen | Rp11.000.000,00 |
| Citra Dewi | Harian | Rp3.300.000,00 |

**Total beban gaji Java: Rp27.100.000,00**

## 6. Implementasi PHP

### `main.php`

```php
<?php
declare(strict_types=1);

require_once __DIR__ . '/Pegawai.php';

// TODO Langkah 4: tambahkan Dosen dan PegawaiHarian setelah kelasnya dibuat.
$daftar = [
    new PegawaiTetap('198701012010', 'Ani Lestari', 6_000_000, 15),
    new PegawaiKontrak('K-2024-007', 'Budi Santoso', 5_000_000, 12),
    new Dosen('198501012009', 'Citra Dewi', 7_000_000, 10),
    new PegawaiHarian('H-2024-001', 'Dedi Kurniawan', 200_000, 22),
];

echo '=== Daftar Gaji ===', PHP_EOL;
foreach ($daftar as $p) {
    echo '  ', $p, PHP_EOL;
}

$total = array_sum(array_map(fn (Pegawai $p): float => $p->hitungGaji(), $daftar));
printf('%s  Total beban gaji: Rp%s%s', PHP_EOL, number_format($total, 2, ',', '.'), PHP_EOL);

echo PHP_EOL, 'Periksa: Ani (pokok 6.000.000, masa kerja 15 tahun)', PHP_EOL;
echo '  tunjangan 15 x 2% = 30%, jadi gaji seharusnya Rp7.800.000,00', PHP_EOL;
```

### `Pegawai.php` dan kelas turunannya

```php
<?php
declare(strict_types=1);

/**
 * Sesi 4 — hierarki pegawai (PHP).
 * Seluruh hierarki ditaruh dalam satu berkas agar mudah dibaca berdampingan
 * dengan versi Java. Mulai sesi 9, satu kelas = satu berkas.
 */
abstract class Pegawai
{
    public function __construct(
        protected readonly string $nip,
        protected readonly string $nama,
        protected readonly float  $gajiPokok,
    ) {
        // TODO 1: tolak g aji pokok negatif.
        if ($gajiPokok < 0){
            throw new InvalidArgumentException("Gaji Pokok tidak boleh negatif");
        }
    }

    /** TODO 2: kembalikan gaji pokok apa adanya. */
    public function hitungGaji(): float
    {
        return $this->gajiPokok;   // ganti
    }

    abstract public function jenis(): string;

    public function getNama(): string { return $this->nama; }
    public function getNip(): string  { return $this->nip; }

    public function __toString(): string
    {
        return sprintf('%-14s %-9s %-20s Rp%s',
            $this->nip, $this->jenis(), $this->nama,
            number_format($this->hitungGaji(), 2, ',', '.'));
    }
}

class PegawaiTetap extends Pegawai
{
    protected const TUNJANGAN_PER_TAHUN = 0.02;
    protected const TUNJANGAN_MAKSIMUM  = 0.40;

    public function __construct(
        string $nip, 
        string $nama, 
        float $gajiPokok,
        protected readonly int $masaKerjaTahun,
    ) {
        // WAJIB. TODO 3 (Langkah 3): hapus sementara baris ini,
        //         jalankan, salin pesan kesalahannya, lalu kembalikan.
        parent::__construct($nip, $nama, $gajiPokok);
    }

    /**
     * TODO 4: gaji dasar induk + tunjangan masa kerja.
     *         Gunakan parent::hitungGaji(), jangan menyalin rumusnya.
     */
    public function hitungGaji(): float
    {
       $tunjangan = min(
       $this->masaKerjaTahun * self::TUNJANGAN_PER_TAHUN, 
       self::TUNJANGAN_MAKSIMUM);
        
       return parent::hitungGaji() * ( 1 + $tunjangan);
    }

    public function jenis(): string { return 'TETAP'; }
}

class PegawaiKontrak extends Pegawai
{
    public function __construct(
        string $nip, string $nama, float $gajiPokok,
        private readonly int $bulanKontrak,
    ) {
        parent::__construct($nip, $nama, $gajiPokok);
    }

    public function jenis(): string { return 'KONTRAK'; }

    public function getBulanKontrak(): int { return $this->bulanKontrak; }
}

// TODO Langkah 4: buat kelas Dosen (turunan PegawaiTetap, punya tunjangan fungsional)
//                 dan PegawaiHarian (gaji per hari kerja) di bawah ini.
class Dosen extends PegawaiTetap
{
    protected const TUNJANGAN_FUNGSIONAL = 0.10;
    public function hitungGaji():float{
        return parent::hitungGaji() *(1 + self::TUNJANGAN_FUNGSIONAL);
    }
    public function jenis(): string {
        return 'DOSEN';
    }
}

class PegawaiHarian extends Pegawai
{
    public function __construct(
        string $nip,
        string $nama,
        float $gajiPerHari,
        private readonly int $hariKerja,
    ) {
        parent::__construct($nip, $nama, $gajiPerHari);
    }

    public function hitungGaji(): float 
    {
        return parent ::hitungGaji() * $this->hariKerja;
    }

    public function jenis() : string
    {
        return 'HARIAN';
    }
    
    public function getHariKerja(): int 
    {
        return $this->hariKerja;
    }

}
```

### Hasil Running PHP

<img width="486" height="158" alt="Screenshot 2026-10-10 at 23 54 00" src="https://github.com/user-attachments/assets/7b29f8f4-074c-4ec5-96c5-ece08cb4b923" />



| Pegawai | Status | Gaji |
|---|---|---:|
| Ani Lestari | Tetap | Rp7.800.000,00 |
| Budi Santoso | Kontrak | Rp5.000.000,00 |
| Citra Dewi | Dosen | Rp9.240.000,00 |
| Dedi Kurniawan | Harian | Rp4.400.000,00 |

**Total beban gaji PHP: Rp26.440.000,00**

## 7. Analisis

Kedua program berhasil dijalankan. Java dan PHP sama-sama menampilkan daftar pegawai, status kepegawaian, gaji, dan total beban gaji.

Namun, hasil total gaji berbeda karena data pegawai dan nominal gaji pada kedua program tidak sama. Oleh karena itu, hasil tersebut belum bisa digunakan untuk menyimpulkan bahwa perhitungan Java dan PHP berbeda sebelum data pada kedua program disamakan.

## 8. Kesimpulan

Melalui praktikum ini, saya dapat menjalankan program berbasis objek menggunakan Java dan PHP. Saya juga memahami bahwa data yang digunakan harus konsisten agar hasil dari kedua program dapat dibandingkan dengan tepat.
