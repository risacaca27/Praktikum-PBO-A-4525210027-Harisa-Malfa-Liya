# Laporan Praktikum PBO — Pertemuan 5

**Polimorfisme**

| | |
|---|---|
| **Nama** | Harisa Malfa Liya |
| **NPM** | 4525210027 |
| **Mata Kuliah** | Pemrograman Berorientasi Objek |

---

## 1. Tujuan Praktikum

Praktikum pertemuan 5 membahas **polimorfisme** pada Java dan PHP. Setelah praktikum ini, mahasiswa diharapkan mampu:

- Menjelaskan apa yang terjadi saat method yang di-*override* dipanggil.
- Memakai *upcasting* untuk menulis kode yang tidak peduli tipe konkret.
- Mengenali rantai `instanceof` sebagai tanda rancangan yang bisa diperbaiki.
- Menyebutkan padanan *method overloading* di PHP.

## 2. Konsep yang Digunakan

| Konsep | Penjelasan |
|---|---|
| **Override** | Kelas turunan menyediakan isi sendiri untuk method yang sudah dideklarasikan di kelas induk. |
| **Polimorfisme** | Pemanggilan method lewat variabel bertipe induk dijalankan sesuai tipe objek sebenarnya saat program berjalan. |
| **Upcasting** | Objek turunan disimpan dalam variabel bertipe induk, misalnya `BangunDatar b = new Lingkaran(7);`. |
| **Downcasting** | Mengembalikan variabel bertipe induk ke tipe turunan, hanya dilakukan bila benar-benar perlu (misalnya dengan `instanceof`). |
| **Anti-pattern `instanceof`** | Rantai `if / else if` yang memeriksa tipe membuat kode harus disunting setiap ada tipe baru. Solusinya: pindahkan perilaku ke kelas masing-masing. |
| **Padanan overloading di PHP** | PHP tidak punya *method overloading*. Padanannya antara lain default parameter, *union/nullable type*, dan *variadic*. |

## 3. Struktur Folder

- `src/` — menyimpan kode sumber Java.
- `php/` — menyimpan kode sumber PHP.
- `bin/` — menyimpan hasil kompilasi program Java.
- `README.md` — berisi laporan praktikum.

## 4. Proses Menjalankan Program

### A. Java

Kompilasi semua berkas:

```bash
javac -d bin src/*.java
```

Menjalankan program utama:

```bash
java -cp bin Main
```

Menjalankan versi anti-pattern dan versi refaktornya:

```bash
java -cp bin AntiPattern
java -cp bin AntiPatternRefaktor
```

### B. PHP

```bash
php php/main.php
php php/notifikasi.php
```

## 5. Code Sebelum Diperbarui

Kode awal (template) berisi `TODO` yang harus dikerjakan. Klik untuk membuka.

### Java

<details>
<summary><code>BangunDatar.java</code> (sebelum)</summary>

```java
/**
 * Sesi 5 — polimorfisme.
 * Kelas induk menetapkan KONTRAK; turunan mengisi caranya.
 */
public abstract class BangunDatar {

    private final String nama;

    protected BangunDatar(String nama) { this.nama = nama; }

    /** Kontrak: setiap bangun datar wajib bisa menghitung luas dan kelilingnya. */
    public abstract double luas();
    public abstract double keliling();

    public String getNama() { return nama; }

    /**
     * Perhatikan baris ini baik-baik.
     * toString() ada di kelas INDUK, tetapi memanggil luas() yang isinya
     * hanya ada di TURUNAN. Bagaimana itu mungkin?
     * Jawabannya Anda tulis di penelusuran.md.
     */
    @Override
    public String toString() {
        return String.format("%-12s luas=%10.2f  keliling=%10.2f", nama, luas(), keliling());
    }
}
```

</details>

<details>
<summary><code>Lingkaran.java</code> (sebelum)</summary>

