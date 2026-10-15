# Laporan Praktikum PBO — Pertemuan 6

**Abstract Class, Interface, Enum, dan Trait**

| | |
|---|---|
| **Nama** | Harisa Malfa Liya |
| **NPM** | 4525210027 |
| **Mata Kuliah** | Pemrograman Berorientasi Objek |

---

## 1. Tujuan Praktikum

Praktikum pertemuan 6 membahas **abstract class, interface, enum, dan trait** pada Java dan PHP. Setelah praktikum ini, mahasiswa diharapkan mampu:

- Membedakan peran *abstract class* (kode yang benar-benar sama) dan *interface* (kontrak "apa yang bisa dilakukan").
- Memisahkan kontrak menjadi interface kecil (`Movable` dan `Fuelable`) sesuai *Interface Segregation Principle*.
- Menulis kode yang bergantung pada kontrak (interface), bukan pada kelas konkret.
- Memakai *enum* yang memiliki label, harga, dan method sendiri pada Java dan PHP.
- Memakai *default method* pada interface Java.
- Memakai *trait* pada PHP untuk penggunaan ulang kode secara horizontal.

## 2. Konsep yang Digunakan

| Konsep | Penjelasan |
|---|---|
| **Abstract class** | Kelas induk yang tidak bisa di-`new` langsung. Menampung kode yang benar-benar sama di semua turunan (contoh: `Kendaraan`). |
| **Interface** | Kontrak tentang apa yang bisa dilakukan sebuah kelas, bukan apa kelas itu (contoh: `Movable`, `Fuelable`). Satu kelas boleh mengimplementasikan banyak interface. |
| **Interface Segregation Principle** | Kontrak dipecah kecil-kecil agar kelas tidak dipaksa mengimplementasikan hal yang tidak ia butuhkan. Sepeda bergerak tetapi tidak butuh bahan bakar, sehingga hanya `Movable`. |
| **Pewarisan tunggal, interface jamak** | Satu kelas hanya boleh mewarisi satu class, tetapi boleh mengimplementasikan banyak interface. |
| **Default method (Java)** | Interface menyediakan implementasi bawaan, misalnya `ringkasanGerak()` pada `Movable`. PHP tidak punya padanannya di interface. |
| **Enum** | Hanya nilai yang terdaftar yang mungkin ada, dan enum boleh punya method (`biayaPengisian()`, `ramahLingkungan()`). Di PHP dipakai *backed enum* (PHP 8.1+) dengan nilai string. |
| **Trait (PHP)** | Penggunaan ulang kode secara horizontal. Trait `Loggable` dipakai oleh `Mobil` dan `Pesanan` yang tidak sekerabat. |

## 3. Struktur Folder

- `src/` — menyimpan kode sumber Java.
- `php/` — menyimpan kode sumber PHP.
- `bin/` — menyimpan hasil kompilasi program Java.
- `README.md` — berisi laporan praktikum.

## 4. Proses Menjalankan Program

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

`main.php` memuat `abstraksi.php` dengan `require_once`, jadi cukup menjalankan:

```bash
php php/main.php
```

## 5. Code Sebelum Diperbarui

Kode awal (template) berisi `TODO` yang harus dikerjakan. Klik untuk membuka.

### Java

<details>
<summary><code>Movable.java</code> (sebelum)</summary>

```java
/**
 * Sesi 6 — kontrak "bisa bergerak".
 * Interface menjawab: APA YANG BISA dilakukan, bukan APA benda ini.
 */
public interface Movable {

    void bergerak();

    double kecepatanMaksimum();

    /**
     * TODO 1: default method — kembalikan ringkasan berupa
     *         "kecepatan maksimum 180 km/jam" memakai kecepatanMaksimum().
     *
     * Default method (Java 8+) menyediakan implementasi bawaan yang boleh
     * ditimpa implementornya. PHP tidak punya padanannya di interface.
     */
    default String ringkasanGerak() {
        return "(TODO 1 belum dikerjakan)";
    }
}
```

</details>

<details>
<summary><code>Fuelable.java</code> (sebelum)</summary>

```java
/**
 * Kontrak "bisa diisi bahan bakar".
 *
 * Sengaja DIPISAH dari Movable: tidak semua yang bergerak butuh bahan bakar
 * (sepeda), dan tidak semua yang butuh bahan bakar bergerak (generator).
 * Inilah Interface Segregation Principle.
 */
public interface Fuelable {
    void isiBahanBakar(double jumlah);
    double kapasitasTangki();
    TipeBahanBakar tipeBahanBakar();
}
```

