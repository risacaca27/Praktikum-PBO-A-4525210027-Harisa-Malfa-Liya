# Praktikum PBO — Pertemuan 3

**Constructor Berdelegasi, Anggota Statis, dan Konstanta**

| | |
|---|---|
| **Nama** | Harisa Malfa Liya |
| **NPM** | 4525210027 |

---

## 1. Deskripsi

Pada pertemuan ini, program `RekeningBank` dikembangkan untuk menerapkan konsep constructor, anggota statis (`static`), konstanta, serta validasi agar kondisi objek tetap sesuai aturan (*invariant*). Contoh program menggunakan rekening milik Ani, Budi, dan Citra.

Implementasi dibuat dalam dua bahasa, yaitu **Java** dan **PHP**. Keduanya memiliki tujuan yang sama, tetapi cara membuat objek dengan constructor berbeda disesuaikan dengan fitur masing-masing bahasa.

## 2. Tujuan Praktikum

- Menggunakan konstanta bernama untuk bunga tahunan, biaya administrasi, dan batas penarikan.
- Menghitung jumlah rekening yang dibuat menggunakan anggota statis.
- Menerapkan constructor berdelegasi pada Java.
- Menggunakan default parameter dan named constructor pada PHP.
- Memvalidasi nomor rekening, saldo awal, setoran, dan penarikan.
- Menjaga invariant agar saldo tidak negatif dan transaksi mengikuti aturan.

## 3. Konsep yang Digunakan

| Konsep | Penjelasan |
|---|---|
| **Constructor berdelegasi (Java)** | Constructor ringkas memanggil constructor lengkap dengan `this(...)` agar validasi dan inisialisasi tidak ditulis berulang. |
| **Konstanta** | Nilai tetap, seperti bunga tahunan `0.025`, biaya administrasi `5000`, dan batas penarikan `5000000`, disimpan sebagai konstanta bernama. |
| **Anggota statis** | Penghitung jumlah rekening dan metode utilitas dapat digunakan melalui kelas tanpa bergantung pada satu objek tertentu. |
| **Invariant** | Saldo tidak boleh negatif, nomor rekening tidak berubah setelah objek dibuat, dan setoran maupun penarikan harus bernilai positif. |
| **Validasi dan exception** | Input yang tidak sesuai aturan ditolak dengan exception dan pesan yang menjelaskan penyebabnya. |
| **Named constructor (PHP)** | Metode statis `rekeningPelajar()` digunakan untuk membuat rekening pelajar dengan saldo awal nol. Metode ini menggunakan `new static()` agar mendukung *late static binding*. |

## 4. Code Sebelum Diperbarui

Kode awal (template) berisi `TODO` yang harus dikerjakan. Klik untuk membuka.

### Java

<details>
<summary><code>Main.java</code> (sebelum)</summary>

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Jumlah rekening di awal: " + RekeningBank.getJumlahRekening());

        RekeningBank a = new RekeningBank("111", "Ani", 1_000_000);
        RekeningBank b = new RekeningBank("222", "Budi");        // constructor ringkas
        RekeningBank c = new RekeningBank("333", "Citra", 250_000);

        System.out.println(a);
        System.out.println(b);
        System.out.println(c);

        System.out.println("Jumlah rekening sekarang: " + RekeningBank.getJumlahRekening()
                           + "   (seharusnya 3, bukan 4)");

        System.out.println();
        System.out.println("=== Operasi ===");
        a.setor(500_000);
        System.out.println("Setelah setor 500.000  -> " + a);

        try {
            a.tarik(9_999_999);
            System.out.println("  MASALAH: penarikan melebihi batas seharusnya ditolak!");
        } catch (RuntimeException e) {
            System.out.println("  Ditolak: " + e.getMessage());
        }

        b.potongBiayaAdmin();
        System.out.println("Budi setelah potong admin: " + b + "   (saldo tidak boleh negatif)");

        System.out.printf("Bunga setahun dari saldo Ani: Rp%,.2f%n",
                RekeningBank.bungaSetahun(a.getSaldo()));
    }
}
```

</details>

<details>
<summary><code>RekeningBank.java</code> (sebelum)</summary>

```java
/**
 * Sesi 3 — constructor berdelegasi, anggota statis, dan konstanta.
 *
 * Invariant:
 *   1. saldo tidak pernah negatif
 *   2. nomor rekening tidak berubah setelah objek dibuat
 *   3. setoran dan penarikan selalu bernilai positif
 */