```java
public class Lingkaran extends BangunDatar {

    private final double jariJari;

    public Lingkaran(double jariJari) {
        super("Lingkaran");
        // TODO 1: tolak jari-jari <= 0.
        this.jariJari = jariJari;
    }

    // TODO 2: lengkapi luas() dan keliling().
    //         Gunakan Math.PI, bukan angka 3.14.
    @Override public double luas()     { return 0; }
    @Override public double keliling() { return 0; }

    public double getJariJari() { return jariJari; }
}
```

</details>

<details>
<summary><code>Persegi.java</code> (sebelum)</summary>

```java
public class Persegi extends BangunDatar {

    private final double sisi;

    public Persegi(double sisi) {
        super("Persegi");
        // TODO 1: tolak sisi <= 0.
        this.sisi = sisi;
    }

    // TODO 2: lengkapi luas() dan keliling().
    @Override public double luas()     { return 0; }
    @Override public double keliling() { return 0; }
}
```

</details>

<details>
<summary><code>Main.java</code> (sebelum)</summary>

```java
/**
 * Program uji.
 * ATURAN LANGKAH 1-2: Anda hanya boleh MENAMBAH baris ke dalam array.
 * Logika perulangan di bawah TIDAK BOLEH diubah sama sekali.
 * Kalau Anda merasa perlu mengubahnya, rancangan Anda belum polimorfik.
 */
public class Main {
    public static void main(String[] args) {

        // Upcasting: variabel bertipe induk, objek bertipe turunan.
        BangunDatar[] daftar = {
            new Lingkaran(7),
            new Persegi(5),
            // TODO Langkah 2: tambahkan new Segitiga(3, 4, 5) setelah kelasnya dibuat.
            // TODO Langkah 4: tambahkan Trapesium setelah kelasnya dibuat.
        };

        System.out.println("=== Bangun Datar ===");
        for (BangunDatar b : daftar) {
            System.out.println("  " + b);
        }

        double total = 0;
        for (BangunDatar b : daftar) total += b.luas();
        System.out.printf("%n  Total luas: %.2f%n", total);

        System.out.println();
        System.out.println("Periksa: Lingkaran(7) luas = 153,94 ; Persegi(5) luas = 25,00");
        System.out.println("         Segitiga(3,4,5) luas = 6,00");

        System.out.println();
        System.out.println("=== Downcasting hanya bila benar-benar perlu ===");
        for (BangunDatar b : daftar) {
            if (b instanceof Lingkaran l) {
                System.out.printf("  %s punya jari-jari %.1f%n", l.getNama(), l.getJariJari());
            }
        }
    }
}
```

</details>

<details>
<summary><code>AntiPattern.java</code> (sebelum)</summary>

```java
/**
 * BAHAN LANGKAH 5 — versi yang TIDAK memakai polimorfisme.
 *
 * Jalankan dulu apa adanya, amati keluarannya.
 * Lalu refaktor menjadi polimorfik dan simpan sebagai AntiPatternRefaktor.java.
 * JANGAN hapus berkas ini — keduanya diperlukan saat demo.
 *
 * Pertanyaan pemandu:
 *   1. Berapa baris yang harus disunting untuk menambah satu bangun datar baru?
 *   2. Apa yang terjadi jika seseorang lupa menambahkan satu cabang else-if?
 *   3. Di mana pengetahuan "cara menghitung luas lingkaran" SEHARUSNYA berada?
 */
public class AntiPattern {

    record LingkaranData(double r) {}
    record PersegiData(double sisi) {}
    record SegitigaData(double alas, double tinggi) {}

    /** Setiap bangun datar baru memaksa method ini disunting. */
    static double hitungLuas(Object bangun) {
        if (bangun instanceof LingkaranData l) {
            return Math.PI * l.r() * l.r();
        } else if (bangun instanceof PersegiData p) {
            return p.sisi() * p.sisi();
        } else if (bangun instanceof SegitigaData s) {
            return 0.5 * s.alas() * s.tinggi();
        }
        throw new IllegalArgumentException("Bangun tidak dikenal: " + bangun);
    }

    public static void main(String[] args) {
        Object[] daftar = {
            new LingkaranData(7),
            new PersegiData(5),
            new SegitigaData(4, 3)
        };

        double total = 0;
        for (Object o : daftar) total += hitungLuas(o);
        System.out.printf("Total luas (cara anti-pattern): %.2f%n", total);

        System.out.println();
        System.out.println("Tugas Langkah 5: ubah menjadi polimorfik.");
        System.out.println("Catat di penelusuran.md: berapa baris yang disunting");
        System.out.println("untuk menambah tipe baru pada masing-masing versi?");
    }
}
```

