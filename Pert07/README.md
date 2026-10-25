# LAPORAN PRAKTIKUM PBO
## Sesi 7 — Pemodelan Relasi Objek dan UML

**Nama:** Harisa Malfa Liya  
**NPM:** 4525210027  
**Mata Kuliah:** Pemrograman Berorientasi Objek (PBO)

---

## 1. Tujuan Praktikum

Praktikum ini bertujuan untuk memahami relasi antarobjek, khususnya komposisi dan agregasi, serta menerapkan konsep tersebut ke dalam program menggunakan Java dan PHP.

## 2. Implementasi Program

### 2.1 Implementasi Java

Implementasi Java terdapat pada folder `starter/java/`.

File yang digunakan:
- 
import model.Buku;
import model.Jurusan;

/**
 * Kompilasi dari folder sesi-07/java:
 *     javac -d out $(find . -name "*.java")
 *     java -cp out Main
 */
public class Main {
    public static void main(String[] args) {

        System.out.println("=== Komposisi: Buku memiliki Bab ===");
        Buku buku = new Buku("PBO-001", "Pemrograman Berorientasi Objek", "A. W. Pribadi", 3);
        buku.tambahBab(1, "Paradigma Objek");
        buku.tambahBab(2, "Enkapsulasi");
        buku.tambahBab(3, "Pewarisan dan Polimorfisme");
        System.out.println("  " + buku);
        buku.getBab().forEach(b -> System.out.println("    " + b));

        // Percobaan bukti-relasi.md nomor 1:
        System.out.println();
        System.out.println("  Mencoba mengubah koleksi internal dari luar...");
        try {
            buku.getBab().clear();
            System.out.println("    MASALAH: koleksi internal berhasil dikosongkan dari luar!");
            System.out.println("    Berarti getBab() Anda belum mengembalikan salinan.");
        } catch (UnsupportedOperationException e) {
            System.out.println("    Ditolak — koleksi bab tidak bisa diubah dari luar. Benar.");
        }

        System.out.println();
        System.out.println("=== Agregasi: Jurusan menaungi Mahasiswa ===");
        Jurusan.Mahasiswa ani  = new Jurusan.Mahasiswa("2024001", "Ani Lestari");
        Jurusan.Mahasiswa budi = new Jurusan.Mahasiswa("2024002", "Budi Santoso");

        Jurusan informatika = new Jurusan("TI", "Teknik Informatika");
        informatika.daftarkan(ani);
        informatika.daftarkan(budi);
        System.out.println("  " + informatika);

        informatika.keluarkan(budi);
        System.out.println("  Setelah Budi pindah: " + informatika);
        System.out.println("  Budi tetap ada sebagai objek: " + budi);
        System.out.println();
        System.out.println("  Itulah bedanya: Bab tidak bisa hidup di luar Buku,");
        System.out.println("  tetapi Mahasiswa tetap ada di luar Jurusan.");
    }
}
`
- File kelas yang mendukung implementasi relasi objek.

**Kode implementasi Java:**


### 2.2 Implementasi PHP

Implementasi PHP terdapat pada folder `starter/php/`.

File yang digunakan:
- `<?php
declare(strict_types=1);

/**
 * Sesi 7 — komposisi dan agregasi (PHP).
 * Bandingkan dengan versi Java.
 */
enum JenisKeanggotaan: string{
    case MAHASISWA = 'MAHASISWA';

    case DOSEN = 'DOSEN';

    case UMUM = 'UMUM';
}

enum StatusKoleksi: string {
    case TERSEDIA = 'TERSEDIA';

    case DIPINJAM = 'DIPINJAM';

    case HILANG = 'HILANG';
}

final class Bab
{
    /**
     * TODO 1: buat constructor readonly untuk $nomor dan $judul.
     *         Perhatikan: agar benar-benar komposisi, kelas ini hanya
     *         boleh dibuat dari dalam Buku. PHP tidak punya nested class
     *         seperti Java, jadi disiplin ini dijaga lewat konvensi —
     *         tuliskan catatannya di bukti-relasi.md.
     */
    private int $nomor;