public class RekeningBank {

    // TODO 1: ganti tiga angka ajaib berikut menjadi konstanta bernama
    //         (public static final). Setelah itu, tidak boleh ada lagi
    //         angka literal di dalam badan method.
    //   - bunga tahunan          : 0.025
    //   - biaya administrasi     : 5000
    //   - batas penarikan sekali : 5000000

    // TODO 2: deklarasikan field statis penghitung jumlah rekening.
    //         Perhatikan: static, privat, dan bernilai awal 0.

    private final String nomor;
    private final String pemilik;
    private double saldo;

    /**
     * Constructor ringkas.
     * TODO 3: DELEGASIKAN ke constructor lengkap dengan this(...).
     *         Jangan menyalin validasi ke sini.
     */
    public RekeningBank(String nomor, String pemilik) {
        // TODO 3 — ganti baris di bawah dengan delegasi
        this.nomor = nomor;
        this.pemilik = pemilik;
        this.saldo = 0;
    }

    /** Constructor lengkap — SATU-SATUNYA tempat validasi berada. */
    public RekeningBank(String nomor, String pemilik, double saldoAwal) {
        // TODO 4: tolak nomor kosong dan saldo awal negatif.

        this.nomor = nomor;
        this.pemilik = pemilik;
        this.saldo = saldoAwal;

        // TODO 5: naikkan penghitung jumlah rekening DI SINI SAJA.
        //         Pikirkan mengapa bukan di kedua constructor.
    }

    public void setor(double jumlah) {
        // TODO 6: tolak jumlah <= 0, lalu tambahkan ke saldo.
    }

    public void tarik(double jumlah) {
        // TODO 7: tolak jumlah <= 0, tolak jika melebihi saldo,
        //         dan tolak jika melebihi batas penarikan sekali transaksi.
    }

    /** TODO 8: kurangi saldo sebesar biaya administrasi, tetapi jangan sampai negatif. */
    public void potongBiayaAdmin() {
    }

    /** TODO 9: method statis — kembalikan jumlah rekening yang pernah dibuat. */
    public static int getJumlahRekening() {
        return -1;   // ganti
    }

    /**
     * TODO 10: method statis utilitas — hitung bunga setahun dari pokok.
     *          Perhatikan: method ini tidak membaca keadaan objek mana pun.
     *          Itulah alasan ia pantas menjadi static.
     */
    public static double bungaSetahun(double pokok) {
        return 0;   // ganti
    }

    public double getSaldo()  { return saldo; }
    public String getNomor()  { return nomor; }

    @Override
    public String toString() {
        return String.format("Rekening[%s] %-14s Rp%,.2f", nomor, pemilik, saldo);
    }
}
```

</details>

### PHP

<details>
<summary><code>main.php</code> (sebelum)</summary>

```php
<?php
declare(strict_types=1);

require_once __DIR__ . '/RekeningBank.php';

echo 'Jumlah rekening di awal: ', RekeningBank::getJumlahRekening(), PHP_EOL;

$a = new RekeningBank('111', 'Ani', 1_000_000);
$b = RekeningBank::rekeningPelajar('222', 'Budi');   // named constructor
$c = new RekeningBank('333', 'Citra', 250_000);

echo $a, PHP_EOL, $b, PHP_EOL, $c, PHP_EOL;
echo 'Jumlah rekening sekarang: ', RekeningBank::getJumlahRekening(), '   (seharusnya 3)', PHP_EOL;

echo PHP_EOL, '=== Operasi ===', PHP_EOL;
$a->setor(500_000);
echo 'Setelah setor 500.000  -> ', $a, PHP_EOL;

try {
    $a->tarik(9_999_999);
    echo '  MASALAH: penarikan melebihi batas seharusnya ditolak!', PHP_EOL;
} catch (RuntimeException | InvalidArgumentException $e) {
    echo '  Ditolak: ', $e->getMessage(), PHP_EOL;
}

$b->potongBiayaAdmin();
echo 'Budi setelah potong admin: ', $b, '   (saldo tidak boleh negatif)', PHP_EOL;

printf('Bunga setahun dari saldo Ani: Rp%s%s',
    number_format(RekeningBank::bungaSetahun($a->getSaldo()), 2, ',', '.'), PHP_EOL);
```

</details>

<details>
<summary><code>RekeningBank.php</code> (sebelum)</summary>

```php
<?php
declare(strict_types=1);