</details>

### PHP

<details>
<summary><code>BangunDatar.php</code> (sebelum)</summary>

```php
<?php
declare(strict_types=1);

abstract class BangunDatar
{
    public function __construct(private readonly string $nama) {}

    abstract public function luas(): float;
    abstract public function keliling(): float;

    public function getNama(): string { return $this->nama; }

    public function __toString(): string
    {
        return sprintf('%-12s luas=%10.2f  keliling=%10.2f',
            $this->nama, $this->luas(), $this->keliling());
    }
}

class Lingkaran extends BangunDatar
{
    public function __construct(private readonly float $jariJari)
    {
        parent::__construct('Lingkaran');
        // TODO 1: tolak jari-jari <= 0.
    }

    // TODO 2: lengkapi. Gunakan M_PI, bukan 3.14.
    public function luas(): float     { return 0; }
    public function keliling(): float { return 0; }

    public function getJariJari(): float { return $this->jariJari; }
}

class Persegi extends BangunDatar
{
    public function __construct(private readonly float $sisi)
    {
        parent::__construct('Persegi');
        // TODO 1: tolak sisi <= 0.
    }

    // TODO 2: lengkapi.
    public function luas(): float     { return 0; }
    public function keliling(): float { return 0; }
}

// TODO Langkah 2: buat kelas Segitiga (tiga sisi, rumus Heron).
//                 Tolak konstruksi bila ketiga sisi tidak membentuk segitiga.
// TODO Langkah 4: buat kelas Trapesium.
```

</details>

<details>
<summary><code>main.php</code> (sebelum)</summary>

```php
<?php
declare(strict_types=1);

require_once __DIR__ . '/BangunDatar.php';

/** @var BangunDatar[] $daftar */
$daftar = [
    new Lingkaran(7),
    new Persegi(5),
    // TODO Langkah 2: tambahkan new Segitiga(3, 4, 5)
];

echo '=== Bangun Datar ===', PHP_EOL;
foreach ($daftar as $b) {
    echo '  ', $b, PHP_EOL;
}

$total = array_sum(array_map(fn (BangunDatar $b): float => $b->luas(), $daftar));
printf('%s  Total luas: %.2f%s', PHP_EOL, $total, PHP_EOL);

echo PHP_EOL, 'Periksa: Lingkaran(7) luas = 153,94 ; Persegi(5) luas = 25,00', PHP_EOL;
```

</details>

<details>
<summary><code>notifikasi.php</code> (sebelum)</summary>

```php
<?php
declare(strict_types=1);

/**
 * Langkah 6 — latihan mandiri.
 *
 * Buat hierarki Notifikasi dengan tiga turunan: Email, SMS, WhatsApp.
 * Lalu lengkapi kirimSemua() TANPA satu pun pemeriksaan tipe.
 */

// TODO 1: buat kelas abstrak Notifikasi dengan:
//         - properti readonly $tujuan
//         - method abstract kirim(string $pesan): void
//         - method saluran(): string yang menyebut nama salurannya


// TODO 2: buat tiga turunan: Email, SMS, WhatsApp.
//         Masing-masing mencetak format pesan yang berbeda.


/**
 * TODO 3: kirim pesan ke seluruh notifikasi dalam daftar.
 *
 * ATURAN: tidak boleh ada instanceof, tidak boleh ada match/switch
 *         atas jenis notifikasi. Kalau Anda merasa membutuhkannya,
 *         berarti hierarki Anda belum benar.
 *
 * @param Notifikasi[] $daftar
 */
function kirimSemua(array $daftar, string $pesan): void
{
    // TODO 3
}

// Uji setelah TODO 1-3 selesai:
// kirimSemua([
//     new Email('ani@univpancasila.ac.id'),
//     new SMS('081234567890'),
//     new WhatsApp('081234567890'),
// ], 'Buku yang Anda pesan sudah tersedia.');
```