</details>

<details>
<summary><code>TipeBahanBakar.java</code> (sebelum)</summary>

```java
/**
 * Enum: hanya nilai yang terdaftar di sini yang mungkin ada.
 * Bandingkan dengan `public static final int BENSIN = 1;`
 * yang membiarkan angka 99 lolos begitu saja.
 */
public enum TipeBahanBakar {

    // TODO 1: lengkapi konstanta enum beserta label dan harga per satuannya.
    //         BENSIN  -> "Bensin",  12000
    //         SOLAR   -> "Solar",   10500
    //         LISTRIK -> "Listrik",  2500   (per kWh)
    BENSIN("Bensin", 0),
    SOLAR("Solar", 0);
    // TODO 2 (Langkah 2): tambahkan LISTRIK.

    private final String label;
    private final double hargaPerSatuan;

    TipeBahanBakar(String label, double hargaPerSatuan) {
        this.label = label;
        this.hargaPerSatuan = hargaPerSatuan;
    }

    public String getLabel() { return label; }

    /** TODO 3: enum boleh punya method — ini yang tidak bisa dilakukan konstanta int. */
    public double biayaPengisian(double jumlah) {
        return 0;   // ganti
    }

    /** TODO 4: kembalikan true hanya untuk LISTRIK. Petunjuk: this == LISTRIK */
    public boolean ramahLingkungan() {
        return false;   // ganti
    }
}
```

</details>

<details>
<summary><code>Kendaraan.java</code> (sebelum)</summary>

```java
/**
 * Abstract class: menampung kode yang BENAR-BENAR SAMA di semua kendaraan.
 * Bandingkan perannya dengan interface Movable dan Fuelable.
 */
public abstract class Kendaraan {

    protected final String merek;
    protected final int tahun;

    protected Kendaraan(String merek, int tahun) {
        this.merek = merek;
        this.tahun = tahun;
    }

    /** TODO 1: kembalikan umur kendaraan, tidak boleh negatif. */
    public int umur(int tahunSekarang) {
        return 0;   // ganti
    }

    public abstract int jumlahRoda();

    @Override
    public String toString() {
        return String.format("%s (%d, %d roda)", merek, tahun, jumlahRoda());
    }
}
```

</details>

<details>
<summary><code>Mobil.java</code> (sebelum)</summary>

```java
/**
 * Satu kelas boleh mewarisi SATU class, tetapi mengimplementasikan BANYAK interface.
 * Tuliskan di keputusan.md: mengapa Java membuat aturan seperti itu?
 */
public class Mobil extends Kendaraan implements Movable, Fuelable {

    private final double kapasitasTangki;
    private double isiTangki = 0;

    public Mobil(String merek, int tahun, double kapasitasTangki) {
        super(merek, tahun);
        this.kapasitasTangki = kapasitasTangki;
    }

    @Override public int jumlahRoda() { return 4; }

    // TODO 1: lengkapi kontrak Movable.
    @Override public void bergerak() {
        // cetak sesuatu seperti "Toyota Avanza melaju di jalan raya"
    }

    @Override public double kecepatanMaksimum() { return 0; }   // ganti

    // TODO 2: lengkapi kontrak Fuelable.
    //         isiBahanBakar harus menolak jumlah <= 0 dan
    //         tidak boleh mengisi melebihi kapasitas tangki.
    @Override public void isiBahanBakar(double jumlah) {
    }

    @Override public double kapasitasTangki() { return kapasitasTangki; }

    @Override public TipeBahanBakar tipeBahanBakar() { return TipeBahanBakar.BENSIN; }

    public double getIsiTangki() { return isiTangki; }
}
```

</details>

<details>
<summary><code>Main.java</code> (sebelum)</summary>