/**
 * Sesi 3 — PHP tidak punya constructor overloading.
 * Padanannya: default parameter + named constructor (static factory).
 */
class RekeningBank
{
    // TODO 1: ganti angka ajaib berikut menjadi konstanta bernama.
    //   bunga tahunan 0.025 · biaya admin 5000 · batas penarikan 5000000

    // TODO 2: deklarasikan properti statis penghitung jumlah rekening.

    private float $saldo;

    /**
     * Default parameter menggantikan constructor overloading.
     * TODO 3: lengkapi validasi nomor kosong dan saldo awal negatif.
     * TODO 4: naikkan penghitung jumlah rekening.
     */
    public function __construct(
        private readonly string $nomor,
        private readonly string $pemilik,
        float $saldoAwal = 0,
    ) {
        $this->saldo = $saldoAwal;
    }

    /**
     * TODO 5: named constructor — rekening pelajar, saldo awal nol.
     *         Gunakan `new static()`, BUKAN `new self()`.
     *         Alasannya ada di modul teori pertemuan 3 (LateBinding.php).
     */
    public static function rekeningPelajar(string $nomor, string $pemilik): static
    {
        throw new RuntimeException('TODO 5 belum dikerjakan');
    }

    public function setor(float $jumlah): void
    {
        // TODO 6
    }

    public function tarik(float $jumlah): void
    {
        // TODO 7: tolak <= 0, tolak melebihi saldo, tolak melebihi batas sekali tarik.
    }

    /** TODO 8 */
    public function potongBiayaAdmin(): void
    {
    }

    /** TODO 9 */
    public static function getJumlahRekening(): int
    {
        return -1;   // ganti
    }

    /** TODO 10 */
    public static function bungaSetahun(float $pokok): float
    {
        return 0;   // ganti
    }

    public function getSaldo(): float { return $this->saldo; }
    public function getNomor(): string { return $this->nomor; }

    public function __toString(): string
    {
        return sprintf('Rekening[%s] %-14s Rp%s',
            $this->nomor, $this->pemilik, number_format($this->saldo, 2, ',', '.'));
    }
}
```

</details>

## 5. Implementasi Java

### `Main.java`

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Jumlah rekening di awal: " + RekeningBank.getJumlahRekening());

        RekeningBank a = new RekeningBank("111", "Ani", 1_000_000);
        RekeningBank b = new RekeningBank("222", "Budi");        // constructor ringkas
        RekeningBank c = new RekeningBank("333", "Citra", 250_000);

        System.out.println(a);
        System.out.println(b);
        System.out.println(c);

        System.out.println("Jumlah rekening sekarang: " + RekeningBank.getJumlahRekening()
                           + "   (seharusnya 3, bukan 4)");

        System.out.println();
        System.out.println("=== Operasi ===");
        a.setor(500_000);
        System.out.println("Setelah setor 500.000  -> " + a);

        try {
            a.tarik(9_999_999);
            System.out.println("  MASALAH: penarikan melebihi batas seharusnya ditolak!");
        } catch (RuntimeException e) {
            System.out.println("  Ditolak: " + e.getMessage());
        }

        b.potongBiayaAdmin();
        System.out.println("Budi setelah potong admin: " + b + "   (saldo tidak boleh negatif)");

        System.out.printf("Bunga setahun dari saldo Ani: Rp%,.2f%n",
                RekeningBank.bungaSetahun(a.getSaldo()));
    }
}
```

### `RekeningBank.java`