</details>

## 6. Implementasi Java

### `BangunDatar.java`

```java
/**
 * Sesi 5 — polimorfisme.
 * Kelas induk menetapkan KONTRAK; turunan mengisi caranya.
 */
public abstract class BangunDatar {

    private final String nama;

    protected BangunDatar(String nama) { this.nama = nama; }

    /** Kontrak: setiap bangun datar wajib bisa menghitung luas dan kelilingnya. */
    public abstract double luas();
    public abstract double keliling();

    public String getNama() { return nama; }

    /**
     * Perhatikan baris ini baik-baik.
     * toString() ada di kelas INDUK, tetapi memanggil luas() yang isinya
     * hanya ada di TURUNAN. Bagaimana itu mungkin?
     * Jawabannya Anda tulis di penelusuran.md.
     */
    @Override
    public String toString() {
        return String.format("%-12s luas=%10.2f  keliling=%10.2f", nama, luas(), keliling());
    }
}
```

### `Lingkaran.java`

```java
public class Lingkaran extends BangunDatar {

    private final double jariJari;

    public Lingkaran(double jariJari) {
        super("Lingkaran");
        // TODO 1: tolak jari-jari <= 0.
        if (jariJari <=0){
            throw new IllegalArgumentException("Jari - jari harus lebih besar dari 0.");
        }
        this.jariJari = jariJari;
    }

    // TODO 2: lengkapi luas() dan keliling().
    //         Gunakan Math.PI, bukan angka 3.14.
    @Override 
    public double luas(){
        return Math.PI *jariJari;
    }

    @Override
    public double keliling(){
        return 2 * Math.PI *jariJari;
    }
    public double getJariJari() { 
        return jariJari; 
    }

}
```

### `Persegi.java`

```java
public class Persegi extends BangunDatar {

    private final double sisi;

    public Persegi(double sisi) {
        super("Persegi");
        // TODO 1: tolak sisi <= 0.
        if(sisi <= 0)
        {
            throw new IllegalArgumentException("Sisi harus lebih besar dari 0");
        }
        this.sisi = sisi;
    }

    // TODO 2: lengkapi luas() dan keliling().
    @Override 
    public double luas()  {   
        return sisi * sisi;
    }
    @Override public double keliling() { 
        return 4 * sisi;
    }

    public double getsisi(){
        return sisi;
    }
}
```

### `Segitiga.java`

```java
public class Segitiga extends BangunDatar{
    private  double alas;
    private double tinggi;
    private double sisiMiring;

    public Segitiga(double alas, double tinggi, double sisiMiring){
        super("Segitiga");
        this.alas = alas;
        this.tinggi = tinggi;
        this.sisiMiring = sisiMiring;
    }

    @Override 
    public double luas(){
        return 0.5 * alas * tinggi;
    }

    @Override 
    public double keliling(){
        return alas + tinggi + sisiMiring;
    }

    @Override 
    public String toString() {
        return getNama() + " (alas = " + alas + 
                            ", tinggi = " + tinggi + 
                            ", sisi Miring = " + sisiMiring + 
                            ") luas =" + luas();
    }
}
```

### `Trapesium.java`

```java
public class Trapesium extends BangunDatar{
    private double sisiAtas;
    private double sisiBawah;
    private double tinggi;
    private double sisiKiri;
    private double sisiKanan;

    public Trapesium(double sisiAtas, double sisiBawah, double tinggi, double sisiKiri, double sisiKanan){
        super("Trapesium");
        this.sisiAtas = sisiAtas;
        this.sisiBawah = sisiBawah;
        this.tinggi = tinggi;
        this.sisiKiri = sisiKiri;
        this.sisiKanan = sisiKanan;
    }

    @Override 
    public double luas(){
        return 0.5 * (sisiAtas + sisiBawah) * tinggi;
    }

    @Override 
    public double keliling(){
        return sisiAtas + sisiBawah + sisiKiri + sisiKanan;
    }

    @Override 
    public String toString() {
        return getNama() + " (atas = " + sisiAtas +
        ", bawah = " + sisiBawah +
        ", tinggi = " + tinggi +
        ") luas = " + luas();
    }
}
```