```java
import java.util.List;

public class Main {

    /**
     * Perhatikan tipe parameternya: Fuelable, bukan Mobil.
     * Method ini tidak peduli kelas konkretnya — hanya peduli kontraknya.
     * Kelas baru yang implements Fuelable langsung bisa dipakai di sini,
     * tanpa menyunting satu baris pun.
     */
    static void isiPenuh(Fuelable kendaraan) {
        kendaraan.isiBahanBakar(kendaraan.kapasitasTangki());
        double biaya = kendaraan.tipeBahanBakar().biayaPengisian(kendaraan.kapasitasTangki());
        System.out.printf("  Diisi penuh %s — biaya Rp%,.0f%n",
                kendaraan.tipeBahanBakar().getLabel(), biaya);
    }

    public static void main(String[] args) {
        Mobil mobil = new Mobil("Toyota Avanza", 2022, 45);

        System.out.println("=== Semua Movable ===");
        // TODO Langkah 4: tambahkan Sepeda ke daftar setelah kelasnya dibuat.
        for (Movable m : List.of(mobil)) {
            m.bergerak();
            System.out.println("    " + m.ringkasanGerak());
        }

        System.out.println();
        System.out.println("=== Hanya yang Fuelable ===");
        isiPenuh(mobil);

        // TODO Langkah 4: hapus komentar baris berikut setelah Sepeda dibuat.
        //                 Kompilasi. Salin pesan kesalahannya ke keputusan.md.
        //                 Lalu jelaskan mengapa penolakan saat KOMPILASI itu menguntungkan.
        // isiPenuh(sepeda);

        System.out.println();
        System.out.println("=== Enum punya perilaku ===");
        for (TipeBahanBakar t : TipeBahanBakar.values()) {
            System.out.printf("  %-8s ramah lingkungan? %-5s  biaya 10 satuan: Rp%,.0f%n",
                    t.getLabel(), t.ramahLingkungan(), t.biayaPengisian(10));
        }
    }
}
```

</details>

### PHP

<details>
<summary><code>abstraksi.php</code> (sebelum)</summary>

```php
<?php
declare(strict_types=1);

// ══ INTERFACE — kontrak "apa yang bisa dilakukan" ═══════════════
interface Movable
{
    public function bergerak(): void;
    public function kecepatanMaksimum(): float;
}

interface Fuelable
{
    public function isiBahanBakar(float $jumlah): void;
    public function kapasitasTangki(): float;
    public function tipeBahanBakar(): TipeBahanBakar;
}

// ══ ENUM (PHP 8.1+) — backed enum, punya nilai string ══════════
enum TipeBahanBakar: string
{
    case Bensin  = 'bensin';
    case Solar   = 'solar';
    // TODO 1 (Langkah 2): tambahkan case Listrik = 'listrik';

    /** TODO 2: kembalikan label yang enak dibaca. Petunjuk: match ($this) { ... } */
    public function label(): string
    {
        return '?';   // ganti
    }

    /** TODO 3: Bensin 12000 · Solar 10500 · Listrik 2500 */
    public function hargaPerSatuan(): float
    {
        return 0;   // ganti
    }

    /** TODO 4 */
    public function biayaPengisian(float $jumlah): float
    {
        return 0;   // ganti
    }

    /** TODO 5: hanya Listrik yang ramah lingkungan. */
    public function ramahLingkungan(): bool
    {
        return false;   // ganti
    }
}

// ══ TRAIT — penggunaan ulang horizontal, khas PHP ══════════════
trait Loggable
{
    /**
     * TODO 6: cetak baris log berformat:
     *         [14:32:05] Mobil: servis berkala selesai
     *
     * Petunjuk: static::class memberi nama kelas yang memakai trait ini.
     */
    public function log(string $pesan): void
    {
        // TODO 6
    }
}

// ══ ABSTRACT CLASS — kode yang benar-benar sama ═══════════════
abstract class Kendaraan
{
    public function __construct(
        protected readonly string $merek,
        protected readonly int    $tahun,
    ) {}

    /** TODO 7: umur kendaraan, tidak boleh negatif. */
    public function umur(int $tahunSekarang): int
    {
        return 0;   // ganti
    }

    abstract public function jumlahRoda(): int;

    public function __toString(): string
    {
        return sprintf('%s (%d, %d roda)', $this->merek, $this->tahun, $this->jumlahRoda());
    }
}

final class Mobil extends Kendaraan implements Movable, Fuelable
{
    use Loggable;                       // trait disisipkan

    private float $isiTangki = 0;

    public function __construct(string $merek, int $tahun, private readonly float $kapasitas)
    {
        parent::__construct($merek, $tahun);
    }

    public function jumlahRoda(): int { return 4; }

    // TODO 8: lengkapi kontrak Movable dan Fuelable.
    public function bergerak(): void {}
    public function kecepatanMaksimum(): float { return 0; }
    public function isiBahanBakar(float $jumlah): void {}

    public function kapasitasTangki(): float { return $this->kapasitas; }
    public function tipeBahanBakar(): TipeBahanBakar { return TipeBahanBakar::Bensin; }
    public function getIsiTangki(): float { return $this->isiTangki; }
}

// TODO Langkah 4: buat Sepeda — extends Kendaraan implements Movable,
//                 TETAPI BUKAN Fuelable.

/**
 * TODO Langkah 5: buat kelas Pesanan yang juga memakai trait Loggable.
 * Kelas ini sama sekali bukan kerabat Kendaraan — itulah maksud
 * "penggunaan ulang horizontal".
 */
```

