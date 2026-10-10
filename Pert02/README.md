# Praktikum PBO Sesi 2 — Enkapsulasi dan Invariant

**Nama:** Harisa Malfa Liya  
**NIM:** 4525210027  
**Mata Kuliah:** Pemrograman Berbasis Objek (PBO)

## 1. Tujuan Praktikum

Praktikum sesi kedua ini bertujuan untuk memahami penggunaan enkapsulasi dalam pemrograman berorientasi objek serta cara menjaga agar data objek tetap sesuai dengan aturan yang telah ditentukan (*invariant*).

Implementasi dilakukan menggunakan PHP 8 dan Java melalui kelas `Mahasiswa`. Kedua program mengolah nilai tugas, UTS, dan UAS untuk menghasilkan nilai akhir serta huruf mutu.

## 2. Aturan yang Diterapkan

Beberapa ketentuan yang digunakan dalam program adalah:

1. NIM wajib diisi dan tidak boleh berupa teks kosong.
2. NIM tidak dapat diubah setelah objek dibuat.
3. Nilai tugas, UTS, dan UAS harus berada di antara 0 sampai 100.
4. Perhitungan nilai akhir menggunakan bobot tugas 30%, UTS 30%, dan UAS 40%.
5. Huruf mutu ditentukan berdasarkan nilai akhir.

| Nilai Akhir | Huruf Mutu |
|---|---|
| 80–100 | A |
| 70–79,99 | B |
| 60–69,99 | C |
| 50–59,99 | D |
| Di bawah 50 | E |

Rumus nilai akhir:

`Nilai Akhir = (Tugas × 0,30) + (UTS × 0,30) + (UAS × 0,40)`

## 3. Kode Sebelum Perbaikan

### A. PHP — `Mahasiswa.php`
<?php
declare(strict_types=1);

/**
 * Sesi 2 - enkapsulasi yang menjaga invariant (PHP).
 * Bandingkan baris demi baris dengan Java/Mahasiswa.java.
 */
class Mahasiswa
{
    public const float BOBOT_TUGAS = 0.30;
    public const float BOBOT_UTS = 0.30;
    public const float BOBOT_UAS = 0.40;

    private const float NILAI_MIN = 0;
    private const float NILAI_MAX = 100;

    /**
     * Constructor property promotion (PHP 8):
     * readonly adalah padanan final pada atribut Java.
     */
    public function __construct(
        private readonly string $nim,
        private readonly string $nama,
        private float $nilaiTugas,
        private float $nilaiUts,
        private float $nilaiUas,
    ) {
        // TODO 2: tolak NIM yang kosong (setelah di-trim).
        // Lemparkan InvalidArgumentException dengan pesan yang jelas.

        // TODO 3: tolak setiap komponen nilai di luar rentang 0-100
        // menggunakan method pembantu di bawah.
    }

    /**
     * TODO 4: Lengkapi validasi satu komponen nilai.
     */
    private static function pastikanNilaiSah(
        string $namaKomponen,
        float $nilai
    ): void {
        // TODO
    }

    /** TODO 5: hitung nilai akhir memakai konstanta bobot. */
    public function nilaiAkhir(): float
    {
        return 0; // ganti
    }

    /** TODO 6: Kembalikan huruf mutu. Petunjuk: match (true) { ... } */
    public function hurufMutu(): string
    {
        return '?'; // ganti
    }

    // TODO 7: sediakan getter seperlunya. JANGAN membuat setNim().
    public function getNim(): string { return $this->nim; }
    public function getNama(): string { return $this->nama; }

    public function __toString(): string
    {
        return sprintf(
            '%-10s %-18s akhir=%6.2f  mutu=%s',
            $this->nim,
            $this->nama,
            $this->nilaiAkhir(),
            $this->hurufMutu()
        );
    }
}

### A. PHP — `main.php`
<?php
declare(strict_types=1);

require_once __DIR__ . '/Mahasiswa.php';

echo '=== Rekap Nilai ===', PHP_EOL;