### `Main.java`

```java
/**
 * Program uji.
 * ATURAN LANGKAH 1-2: Anda hanya boleh MENAMBAH baris ke dalam array.
 * Logika perulangan di bawah TIDAK BOLEH diubah sama sekali.
 * Kalau Anda merasa perlu mengubahnya, rancangan Anda belum polimorfik.
 */
public class Main {
    public static void main(String[] args) {

        // Upcasting: variabel bertipe induk, objek bertipe turunan.
        BangunDatar[] daftar = {
            new Lingkaran(7),
            new Persegi(5),
            new Segitiga(3.0, 4, 5),
            new Trapesium(10, 6, 5, 5, 4)
            // TODO Langkah 2: tambahkan new Segitiga(3, 4, 5) setelah kelasnya dibuat.
            // TODO Langkah 4: tambahkan Trapesium setelah kelasnya dibuat.
        };

        System.out.println("=== Bangun Datar ===");
        for (BangunDatar b : daftar) {
            System.out.println("  " + b);
        }

        double total = 0;
        for (BangunDatar b : daftar) total += b.luas();
        System.out.printf("%n  Total luas: %.2f%n", total);

        System.out.println();
        System.out.println("Periksa: Lingkaran(7) luas = 153,94 ; Persegi(5) luas = 25,00");
        System.out.println("         Segitiga(3,4,5) luas = 6,00");

        System.out.println();
        System.out.println("=== Downcasting hanya bila benar-benar perlu ===");
        for (BangunDatar b : daftar) {
            if (b instanceof Lingkaran l) {
                System.out.printf("  %s punya jari-jari %.1f%n", l.getNama(), l.getJariJari());
            }
        }
    }
}

```

### `AntiPatternRefaktor.java`

```java
public class AntiPatternRefaktor {

    interface Bangun {
        double luas();
    }

    //Tiap
    record LingkaranData(double r ) implements Bangun {
        @Override public double luas(){
            return Math.PI * r * r;
        }
    }

    record PersegiData(double sisi) implements Bangun {
        @Override 
        public double luas(){
            return sisi * sisi;
        }
    }

    record SegitigaData(double alas, double tinggi) implements Bangun {
        @Override 
        public double luas(){
            return 0.5 * alas * tinggi;
        }
    }

    public static void main(String[] args){
        Bangun[] daftar = {
            new LingkaranData(7),
            new PersegiData(5),
            new SegitigaData(4, 3)
        };

        double total = 0; 
        for (Bangun b : daftar){
            total += b.luas();
        }


    System.out.printf("Total luas (cara polimorfik):%.2f%n", total);
    }
}
```

### Hasil Running Java

**`Main`**

<img width="520" height="212" alt="Screenshot 2026-10-11 at 16 25 01" src="https://github.com/user-attachments/assets/cf61de5e-9ca6-4816-a6e2-a5490fe5a197" />


**`AntiPattern`**

<img width="675" height="676" alt="Screenshot 2026-10-11 at 19 15 06" src="https://github.com/user-attachments/assets/f89d747e-b318-487b-b9ad-9ef4f8f150fc" />


**`AntiPatternRefaktor`**

<img width="569" height="619" alt="Screenshot 2026-10-11 at 19 15 27" src="https://github.com/user-attachments/assets/e494a325-1af4-4f1d-8b5e-399fe1160099" />


## 7. Implementasi PHP

### `BangunDatar.php`