</details>

<details>
<summary><code>main.php</code> (sebelum)</summary>

```php
<?php
declare(strict_types=1);

require_once __DIR__ . '/abstraksi.php';

/** Tidak peduli kelas konkretnya — hanya peduli kontraknya. */
function isiPenuh(Fuelable $kendaraan): void
{
    $kendaraan->isiBahanBakar($kendaraan->kapasitasTangki());
    $biaya = $kendaraan->tipeBahanBakar()->biayaPengisian($kendaraan->kapasitasTangki());
    printf('  Diisi penuh %s — biaya Rp%s%s',
        $kendaraan->tipeBahanBakar()->label(),
        number_format($biaya, 0, ',', '.'), PHP_EOL);
}

$mobil = new Mobil('Toyota Avanza', 2022, 45);

echo '=== Semua Movable ===', PHP_EOL;
// TODO Langkah 4: tambahkan $sepeda ke daftar setelah kelasnya dibuat.
foreach ([$mobil] as $m) {
    $m->bergerak();
    printf('    kecepatan maksimum %.0f km/jam%s', $m->kecepatanMaksimum(), PHP_EOL);
}

echo PHP_EOL, '=== Hanya yang Fuelable ===', PHP_EOL;
isiPenuh($mobil);
// TODO Langkah 4: hapus komentar berikut setelah Sepeda dibuat, jalankan,
//                 dan salin pesan TypeError-nya ke keputusan.md.
// isiPenuh($sepeda);

echo PHP_EOL, '=== Enum punya perilaku ===', PHP_EOL;
foreach (TipeBahanBakar::cases() as $t) {
    printf('  %-8s ramah lingkungan? %-5s  biaya 10 satuan: Rp%s%s',
        $t->label(),
        $t->ramahLingkungan() ? 'ya' : 'tidak',
        number_format($t->biayaPengisian(10), 0, ',', '.'), PHP_EOL);
}

echo PHP_EOL, '=== Trait dipakai kelas yang tidak sekerabat ===', PHP_EOL;
$mobil->log('servis berkala selesai');
// TODO Langkah 5: (new Pesanan())->log('pesanan #1042 dibuat');
```

</details>

## 6. Implementasi Java

### `Movable.java`

```java
/**
 * Sesi 6 — kontrak "bisa bergerak".
 * Interface menjawab: APA YANG BISA dilakukan, bukan APA benda ini.
 */
public interface Movable {

    void bergerak();

    double kecepatanMaksimum();

    /**
     * TODO 1: default method — kembalikan ringkasan berupa
     *         "kecepatan maksimum 180 km/jam" memakai kecepatanMaksimum().
     *
     * Default method (Java 8+) menyediakan implementasi bawaan yang boleh
     * ditimpa implementornya. PHP tidak punya padanannya di interface.
     */
    default String ringkasanGerak() {
        return "kecepatan maksimum" + kecepatanMaksimum() + "km/jam";
    }
}
```

### `Fuelable.java`

```java
/**
 * Kontrak "bisa diisi bahan bakar".
 *
 * Sengaja DIPISAH dari Movable: tidak semua yang bergerak butuh bahan bakar
 * (sepeda), dan tidak semua yang butuh bahan bakar bergerak (generator).
 * Inilah Interface Segregation Principle.
 */
public interface Fuelable {
    void isiBahanBakar(double jumlah);
    double kapasitasTangki();
    TipeBahanBakar tipeBahanBakar();
}
```

### `TipeBahanBakar.java`