$kelas = [
    new Mahasiswa('2024001', 'Ani Lestari', 85, 78, 90),
    new Mahasiswa('2024002', 'Budi Santoso', 60, 55, 62),
    new Mahasiswa('2024003', 'Citra Wijaya', 92, 88, 95),
];

foreach ($kelas as $m) {
    echo '  ', $m, PHP_EOL;
}

echo PHP_EOL, '=== Objek menolak data yang melanggar aturan ===', PHP_EOL;

try {
    new Mahasiswa('2024004', 'Salah Nilai', 150, 80, 80);
    echo 'MASALAH: nilai 150 seharusnya ditolak!', PHP_EOL;
} catch (InvalidArgumentException $e) {
    echo '  Ditolak: ', $e->getMessage(), PHP_EOL;
}

try {
    new Mahasiswa('', 'NIM Kosong', 80, 80, 80);
    echo 'MASALAH: NIM kosong seharusnya ditolak!', PHP_EOL;
} catch (InvalidArgumentException $e) {
    echo '  Ditolak: ', $e->getMessage(), PHP_EOL;
}

### B. Java — `Mahasiswa.java`
/**
 * Mahasiswa.java
 * Sesi 2 - enkapsulasi yang menjaga invariant.
 *
 * Deskripsi masalah:
 * Sistem akademik mencatat mahasiswa dengan NIM, nama, dan tiga komponen
 * nilai: tugas, UTS, dan UAS. NIM tidak boleh berubah setelah objek
 * terbentuk. Setiap komponen nilai harus berada dalam rentang 0 sampai 100.
 * Nilai akhir dihitung menggunakan bobot tugas, UTS, dan UAS.
 *
 * Tuliskan lebih dulu implementasi di bawah ini.
 * TODO di bawah ini harus dilengkapi.
 */
public class Mahasiswa {

    // Konstanta bobot - jangan menulis angka 0.30 dan 0.40 di dalam method.
    public static final double BOBOT_TUGAS = 0.30;
    public static final double BOBOT_UTS = 0.30;
    public static final double BOBOT_UAS = 0.40;

    private static final double NILAI_MIN = 0;
    private static final double NILAI_MAX = 100;

    // TODO 1: deklarasikan atribut. Perhatikan mana yang boleh berubah
    // dan mana yang tidak. Gunakan final untuk yang tidak boleh berubah.
    private final String nim;
    private final String nama;
    private final double nilaiTugas;
    private final double nilaiUts;
    private final double nilaiUas;

    public Mahasiswa(String nim, String nama, double nilaiTugas,
                     double nilaiUts, double nilaiUas) {
        // TODO 2: tolak NIM yang kosong atau hanya berisi spasi.
        // Lemparkan IllegalArgumentException dengan pesan yang menyebutkan
        // apa yang salah — bukan sekadar "Error".

        // TODO 3: tolak setiap komponen nilai di luar rentang 0-100.
        // Petunjuk: buat satu method private pembantu agar kode tidak duplikat.

        this.nim = nim;
        this.nama = nama;
        this.nilaiTugas = nilaiTugas;
        this.nilaiUts = nilaiUts;
        this.nilaiUas = nilaiUas;
    }

    // TODO 4: buat method private pembantu untuk memvalidasi satu komponen nilai.
    // Tanda tangan method di bawah ini harus dilengkapi.
    private static void pastikanNilaiSah(String namaKomponen, double nilai) {
        // TODO
    }

    /**
     * TODO 5: hitung nilai akhir memakai konstanta bobot di atas.
     */
    public double nilaiAkhir() {
        return 0; // ganti
    }

    /**
     * TODO 6: kembalikan huruf mutu berdasarkan nilai akhir.
     * Petunjuk:
     * >= 85 -> A
     * >= 70 -> B
     * >= 60 -> C
     * >= 50 -> D
     * selain itu -> E
     */
    public String hurufMutu() {
        return "?"; // ganti
    }

    // Getter
    // TODO 7: sediakan getter untuk nim, nama, dan nilai akhir.
    // JANGAN membuat setNim(). Baca ulang invariant yang dijaga.