```php
<?php
declare(strict_types=1);

abstract class BangunDatar
{
    private string $nama;

    public function __construct(string $nama)
    {
        $this->nama = $nama;
    }

    abstract public function luas(): float;
    abstract public function keliling(): float;

    public function getNama(): string { return $this->nama; }

    public function __toString(): string
    {
        return sprintf('%-12s luas=%10.2f  keliling=%10.2f',
            $this->nama, $this->luas(), $this->keliling());
    }
}

class Lingkaran extends BangunDatar
{
    private float $jariJari;

    public function __construct(float $jariJari)
    {
        parent::__construct('Lingkaran');

        // TODO 1: tolak jari-jari <= 0.
        if ($jariJari <= 0) {
            throw new InvalidArgumentException("Jari-jari harus lebih besar dari 0.");
        }

        $this->jariJari = $jariJari;
    }

    // TODO 2: lengkapi. Gunakan M_PI, bukan 3.14. child 1
    public function luas(): float
    {
        return M_PI * ($this->jariJari ** 2);
    }

    public function keliling(): float
    {
        return 2 * M_PI * $this->jariJari;
    }

    public function getJariJari(): float { return $this->jariJari; }
}

class Persegi extends BangunDatar
{
    private float $sisi;

    public function __construct(float $sisi)
    {
        parent::__construct('Persegi');

        // TODO 1: tolak sisi <= 0.
        if ($sisi <= 0) {
            throw new InvalidArgumentException("Panjang sisi harus lebih besar dari 0.");
        }

        $this->sisi = $sisi;
    }

    // TODO 2: lengkapi. child 2
    public function luas(): float
    {
        return $this->sisi * $this->sisi;
    }

    public function keliling(): float
    {
        return 4 * $this->sisi;
    }

    public function getSisi(): float { return $this->sisi; }
}

// TODO Langkah 2: buat kelas Segitiga (tiga sisi, rumus Heron).
//                 Tolak konstruksi bila ketiga sisi tidak membentuk segitiga.
class Segitiga extends BangunDatar
{
    private float $a;
    private float $b;
    private float $c;

    public function __construct(float $a, float $b, float $c)
    {
        parent::__construct('Segitiga');

        if ($a <= 0 || $b <= 0 || $c <= 0) {
            throw new InvalidArgumentException("Setiap sisi segitiga harus bernilai lebih besar dari 0.");
        }

        // Syarat segitiga valid: jumlah dua sisi harus selalu lebih panjang dari sisi ketiga
        if ($a + $b <= $c || $a + $c <= $b || $b + $c <= $a) {
            throw new InvalidArgumentException("Kombinasi sisi tidak memenuhi syarat segitiga.");
        }

        $this->a = $a;
        $this->b = $b;
        $this->c = $c;
    }

    // child 3
    public function luas(): float
    {
        // Rumus Heron: s = (a + b + c) / 2
        // Luas = sqrt(s * (s - a) * (s - b) * (s - c))
        $s = $this->keliling() / 2;
        return sqrt($s * ($s - $this->a) * ($s - $this->b) * ($s - $this->c));
    }

    public function keliling(): float
    {
        return $this->a + $this->b + $this->c;
    }

    public function getA(): float { return $this->a; }
    public function getB(): float { return $this->b; }
    public function getC(): float { return $this->c; }
}

// TODO Langkah 4: buat kelas Trapesium.
class Trapesium extends BangunDatar
{
    private float $sisiSejajarA;
    private float $sisiSejajarB;
    private float $sisiMiringC;
    private float $sisiMiringD;
    private float $tinggi;

    public function __construct(
        float $sisiSejajarA,
        float $sisiSejajarB,
        float $sisiMiringC,
        float $sisiMiringD,
        float $tinggi
    ) {
        parent::__construct('Trapesium');

        if ($sisiSejajarA <= 0 || $sisiSejajarB <= 0 || 
            $sisiMiringC <= 0 || $sisiMiringD <= 0 || $tinggi <= 0) {
            throw new InvalidArgumentException("Semua parameter trapesium harus bernilai lebih besar dari 0.");
        }

        $this->sisiSejajarA = $sisiSejajarA;
        $this->sisiSejajarB = $sisiSejajarB;
        $this->sisiMiringC = $sisiMiringC;
        $this->sisiMiringD = $sisiMiringD;
        $this->tinggi = $tinggi;
    }

    // child 4
    public function luas(): float
    {
        return 0.5 * ($this->sisiSejajarA + $this->sisiSejajarB) * $this->tinggi;
    }

    public function keliling(): float
    {
        return $this->sisiSejajarA + $this->sisiSejajarB + $this->sisiMiringC + $this->sisiMiringD;
    }

    public function getSisiSejajarA(): float { return $this->sisiSejajarA; }
    public function getSisiSejajarB(): float { return $this->sisiSejajarB; }
    public function getSisiMiringC(): float  { return $this->sisiMiringC; }
    public function getSisiMiringD(): float  { return $this->sisiMiringD; }
    public function getTinggi(): float       { return $this->tinggi; }
}

// --- Driver Code untuk menguji hasil di browser / terminal ---
header('Content-Type: text/plain');

$daftarBangun = [
    new Lingkaran(7),
    new Persegi(4),
    new Segitiga(3, 4, 5),
    new Trapesium(10, 6, 5, 5, 4),
];

foreach ($daftarBangun as $bangun) {
    echo $bangun . PHP_EOL;
}
```