```java
/**
 * Enum: hanya nilai yang terdaftar di sini yang mungkin ada.
 * Bandingkan dengan `public static final int BENSIN = 1;`
 * yang membiarkan angka 99 lolos begitu saja.
 */
public enum TipeBahanBakar {

    // TODO 1: lengkapi konstanta enum beserta label dan harga per satuannya.
    //         BENSIN  -> "Bensin",  12000
    //         SOLAR   -> "Solar",   10500
    //         LISTRIK -> "Listrik",  2500   (per kWh)
    BENSIN("Bensin", 12000),
    SOLAR("Solar", 10500),
    LISTRIK("Listrik", 2500);
    // TODO 2 (Langkah 2): tambahkan LISTRIK.

    private final String label;
    private final double hargaPerSatuan;

    TipeBahanBakar(String label, double hargaPerSatuan) {
        this.label = label;
        this.hargaPerSatuan = hargaPerSatuan;
    }

    public String getLabel() { return label; }

    /** TODO 3: enum boleh punya method — ini yang tidak bisa dilakukan konstanta int. */
    public double biayaPengisian(double jumlah) {
        return jumlah * hargaPerSatuan;   // ganti
    }

    /** TODO 4: kembalikan true hanya untuk LISTRIK. Petunjuk: this == LISTRIK */
    public boolean ramahLingkungan() {
        return this == LISTRIK;   // ganti
    }
}
```

### `Kendaraan.java`

```java
/**
 * Abstract class: menampung kode yang BENAR-BENAR SAMA di semua kendaraan.
 * Bandingkan perannya dengan interface Movable dan Fuelable.
 */
public abstract class Kendaraan {

    protected final String merek;
    protected final int tahun;

    protected Kendaraan(String merek, int tahun) {
        this.merek = merek;
        this.tahun = tahun;
    }

    /** TODO 1: kembalikan umur kendaraan, tidak boleh negatif. */
    public int umur(int tahunSekarang) {
        return Math.max(0, tahunSekarang - tahun);  // ganti
    }

    public abstract int jumlahRoda();

    @Override
    public String toString() {
        return String.format("%s (%d, %d roda)", merek, tahun, jumlahRoda());
    }
}
```

### `Mobil.java`

```java
/**
 * Satu kelas boleh mewarisi SATU class, tetapi mengimplementasikan BANYAK interface.
 * Tuliskan di keputusan.md: mengapa Java membuat aturan seperti itu?
 */
public class Mobil extends Kendaraan implements Movable, Fuelable {

    private final double kapasitasTangki;
    private double isiTangki = 0;

    public Mobil(String merek, int tahun, double kapasitasTangki) {
        super(merek, tahun);
        this.kapasitasTangki = kapasitasTangki;
    }

    @Override public int jumlahRoda() { return 4; }

    // TODO 1: lengkapi kontrak Movable.
    @Override public void bergerak() {
        // cetak sesuatu seperti "Toyota Avanza melaju di jalan raya"
        System.out.println(merek + " melaju di jalan raya");
    }

    @Override public double kecepatanMaksimum() { return 180; }   // ganti

    // TODO 2: lengkapi kontrak Fuelable.
    //         isiBahanBakar harus menolak jumlah <= 0 dan
    //         tidak boleh mengisi melebihi kapasitas tangki.
    @Override public void isiBahanBakar(double jumlah) {
        if (jumlah <= 0 ){
            throw new IllegalArgumentException("Jumlah bahan bakar harus lebih dari 0");
        }
        if (isiTangki + jumlah > kapasitasTangki){
            throw new IllegalArgumentException("Pengisian melebihi kapastias tangki");
        }
        isiTangki += jumlah;
    }

    @Override public double kapasitasTangki() { return kapasitasTangki; }

    @Override public TipeBahanBakar tipeBahanBakar() { return TipeBahanBakar.BENSIN; }

    public double getIsiTangki() { return isiTangki; }
}
```

### `Sepeda.java`

```java
public class Sepeda extends Kendaraan implements Movable {

    public Sepeda(String merek, int tahun){
        super(merek, tahun);
    }

    @Override
    public int jumlahRoda(){
        return 2;
    }

    @Override public void bergerak(){
        System.out.println(merek + "dikayuh santai di jalan");
    }

    @Override 
    public double kecepatanMaksimum(){
        return 30;
    }

}
```

### `Main.java`