```java
/**
 * Sesi 3 — constructor berdelegasi, anggota statis, dan konstanta.
 *
 * Invariant:
 *   1. saldo tidak pernah negatif
 *   2. nomor rekening tidak berubah setelah objek dibuat
 *   3. setoran dan penarikan selalu bernilai positif
 */
public class RekeningBank {

    // TODO 1: ganti tiga angka ajaib berikut menjadi konstanta bernama
    //         (public static final). Setelah itu, tidak boleh ada lagi
    //         angka literal di dalam badan method.
    public static final double bunga_tahunan          = 0.025;
    public static final double biaya_administrasi     = 5000;
    public static final double batas_penarikan_sekali = 5000000;

    // TODO 2: deklarasikan field statis penghitung jumlah rekening.
    //         Perhatikan: static, privat, dan bernilai awal 0.
    private static int jumlahRekening = 0;

    private final String nomor;
    private final String pemilik;
    private double saldo;

    /**
     * Constructor ringkas.
     * TODO 3: DELEGASIKAN ke constructor lengkap dengan this(...).
     *         Jangan menyalin validasi ke sini.
     */
    public RekeningBank(String nomor, String pemilik) {
        // TODO 3 — ganti baris di bawah dengan delegasi
        this(nomor, pemilik,  0);
    }

    /** Constructor lengkap — SATU-SATUNYA tempat validasi berada. */
    public RekeningBank(String nomor, String pemilik, double saldoAwal) {
        // TODO 4: tolak nomor kosong dan saldo awal negatif.
        if (nomor == null || nomor.isEmpty()){
            throw new IllegalArgumentException( "Nomor Rekening Gak Boleh Kosong");
        }

        if (saldoAwal < 0){
            throw new IllegalArgumentException( "Saldo Awal tidak boleh negatif");

        }

        this.nomor = nomor;
        this.pemilik = pemilik;
        this.saldo = saldoAwal;

        // TODO 5: naikkan penghitung jumlah rekening DI SINI SAJA.
        //         Pikirkan mengapa bukan di kedua constructor.
        jumlahRekening++;
    }

    public void setor(double jumlah) {
        // TODO 6: tolak jumlah <= 0, lalu tambahkan ke saldo.
        if ( jumlah <= 0 ) {
            throw new IllegalArgumentException( "Jumlah setoran harus positif");
        }
        saldo +=jumlah;
    }

    public void tarik(double jumlah) {
        // TODO 7: tolak jumlah <= 0, tolak jika melebihi saldo,
        //         dan tolak jika melebihi batas penarikan sekali transaksi.
        if ( jumlah <=0){
            throw new IllegalArgumentException( "Jumlah penarikan harus positif");
        }
        if ( jumlah > saldo ) {
            throw new IllegalArgumentException( "Saldo tidak cukup");
        }
        if ( jumlah > batas_penarikan_sekali ) {
            throw new IllegalArgumentException( "Melibihi batas penarikan sekali transaksi");
        }
        saldo -= jumlah;
    }

    /** TODO 8: kurangi saldo sebesar biaya administrasi, tetapi jangan sampai negatif. */
    public void potongBiayaAdmin() {
        saldo = Math.max(0, saldo - biaya_administrasi);

    }

    /** TODO 9: method statis — kembalikan jumlah rekening yang pernah dibuat. */
    public static int getJumlahRekening() {
        return jumlahRekening;   // ganti
    }

    /**
     * TODO 10: method statis utilitas — hitung bunga setahun dari pokok.
     *          Perhatikan: method ini tidak membaca keadaan objek mana pun.
     *          Itulah alasan ia pantas menjadi static.
     */
    public static double bungaSetahun(double pokok) {
        return pokok  * bunga_tahunan;
    }

    public double getSaldo()  { return saldo; }
    public String getNomor()  { return nomor; }

    @Override
    public String toString() {
        return String.format("Rekening[%s] %-14s Rp%,.2f", nomor, pemilik, saldo);
    }
}
```

### Hasil Running Java

<img width="656" height="170" alt="Hasil running Java" src="https://github.com/user-attachments/assets/add7ec2f-a7b4-4eb4-a249-999c5476cc7e" />

## 6. Implementasi PHP

### `main.php`

```php
<?php
declare(strict_types=1);

require_once __DIR__ . '/RekeningBank.php';

echo 'Jumlah rekening di awal: ', RekeningBank::getJumlahRekening(), PHP_EOL;

$a = new RekeningBank('111', 'Ani', 1_000_000);
$b = RekeningBank::rekeningPelajar('222', 'Budi');   // named constructor
$c = new RekeningBank('333', 'Citra', 250_000);

echo $a, PHP_EOL, $b, PHP_EOL, $c, PHP_EOL;
echo 'Jumlah rekening sekarang: ', RekeningBank::getJumlahRekening(), '   (seharusnya 3)', PHP_EOL;

echo PHP_EOL, '=== Operasi ===', PHP_EOL;
$a->setor(500_000);
echo 'Setelah setor 500.000  -> ', $a, PHP_EOL;

try {
    $a->tarik(9_999_999);
    echo '  MASALAH: penarikan melebihi batas seharusnya ditolak!', PHP_EOL;
} catch (RuntimeException | InvalidArgumentException $e) {
    echo '  Ditolak: ', $e->getMessage(), PHP_EOL;
}

$b->potongBiayaAdmin();
echo 'Budi setelah potong admin: ', $b, '   (saldo tidak boleh negatif)', PHP_EOL;

printf('Bunga setahun dari saldo Ani: Rp%s%s',
    number_format(RekeningBank::bungaSetahun($a->getSaldo()), 2, ',', '.'), PHP_EOL);
```