    private string $judul;

    public function __construct(int $nomor, string $judul)
    {   
        $this->nomor = $nomor;
        $this->judul = $judul;
    }

    public function getNomor():int{
        return $this->nomor;
    }

    public function getJudul():string{
        return $this->judul;
    }

    public function __toString(): string{
        return sprintf('Bab %d: %s', $this->nomor, $this->judul);
    }
}

final class Buku
{
    private array $bab = [];

    private string $nomorInduk;

    private string $judul;

    private string $penulis;

    private int $jumlahEksemplar;

    private StatusKoleksi $status;

    public function __construct(
        string $nomorInduk,
        string $judul,
        string $penulis,
        int $jumlahEksemplar,
        StatusKoleksi $status = StatusKoleksi::TERSEDIA
    ) {
        // TODO 2: tolak jumlah eksemplar negatif.

        if($jumlahEksemplar < 0){
            throw new InvalidArgumentException("Jumlah eksemplar tidak boleh negatif");
        }
        $this->nomorInduk = $nomorInduk;
             $this->judul = $judul;
                  $this->penulis= $penulis;
                       $this->jumlahEksemplar = $jumlahEksemplar;
                            $this->status = $status;
    }

    /** TODO 3: satu-satunya cara Bab lahir. Parameternya nomor dan judul, bukan objek Bab. */
    public function tambahBab(int $nomor, string $judulBab): void
    {
        $this->bab[] = new Bab($nomor, $judulBab);
    }

    /**
     * PHP mengembalikan array secara by-value, sehingga pemanggil hanya
     * memperoleh salinan larik-nya. Namun objek DI DALAMNYA tetap berbagi
     * referensi — itulah sebabnya Bab dibuat readonly.
     * Tuliskan penjelasan ini di bukti-relasi.md.
     *
     * @return Bab[]
     */
    public function getBab(): array { return $this->bab; }

    public function tersedia(): bool { return $this->jumlahEksemplar > 0; }

    /** TODO 4 */
    public function kurangiStok(): void {
        if(!this->tersedia()){
            throw new UnderflowException("Stok Buku Habis");
        }
        $this->jumlahEksemplar--;
        if($this->jumlahEksemplar == 0){
            $this->status = StatusKoleksi::DIPINJAM;
        }
    }

    public function tambahStok(): void { $this->jumlahEksemplar++;
    $this->status = StatusKoleksi::TERSEDIA;
    }

    public function getNomorInduk(): string { return $this->nomorInduk; }
    public function getJudul(): string { return $this->judul; }

    public function getStatus():StatusKoleksi{
        return $this->status;
    }

    public function __toString(): string
    {
        return sprintf('%-32s %-16s [%d eks, %d bab]',
            $this->judul, $this->penulis, $this->jumlahEksemplar, count($this->bab), $this->status->value);
    }
}

final class Mahasiswa
{
    private string $nim;

    private string $nama;

    private JenisKeanggotaan $jenis;

    public function __construct(
        string $nim,
        string $nama,
        JenisKeanggotaan $jenis = JenisKeanggotaan::MAHASISWA
    ) {
        $this->nim = $nim;
             $this->nama = $nama;
                  $this->jenis = $jenis;
    }

    public function getNim(): string { return $this->nim; }
    public function getNama(): string {
        return $this->nama;
    }
    public function getJenis(): JenisKeanggotaan{
        return $this->jenis;
    }
    public function __toString(): string { 
        return sprintf('%s %s (%s)', $this->nim, $this->nama, $this->jenis->value);
    
    }
}

final class Jurusan
{
    private array $mahasiswa = [];

    private string $kode;

    private string $nama;

    public function __construct(string $kode, string $nama) {
        $this->kode = $kode;
        $this->nama = $nama;
    }