```java
import java.util.List;

public class Main {

    /**
     * Perhatikan tipe parameternya: Fuelable, bukan Mobil.
     * Method ini tidak peduli kelas konkretnya — hanya peduli kontraknya.
     * Kelas baru yang implements Fuelable langsung bisa dipakai di sini,
     * tanpa menyunting satu baris pun.
     */
    static void isiPenuh(Fuelable kendaraan) {
        kendaraan.isiBahanBakar(kendaraan.kapasitasTangki());
        double biaya = kendaraan.tipeBahanBakar().biayaPengisian(kendaraan.kapasitasTangki());
        System.out.printf("  Diisi penuh %s — biaya Rp%,.0f%n",
                kendaraan.tipeBahanBakar().getLabel(), biaya);
    }

    public static void main(String[] args) {
        Mobil mobil = new Mobil("Toyota Avanza", 2022, 45);

        Sepeda sepeda = new Sepeda("Polygon", 2023);

        System.out.println("=== Semua Movable ===");
        // TODO Langkah 4: tambahkan Sepeda ke daftar setelah kelasnya dibuat.
        for (Movable m : List.of(mobil, sepeda)) {
            m.bergerak();
            System.out.println("    " + m.ringkasanGerak());
        }

        System.out.println();
        System.out.println("=== Hanya yang Fuelable ===");
        isiPenuh(mobil);

        // TODO Langkah 4: hapus komentar baris berikut setelah Sepeda dibuat.
        //                 Kompilasi. Salin pesan kesalahannya ke keputusan.md.
        //                 Lalu jelaskan mengapa penolakan saat KOMPILASI itu menguntungkan.
        // isiPenuh(sepeda);

        System.out.println();
        System.out.println("=== Enum punya perilaku ===");
        for (TipeBahanBakar t : TipeBahanBakar.values()) {
            System.out.printf("  %-8s ramah lingkungan? %-5s  biaya 10 satuan: Rp%,.0f%n",
                    t.getLabel(), t.ramahLingkungan(), t.biayaPengisian(10));
        }
    }
}
```

### Hasil Running Java

<img width="497" height="229" alt="Screenshot 2026-10-11 at 19 45 04" src="https://github.com/user-attachments/assets/5a4526aa-049d-46df-b61f-a43bf58f6bff" />


## 7. Implementasi PHP

### `abstraksi.php`