    public String getNim() { return nim; }
    public String getNama() { return nama; }

    @Override
    public String toString() {
        return String.format("%-10s %-18s akhir=%6.2f  mutu=%s",
                nim, nama, nilaiAkhir(), hurufMutu());
    }
}

### B. Java — `Main.java`
/**
 * Main.java
 * Program uji — JANGAN DIUBAH pada langkah 1 sampai 4.
 * Kalau kode Anda benar, seluruh keluaran di bawah akan masuk akal.
 */
public class Main {
    public static void main(String[] args) {

        System.out.println("=== Rekap Nilai ===");

        Mahasiswa[] kelas = {
            new Mahasiswa("2024001", "Ani Lestari", 85, 78, 90),
            new Mahasiswa("2024002", "Budi Santoso", 60, 55, 62),
            new Mahasiswa("2024003", "Citra Wijaya", 92, 88, 95)
        };

        for (Mahasiswa m : kelas) {
            System.out.println("  " + m);
        }

        System.out.println();
        System.out.println("=== Objek menolak data yang melanggar aturan ===");

        try {
            new Mahasiswa("2024004", "Salah Nilai", 150, 80, 80);
            System.out.println("MASALAH: nilai 150 seharusnya ditolak!");
        } catch (IllegalArgumentException e) {
            System.out.println("  Ditolak: " + e.getMessage());
        }

        try {
            new Mahasiswa("", "NIM Kosong", 80, 80, 80);
            System.out.println("MASALAH: NIM kosong seharusnya ditolak!");
        } catch (IllegalArgumentException e) {
            System.out.println("  Ditolak: " + e.getMessage());
        }
    }
}

## 4. Implementasi PHP Setelah Perbaikan
##Mahasiswa.php##
<?php
declare(strict_types=1);

class Mahasiswa
{
    public const float BOBOT_TUGAS = 0.30;
    public const float BOBOT_UTS   = 0.30;
    public const float BOBOT_UAS   = 0.40;

    private const float NILAI_MIN = 0;
    private const float NILAI_MAX = 100;

    public function __construct(
        private readonly string $nim,
        private readonly string $nama,
        private float $nilaiTugas,
        private float $nilaiUts,
        private float $nilaiUas,
    ) {
        if (trim($this->nim) === '') {
            throw new InvalidArgumentException(
                'NIM tidak boleh kosong.'
            );
        }

        self::pastikanNilaiSah('Tugas', $this->nilaiTugas);
        self::pastikanNilaiSah('UTS', $this->nilaiUts);
        self::pastikanNilaiSah('UAS', $this->nilaiUas);
    }

    private static function pastikanNilaiSah(
        string $namaKomponen,
        float $nilai
    ): void {
        if ($nilai < self::NILAI_MIN || $nilai > self::NILAI_MAX) {
            throw new InvalidArgumentException(
                "Nilai $namaKomponen harus antara 0 sampai 100."
            );
        }
    }

    public function nilaiAkhir(): float
    {
        return ($this->nilaiTugas * self::BOBOT_TUGAS)
            + ($this->nilaiUts * self::BOBOT_UTS)
            + ($this->nilaiUas * self::BOBOT_UAS);
    }

    public function hurufMutu(): string
    {
        return match (true) {
            $this->nilaiAkhir() >= 85 => 'A',
            $this->nilaiAkhir() >= 75 => 'B',
            $this->nilaiAkhir() >= 65 => 'C',
            $this->nilaiAkhir() >= 50 => 'D',
            default => 'E',
        };
    }

    public function getNim(): string
    {
        return $this->nim;
    }

    public function getNama(): string
    {
        return $this->nama;
    }

    public function __toString(): string
    {
        return sprintf(
            '%-10s %-18s akhir=%6.2f  mutu=%s',
            $this->nim,
            $this->nama,
            $this->nilaiAkhir(),
            $this->hurufMutu()
        );
    }
}
##main.php##
<?php
declare(strict_types=1);

require_once __DIR__ . '/Mahasiswa.php';