    /** TODO 5: terima objek dari luar (agregasi). Jangan menambahkan duplikat. */
    public function daftarkan(Mahasiswa $m): void {
        foreach($this->mahasiswa as $existing){
            if($existing->getNim()== $m->getNim()){
                return;
            }
        }
        $this->mahasiswa[] = $m;
    }

    /** TODO 6: lepaskan dari jurusan. Objeknya tetap hidup. */
    public function keluarkan(Mahasiswa $m): void {
        foreach ($this->mahasiswa as $i => $existing) {
            if($existing->getNim() == $m->getNim()){
                unset($this->mahasiswa[$i]);
                $this->mahasiswa = array_values($this->mahasiswa);
                return;
            }
        }
    }

    /** @return Mahasiswa[] */
    public function getMahasiswa(): array { return $this->mahasiswa; }

    public function __toString(): string
    {
        return sprintf('%s - %s (%d mahasiswa)', $this->kode, $this->nama, count($this->mahasiswa));
    } `
    
- `<?php
declare(strict_types=1);`

`require_once __DIR__ . '/relasi.php';`

`echo '=== Komposisi: Buku memiliki Bab ===', PHP_EOL;
$buku = new Buku('PBO-001', 'Pemrograman Berorientasi Objek', 'A. W. Pribadi', 3);
$buku->tambahBab(1, 'Paradigma Objek');
$buku->tambahBab(2, 'Enkapsulasi');
$buku->tambahBab(3, 'Pewarisan dan Polimorfisme');
echo '  ', $buku, PHP_EOL;
foreach ($buku->getBab() as $b) {
    echo '    ', $b, PHP_EOL;
}`

`echo PHP_EOL, '=== Agregasi: Jurusan menaungi Mahasiswa ===', PHP_EOL;
$ani  = new Mahasiswa('2024001', 'Ani Lestari');
$budi = new Mahasiswa('2024002', 'Budi Santoso');`

`$informatika = new Jurusan('TI', 'Teknik Informatika');
$informatika->daftarkan($ani);
$informatika->daftarkan($budi);
echo '  ', $informatika, PHP_EOL;`

`$informatika->keluarkan($budi);
echo '  Setelah Budi pindah: ', $informatika, PHP_EOL;
echo '  Budi tetap ada sebagai objek: ', $budi, PHP_EOL;`


## 3. Hasil Running Program

### 3.1 Hasil Running Java

<img width="490" height="226" alt="Screenshot 2026-10-25 at 12 14 24" src="https://github.com/user-attachments/assets/6e4f2a3b-5ef1-417d-8c0e-d1032e79c10f" />



Program Java berhasil dijalankan dan menampilkan hasil implementasi komposisi serta agregasi.

### 3.2 Hasil Running PHP

<img width="503" height="141" alt="Screenshot 2026-10-25 at 12 28 40" src="https://github.com/user-attachments/assets/9186b875-3900-4f31-ab8d-eca77d65a92b" />


Program PHP berhasil dijalankan tanpa error. Informasi buku dan bab ditampilkan dengan benar, serta objek mahasiswa tetap ada setelah dilepaskan dari jurusan.

## 4. Analisis

### 4.1 Komposisi

Komposisi merupakan relasi ketika objek bagian bergantung pada objek induknya. Pada program ini, buku memiliki beberapa bab yang dibuat melalui objek buku. Koleksi bab tidak dapat diubah secara langsung dari luar.

### 4.2 Agregasi

Agregasi merupakan relasi ketika objek bagian tetap dapat berdiri sendiri meskipun sudah tidak terhubung dengan objek induknya. Pada program ini, mahasiswa tetap ada sebagai objek setelah dikeluarkan dari jurusan.

## 5. Kesimpulan

Berdasarkan praktikum yang telah dilakukan, komposisi dan agregasi memiliki perbedaan dalam ketergantungan objek. Komposisi menunjukkan hubungan yang kuat antara objek induk dan bagian, sedangkan agregasi memungkinkan objek bagian tetap hidup secara mandiri. Implementasi pada Java dan PHP berhasil dijalankan untuk menunjukkan kedua konsep tersebut.