```php
<?php
declare(strict_types=1);

// ══ INTERFACE — kontrak "apa yang bisa dilakukan" ═══════════════
interface Movable
{
    public function bergerak(): void;
    public function kecepatanMaksimum(): float;
}

interface Fuelable
{
    public function isiBahanBakar(float $jumlah): void;
    public function kapasitasTangki(): float;
    public function tipeBahanBakar(): TipeBahanBakar;
}

// ══ ENUM (PHP 8.1+) — backed enum, punya nilai string ══════════
enum TipeBahanBakar: string
{
    case Bensin  = 'bensin';
    case Solar   = 'solar';
    // TODO 1 (Langkah 2): tambahkan case Listrik = 'listrik';
    case Listrik ='listrik';

    /** TODO 2: kembalikan label yang enak dibaca. Petunjuk: match ($this) { ... } */
    public function label(): string
    {
        return match ($this){
            self::Bensin => 'Bensin',
            self::Solar => 'Solar',
            self::Listrik => 'Listrik',
        };   // ganti
    }

    /** TODO 3: Bensin 12000 · Solar 10500 · Listrik 2500 */
    public function hargaPerSatuan(): float
    {
        return match ($this){
            self::Bensin => 12000,
            self::Solar => 105000,
            self::Listrik => 2500,
        };   // ganti
    }

    /** TODO 4 */
    public function biayaPengisian(float $jumlah): float
    {
        return $jumlah * $this->hargaPersatuan();   // ganti
    }

    /** TODO 5: hanya Listrik yang ramah lingkungan. */
    public function ramahLingkungan(): bool
    {
        return $this === self::Listrik;   // ganti
    }
}

// ══ TRAIT — penggunaan ulang horizontal, khas PHP ══════════════
trait Loggable
{
    /**
     * TODO 6: cetak baris log berformat:
     *         [14:32:05] Mobil: servis berkala selesai
     *
     * Petunjuk: static::class memberi nama kelas yang memakai trait ini.
     */
    public function log(string $pesan): void
    {// TODO 6
        printf("[%s] %s: %s\n",date('H:i:s'),static::class, $pesan);
    }
}

// ══ ABSTRACT CLASS — kode yang benar-benar sama ═══════════════
abstract class Kendaraan
{
    public function __construct(
        protected readonly string $merek,
        protected readonly int    $tahun,
    ) {}

    /** TODO 7: umur kendaraan, tidak boleh negatif. */
    public function umur(int $tahunSekarang): int
    {
        return max(0, $tahunSekarang - $this->tahun);   // ganti
    }

    abstract public function jumlahRoda(): int;

    public function __toString(): string
    {
        return sprintf('%s (%d, %d roda)', $this->merek, $this->tahun, $this->jumlahRoda());
    }
}

final class Mobil extends Kendaraan implements Movable, Fuelable
{
    use Loggable;                       // trait disisipkan

    private float $isiTangki = 0;

    public function __construct(string $merek, int $tahun, private readonly float $kapasitas)
    {
        parent::__construct($merek, $tahun);
    }

    public function jumlahRoda(): int { return 4; }

    // TODO 8: lengkapi kontrak Movable dan Fuelable.
    public function bergerak(): void 
    {
        echo "{$this->merek} melaju di jalan raya\n";
    }
    public function kecepatanMaksimum(): float 
    { 
        return 180; 
    }
    public function isiBahanBakar(float $jumlah): void 
    {
        if ($jumlah <= 0){
            throw new InvalidArgumentException('Jumlah bahan bakar harus lebih dari 0');
        }

        if ($this->isiTangki + $jumlah > $this->kapasitas){
            throw new InvalidArgumentException('Pengisian melebihi kapasitad tangki');
        }
        $this->isiTangki += $jumlah;
    }

    public function kapasitasTangki(): float { return $this->kapasitas; }
    public function tipeBahanBakar(): TipeBahanBakar { return TipeBahanBakar::Bensin; }
    public function getIsiTangki(): float { return $this->isiTangki; }
}

// TODO Langkah 4: buat Sepeda — extends Kendaraan implements Movable,
//                 TETAPI BUKAN Fuelable.
final class Sepeda extends Kendaraan implements Movable
{
    public function jumlahRoda(): int 
    {
        return 2;
    }

    public function bergerak(): void 
    {
        echo "{$this->merek} mengayuh di jalan\n";
    }

    public function kecepatanMaksimum(): float 
    {
        return 40;
    }
}
/**
 * TODO Langkah 5: buat kelas Pesanan yang juga memakai trait Loggable.
 * Kelas ini sama sekali bukan kerabat Kendaraan — itulah maksud
 * "penggunaan ulang horizontal".
 */
final class Pesanan 
{
    use Loggable;
}
```

### `main.php`

```php
<?php
declare(strict_types=1);

require_once __DIR__ . '/abstraksi.php';

/** Tidak peduli kelas konkretnya — hanya peduli kontraknya. */
function isiPenuh(Fuelable $kendaraan): void
{
    $kendaraan->isiBahanBakar($kendaraan->kapasitasTangki());
    $biaya = $kendaraan->tipeBahanBakar()->biayaPengisian($kendaraan->kapasitasTangki());
    printf('  Diisi penuh %s — biaya Rp%s%s',
        $kendaraan->tipeBahanBakar()->label(),
        number_format($biaya, 0, ',', '.'), PHP_EOL);
}

$mobil = new Mobil('Toyota Avanza', 2022, 45);

$sepeda = new Sepeda('Polygon', 2023);

echo '=== Semua Movable ===', PHP_EOL;
// TODO Langkah 4: tambahkan $sepeda ke daftar setelah kelasnya dibuat.
foreach ([$mobil, $sepeda] as $m) {
    $m->bergerak();
    printf('    kecepatan maksimum %.0f km/jam%s', $m->kecepatanMaksimum(), PHP_EOL);
}

echo PHP_EOL, '=== Hanya yang Fuelable ===', PHP_EOL;
isiPenuh($mobil);
// TODO Langkah 4: hapus komentar berikut setelah Sepeda dibuat, jalankan,
//                 dan salin pesan TypeError-nya ke keputusan.md.
// isiPenuh($sepeda);

echo PHP_EOL, '=== Enum punya perilaku ===', PHP_EOL;
foreach (TipeBahanBakar::cases() as $t) {
    printf('  %-8s ramah lingkungan? %-5s  biaya 10 satuan: Rp%s%s',
        $t->label(),
        $t->ramahLingkungan() ? 'ya' : 'tidak',
        number_format($t->biayaPengisian(10), 0, ',', '.'), PHP_EOL);
}

echo PHP_EOL, '=== Trait dipakai kelas yang tidak sekerabat ===', PHP_EOL;
$mobil->log('servis berkala selesai');
// TODO Langkah 5: (new Pesanan())->log('pesanan #1042 dibuat');
(new Pesanan())->log('pesanan #1042 dibuat');
```