echo '=== Rekap Nilai ===', PHP_EOL;
$kelas = [
    new Mahasiswa('2024001', 'Ani Lestari',  85, 78, 90),
    new Mahasiswa('2024002', 'Budi Santoso', 60, 55, 62),
    new Mahasiswa('2024003', 'Citra Wijaya', 92, 88, 95),
];
foreach ($kelas as $m) {
    echo '  ', $m, PHP_EOL;
}

echo PHP_EOL, '=== Objek menolak data yang melanggar aturan ===', PHP_EOL;

try {
    new Mahasiswa('2024004', 'Salah Nilai', 150, 80, 80);
    echo '  MASALAH: nilai 150 seharusnya ditolak!', PHP_EOL;
} catch (InvalidArgumentException $e) {
    echo '  Ditolak: ', $e->getMessage(), PHP_EOL;
}

try {
    new Mahasiswa('', 'NIM Kosong', 80, 80, 80);
    echo '  MASALAH: NIM kosong seharusnya ditolak!', PHP_EOL;
} catch (InvalidArgumentException $e) {
    echo '  Ditolak: ', $e->getMessage(), PHP_EOL;
}




Hasil running PHP
<img width="397" height="130" alt="Screenshot 2026-10-10 at 20 17 12" src="https://github.com/user-attachments/assets/d37e8ce2-ac88-4b72-b31f-545f9ad173fb" />








## 5. Implementasi Java Setelah Perbaikan
##Mahasiswa.java##
public class Mahasiswa {
    public static final double BOBOT_TUGAS = 0.30;
    public static final double BOBOT_UTS = 0.30;
    public static final double BOBOT_UAS = 0.40;

    private final String nim;
    private final String nama;
    private final double nilaiTugas;
    private final double nilaiUts;
    private final double nilaiUas;

    public Mahasiswa(String nim, String nama,
                     double nilaiTugas, double nilaiUts,
                     double nilaiUas) {

        if (nim == null || nim.trim().isEmpty()) {
            throw new IllegalArgumentException(
                "NIM tidak boleh kosong."
            );
        }

        pastikanNilaiSah("Tugas", nilaiTugas);
        pastikanNilaiSah("UTS", nilaiUts);
        pastikanNilaiSah("UAS", nilaiUas);

        this.nim = nim;
        this.nama = nama;
        this.nilaiTugas = nilaiTugas;
        this.nilaiUts = nilaiUts;
        this.nilaiUas = nilaiUas;
    }

    private static void pastikanNilaiSah(
            String namaKomponen, double nilai) {
        if (nilai < 0 || nilai > 100) {
            throw new IllegalArgumentException(
                "Nilai " + namaKomponen
                + " harus antara 0 sampai 100."
            );
        }
    }

    public double nilaiAkhir() {
        return nilaiTugas * BOBOT_TUGAS
             + nilaiUts * BOBOT_UTS
             + nilaiUas * BOBOT_UAS;
    }

    public String hurufMutu() {
        double nilai = nilaiAkhir();

        if (nilai >= 85) return "A";
        if (nilai >= 75) return "B";
        if (nilai >= 65) return "C";
        if (nilai >= 50) return "D";
        return "E";
    }

    public String getNim() {
        return nim;
    }

    public String getNama() {
        return nama;
    }

    @Override
    public String toString() {
        return String.format(
            "%-10s %-18s akhir=%6.2f  mutu=%s",
            nim, nama, nilaiAkhir(), hurufMutu()
        ).replace('.', ',');
    }
}
##Main.java##
/**
 * Program uji — JANGAN DIUBAH pada Langkah 1 sampai 4.
 * Kalau kode Anda benar, seluruh keluaran di bawah akan masuk akal.
 */