### `main.php`

```php
<?php
declare(strict_types=1);

require_once __DIR__ . '/BangunDatar.php';

/** @var BangunDatar[] $daftar */
$daftar = [
    new Lingkaran(7),
    new Persegi(5),
    // TODO Langkah 2: tambahkan new Segitiga(3, 4, 5)
    new Segitiga(3, 4, 5),
];

echo '=== Bangun Datar ===', PHP_EOL;
foreach ($daftar as $b) {
    echo '  ', $b, PHP_EOL;
}

$total = array_sum(array_map(fn (BangunDatar $b): float => $b->luas(), $daftar));
printf('%s  Total luas: %.2f%s', PHP_EOL, $total, PHP_EOL);

echo PHP_EOL, 'Periksa: Lingkaran(7) luas = 153,94 ; Persegi(5) luas = 25,00; Segitiga(3, 4, 5) luas = 6,00', PHP_EOL;
```

### `notifikasi.php`

```php
<?php
declare(strict_types=1);

/**
 * Langkah 6 — latihan mandiri.
 *
 * Buat hierarki Notifikasi dengan tiga turunan: Email, SMS, WhatsApp.
 * Lalu lengkapi kirimSemua() TANPA satu pun pemeriksaan tipe.
 */

// TODO 1: buat kelas abstrak Notifikasi dengan:
//         - properti readonly $tujuan
//         - method abstract kirim(string $pesan): void
//         - method saluran(): string yang menyebut nama salurannya
abstract class Notifikasi
{
    private string $tujuan;

    public function __construct(string $tujuan)
    {
        $this->tujuan = $tujuan;
    }

    public function getTujuan(): string
    {
        return $this->tujuan;
    }

    abstract public function kirim(string $pesan): void;
    abstract public function saluran(): string;
}

// TODO 2: buat tiga turunan: Email, SMS, WhatsApp.
//         Masing-masing mencetak format pesan yang berbeda.
class Email extends Notifikasi
{
    public function saluran(): string
    {
        return 'Email';
    }

    public function kirim(string $pesan): void
    {
        echo sprintf("[%s] Mengirim email ke <%s>:\n\"%s\"\n\n", $this->saluran(), $this->getTujuan(), $pesan);
    }
}

class SMS extends Notifikasi
{
    public function saluran(): string
    {
        return 'SMS';
    }

    public function kirim(string $pesan): void
    {
        echo sprintf("[%s] SMS terkirim ke nomor %s: %s\n\n", $this->saluran(), $this->getTujuan(), $pesan);
    }
}

class WhatsApp extends Notifikasi
{
    public function saluran(): string
    {
        return 'WhatsApp';
    }

    public function kirim(string $pesan): void
    {
        echo sprintf("[%s] WA chat ke %s -> %s\n\n", $this->saluran(), $this->getTujuan(), $pesan);
    }
}

/**
 * TODO 3: kirim pesan ke seluruh notifikasi dalam daftar.
 *
 * ATURAN: tidak boleh ada instanceof, tidak boleh ada match/switch
 *         atas jenis notifikasi. Kalau Anda merasa membutuhkannya,
 *         berarti hierarki Anda belum benar.
 *
 * @param Notifikasi[] $daftar
 */
function kirimSemua(array $daftar, string $pesan): void
{
    foreach ($daftar as $notifikasi) {
        $notifikasi->kirim($pesan);
    }
}

// Uji setelah TODO 1-3 selesai:
header('Content-Type: text/plain');

kirimSemua([
    new Email('ani@univpancasila.ac.id'),
    new SMS('081234567890'),
    new WhatsApp('081234567890'),
], 'Buku yang Anda pesan sudah tersedia.');
```