### `RekeningBank.php`

```php
<?php
declare(strict_types=1);

class RekeningBank
{
    private const BUNGA_TAHUNAN = 0.025;
    private const BIAYA_ADMIN = 5000;
    private const BATAS_PENARIKAN = 5000000;

    private static int $jumlahRekening = 0;

    private float $saldo;

    public function __construct(
        private readonly string $nomor,
        private readonly string $pemilik,
        float $saldoAwal = 0,
    ) {
        if (trim($this->nomor) === '') {
            throw new InvalidArgumentException(
                'Nomor rekening tidak boleh kosong.'
            );
        }

        if (trim($this->pemilik) === '') {
            throw new InvalidArgumentException(
                'Nama pemilik tidak boleh kosong.'
            );
        }

        if ($saldoAwal < 0) {
            throw new InvalidArgumentException(
                'Saldo awal tidak boleh negatif.'
            );
        }

        $this->saldo = $saldoAwal;
        self::$jumlahRekening++;
    }

    public static function rekeningPelajar(
        string $nomor,
        string $pemilik
    ): static {
        return new static($nomor, $pemilik, 0);
    }

    public function setor(float $jumlah): void
    {
        if ($jumlah <= 0) {
            throw new InvalidArgumentException(
                'Jumlah setoran harus lebih dari nol.'
            );
        }

        $this->saldo += $jumlah;
    }

    public function tarik(float $jumlah): void
    {
        if ($jumlah <= 0) {
            throw new InvalidArgumentException(
                'Jumlah penarikan harus lebih dari nol.'
            );
        }

        if ($jumlah > self::BATAS_PENARIKAN) {
            throw new InvalidArgumentException(
                'Penarikan melebihi batas sekali tarik.'
            );
        }

        if ($jumlah > $this->saldo) {
            throw new InvalidArgumentException(
                'Saldo tidak mencukupi.'
            );
        }

        $this->saldo -= $jumlah;
    }

    public function potongBiayaAdmin(): void
    {
        $this->saldo -= self::BIAYA_ADMIN;
    }

    public static function getJumlahRekening(): int
    {
        return self::$jumlahRekening;
    }

    public static function bungaSetahun(float $pokok): float
    {
        if ($pokok < 0) {
            throw new InvalidArgumentException(
                'Pokok tabungan tidak boleh negatif.'
            );
        }

        return $pokok * self::BUNGA_TAHUNAN;
    }

    public function getSaldo(): float
    {
        return $this->saldo;
    }

    public function getNomor(): string
    {
        return $this->nomor;
    }

    public function __toString(): string
    {
        return sprintf(
            'Rekening[%s] %-14s Rp%s',
            $this->nomor,
            $this->pemilik,
            number_format($this->saldo, 2, ',', '.')
        );
    }
}
```

### Hasil Running PHP

<img width="705" height="170" alt="Hasil running PHP" src="https://github.com/user-attachments/assets/d4c9a019-513b-4641-bd73-5456cefb4aa5" />

## 7. Pengujian Program

Pengujian dilakukan untuk memastikan bahwa:

1. Jumlah rekening bertambah satu kali untuk setiap objek yang berhasil dibuat.
2. Constructor ringkas atau named constructor membuat rekening dengan saldo awal nol.
3. Setoran dan penarikan dengan jumlah tidak valid ditolak.
4. Penarikan yang melebihi saldo atau batas transaksi ditolak.
5. Biaya administrasi tidak membuat saldo menjadi negatif.
6. Bunga tahunan dihitung berdasarkan saldo yang diberikan.

Hasil akhir pengujian mengikuti implementasi program yang dijalankan (lihat screenshot pada bagian **Hasil Running Java** dan **Hasil Running PHP**).

## 8. Kesimpulan

Praktikum ini menunjukkan penggunaan constructor, konstanta, anggota statis, validasi, dan invariant dalam pemrograman berorientasi objek. Java menerapkan constructor berdelegasi dengan `this(...)`, sedangkan PHP menggunakan default parameter dan named constructor berbasis metode statis. Dengan validasi yang terpusat, aturan rekening dapat dijaga agar transaksi yang tidak valid ditolak dan saldo tidak menjadi negatif.