### Hasil Running PHP

<img width="489" height="257" alt="Screenshot 2026-10-11 at 19 47 49" src="https://github.com/user-attachments/assets/ced3d0b9-2908-4ee2-82e4-8fbfc1eca17b" />


## 8. Analisis

### Jawaban Pertanyaan Pemandu

1. **Mengapa Java membuat aturan satu kelas hanya boleh mewarisi satu class, tetapi boleh mengimplementasikan banyak interface?**
   Pewarisan dari banyak class dapat menimbulkan ambiguitas, misalnya bila dua induk punya method yang sama dengan isi berbeda (*diamond problem*). Interface hanya menetapkan kontrak (dan default method yang bisa ditimpa), sehingga satu kelas aman memenuhi banyak kontrak sekaligus.

2. **Mengapa penolakan `isiPenuh(sepeda)` saat kompilasi itu menguntungkan?**
   Kesalahan terdeteksi sebelum program dijalankan, sehingga tidak ada kendaraan tanpa bahan bakar yang sampai diisi saat runtime. Kontrak `Fuelable` dijaga oleh compiler, bukan oleh pemeriksaan manual.

3. **Apa perbedaan penolakan di Java dan di PHP?**
   Java menolak saat kompilasi (*compile-time error*), sedangkan PHP menolak saat program dijalankan dengan `TypeError`. Keduanya sama-sama mencegah `Sepeda` dipakai sebagai `Fuelable`, tetapi PHP baru menemukan kesalahan ketika baris tersebut dieksekusi.

4. **Mengapa `Movable` dan `Fuelable` dipisah?**
   Tidak semua yang bergerak butuh bahan bakar (sepeda), dan tidak semua yang butuh bahan bakar bergerak (generator). Dengan memisah kontrak (*Interface Segregation Principle*), kelas hanya mengimplementasikan apa yang benar-benar ia butuhkan.

### Perbandingan Abstract Class, Interface, dan Trait

| Aspek | Abstract class | Interface | Trait (PHP) |
|---|---|---|---|
| Fungsi utama | Menampung kode yang sama di semua turunan | Menetapkan kontrak apa yang bisa dilakukan | Menyisipkan kode yang bisa dipakai kelas yang tidak sekerabat |
| Boleh berisi implementasi | Ya | Hanya default method (Java) | Ya |
| Jumlah yang boleh dipakai satu kelas | Satu | Banyak | Banyak |
| Contoh pada praktikum | `Kendaraan` | `Movable`, `Fuelable` | `Loggable` |

## 9. Kesimpulan

Praktikum ini menunjukkan perbedaan peran abstract class, interface, enum, dan trait. *Abstract class* `Kendaraan` menampung kode yang benar-benar sama di semua kendaraan, sedangkan *interface* `Movable` dan `Fuelable` menetapkan kontrak tentang apa yang bisa dilakukan. Dengan memisahkan kedua kontrak tersebut, `Sepeda` cukup mengimplementasikan `Movable` tanpa dipaksa memiliki bahan bakar, sesuai *Interface Segregation Principle*.

Method seperti `isiPenuh()` bergantung pada tipe `Fuelable`, bukan pada kelas konkret `Mobil`, sehingga kelas baru yang memenuhi kontrak langsung dapat dipakai tanpa mengubah kode. Ketika `Sepeda` dikirim ke method tersebut, Java menolaknya saat kompilasi, sedangkan PHP menolaknya saat dijalankan dengan `TypeError`.

*Enum* `TipeBahanBakar` membatasi nilai hanya pada yang terdaftar dan dapat menyimpan label, harga, serta method seperti `biayaPengisian()` dan `ramahLingkungan()`, hal yang tidak dapat dilakukan konstanta bilangan bulat biasa. Pada PHP, *trait* `Loggable` memungkinkan kode yang sama dipakai ulang oleh `Mobil` dan `Pesanan` yang tidak sekerabat, sementara *default method* pada interface hanya tersedia di Java.