### Hasil Running PHP

**`main.php`**

<img width="675" height="184" alt="Screenshot 2026-10-11 at 16 27 09" src="https://github.com/user-attachments/assets/4177bda4-7ffd-4e83-9cb2-4cb4fd73d7ba" />

**`notifikasi.php`**

<img width="708" height="112" alt="Screenshot 2026-10-11 at 19 29 19" src="https://github.com/user-attachments/assets/acd4c0d8-95ff-40b8-a05b-b0d40c6510d1" />


## 8. Analisis

### Jawaban Pertanyaan Pemandu

1. **Berapa baris yang harus disunting untuk menambah satu bangun datar baru?**
  
2. **Apa yang terjadi jika seseorang lupa menambahkan satu cabang `else-if`?**
  
3. **Di mana pengetahuan "cara menghitung luas lingkaran" seharusnya berada?**
   
4. **`toString()` ada di kelas induk tetapi memanggil `luas()` yang isinya ada di turunan. Bagaimana itu mungkin?**
   

### Perbandingan Versi Anti-Pattern dan Polimorfik

| Aspek | Anti-pattern (`instanceof`) | Polimorfik |
|---|---|---|
| Baris yang disunting untuk menambah tipe baru | *(isi)* | *(isi)* |
| Risiko lupa menangani tipe baru | *(isi)* | *(isi)* |

## 9. Kesimpulan

Melalui praktikum ini, saya memahami bahwa polimorfisme membuat pemanggilan method lewat variabel bertipe induk dijalankan sesuai tipe objek sebenarnya saat program berjalan. Karena itu, `toString()` di kelas `BangunDatar` dapat memanggil `luas()` dan `keliling()` yang isinya baru ditulis di kelas turunan seperti `Lingkaran`, `Persegi`, `Segitiga`, dan `Trapesium`.

Dengan *upcasting*, satu array bertipe `BangunDatar` dapat menampung berbagai bangun datar, dan perulangan yang sama dapat dipakai tanpa memeriksa tipe konkretnya. Bangun datar baru cukup ditambahkan sebagai kelas turunan dan satu baris pada array, tanpa mengubah logika perulangan. *Downcasting* dengan `instanceof` hanya dipakai bila benar-benar perlu, misalnya untuk mengakses `getJariJari()` yang hanya dimiliki `Lingkaran`.

Perbandingan `AntiPattern` dan `AntiPatternRefaktor` menunjukkan bahwa rantai `if / else if` dengan `instanceof` adalah tanda rancangan yang bisa diperbaiki. Setiap tipe baru memaksa method `hitungLuas()` disunting, dan jika ada cabang yang terlewat, tipe tersebut tidak akan ditangani dengan benar. Pada versi polimorfik, pengetahuan cara menghitung luas berada di kelas masing-masing, sehingga kode lebih mudah dikembangkan dan lebih kecil kemungkinan salah.

Pada PHP, tidak ada *method overloading* seperti di Java. Padanannya adalah default parameter, *union/nullable type*, dan *variadic*. Sementara itu, polimorfisme pada PHP bekerja dengan cara yang sama seperti di Java melalui kelas abstrak dan method yang di-*override*, seperti pada hierarki `BangunDatar` dan `Notifikasi`.