public class Main {
    public static void main(String[] args) {

        System.out.println("=== Rekap Nilai ===");
        Mahasiswa[] kelas = {
            new Mahasiswa("2024001", "Ani Lestari",  85, 78, 90),
            new Mahasiswa("2024002", "Budi Santoso", 60, 55, 62),
            new Mahasiswa("2024003", "Citra Wijaya", 92, 88, 95)
        };
        for (Mahasiswa m : kelas) {
            System.out.println("  " + m);
        }

        System.out.println();
        System.out.println("=== Objek menolak data yang melanggar aturan ===");

        try {
            new Mahasiswa("2024004", "Salah Nilai", 150, 80, 80);
            System.out.println("  MASALAH: nilai 150 seharusnya ditolak!");
        } catch (IllegalArgumentException e) {
            System.out.println("  Ditolak: " + e.getMessage());
        }

        try {
            new Mahasiswa("", "NIM Kosong", 80, 80, 80);
            System.out.println("  MASALAH: NIM kosong seharusnya ditolak!");
        } catch (IllegalArgumentException e) {
            System.out.println("  Ditolak: " + e.getMessage());
        }
    }
}





Hasil running Java
<img width="410" height="129" alt="Screenshot 2026-10-10 at 20 35 25" src="https://github.com/user-attachments/assets/f703c039-0bab-4e00-b8b5-793556fe1419" />






## 6. Cara Menjalankan Program

### PHP
Pastikan Terminal berada di folder `Pert02`, kemudian jalankan:
```bash
php main.php
```

### Java
Kompilasi kedua file Java terlebih dahulu:
```bash
javac Main.java Mahasiswa.java
```
Kemudian jalankan program:
```bash
java Main
```
Jika tidak terdapat error, Terminal akan menampilkan rekap nilai dan hasil pengujian validasi.







## 7. Hasil Pengujian

Pengujian dilakukan untuk memastikan perhitungan nilai dan aturan invariant bekerja dengan benar.

| Pengujian | Hasil yang Diharapkan |
|---|---|
| Membuat objek dengan nilai yang valid | Objek berhasil dibuat |
| Menghitung nilai akhir | Nilai dihitung berdasarkan bobot |
| Menentukan huruf mutu | Huruf mutu sesuai rentang nilai |
| Memasukkan nilai 150 | Data ditolak |
| Memasukkan NIM kosong | Data ditolak |






Contoh data yang digunakan pada program:

| NIM | Nama | Tugas | UTS | UAS | Nilai Akhir | Mutu |
|---|---|---:|---:|---:|---:|:---:|
| 2024001 | Ani Lestari | 85 | 78 | 90 | 84,90 | A |
| 2024002 | Budi Santoso | 60 | 55 | 62 | 59,30 | D |
| 2024003 | Citra Wijaya | 92 | 88 | 95 | 92,20 | A |

*Catatan: Nilai akhir pada tabel dihitung dari bobot 30%, 30%, dan 40% sesuai data masukan program.*







## 8. Perbandingan PHP dan Java

| Aspek | PHP 8 | Java |
|---|---|---|
| Menjaga NIM tetap | `readonly` | `final` |
| Atribut kelas | Constructor Property Promotion | Deklarasi atribut dan constructor |
| Validasi data | `InvalidArgumentException` | `IllegalArgumentException` |
| Menghitung nilai | Method `nilaiAkhir()` | Method `nilaiAkhir()` |
| Menentukan mutu | `match` | Kondisi `if` |
| Representasi objek | `__toString()` | `toString()` |

Kedua bahasa dapat menerapkan prinsip enkapsulasi dan invariant. Perbedaannya terletak pada sintaks serta fitur yang digunakan untuk mencapai tujuan tersebut.








## 9. Kesimpulan

Dari praktikum ini, dapat dipahami bahwa enkapsulasi tidak hanya berkaitan dengan pembatasan akses atribut, tetapi juga dengan menjaga data agar tetap valid selama objek digunakan.

Penerapan validasi pada constructor mencegah objek dibuat dengan data yang melanggar aturan. Selain itu, penggunaan `readonly` pada PHP dan `final` pada Java membantu mempertahankan NIM agar tidak berubah setelah objek dibuat.

Melalui implementasi pada kedua bahasa, konsep pemrograman berorientasi objek dapat diterapkan dengan cara yang berbeda, tetapi tetap memiliki tujuan yang sama, yaitu menghasilkan program yang lebih terstruktur dan menjaga konsistensi data.
