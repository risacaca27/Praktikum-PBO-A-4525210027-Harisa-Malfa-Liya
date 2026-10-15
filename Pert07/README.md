# Praktikum Sesi 7 — Pemodelan Relasi Objek dan UML

**Sub-CPMK-P5 — Mahasiswa mampu menerjemahkan UML class diagram menjadi kode yang konsisten dan sebaliknya. (P3, C4)**

**Keterkaitan teori:** Pertemuan 7 — Relasi Objek dan UML Class Diagram  
**Durasi:** 170 menit (1 SKS praktikum) · **Bobot:** 6% dari nilai praktikum  
**Bahasa:** Java 25 dan PHP 8.4+

> Versi cetak modul ini: [`Modul-Praktikum-07-pemodelan-relasi-objek-dan-uml.docx`](Modul-Praktikum-07-pemodelan-relasi-objek-dan-uml.docx). Kerangka kode: [`starter/`](starter/).

---

## A. Tujuan sesi

1. Menganalisis kebutuhan Sistem Perpustakaan menjadi model objek.
2. Membuat use case diagram, class diagram, dan sequence diagram dengan PlantUML.
3. Menerjemahkan class diagram menjadi kerangka kode yang konsisten.
4. Membedakan agregasi dan komposisi melalui wujudnya di kode, bukan hanya di gambar.

## B. Alat dan bahan

- Lingkungan hasil sesi 1.
- PlantUML terpasang dan bisa merender.

## C. Berkas starter

Salin ke folder tugas Anda; **jangan** menyunting berkas aslinya. Setiap komentar `TODO` harus Anda lengkapi sendiri — kode yang disalin dari sumber lain akan terlihat pada sesi demo.

| Berkas | Keterangan |
|---|---|
| `starter/uml/perpustakaan-kerangka.puml` | Kerangka class diagram dengan bagian yang harus dilengkapi. |
| `starter/java/` | Kerangka paket model dan service dengan TODO. |
| `starter/php/` | Padanan PHP. |
| `starter/deskripsi-kebutuhan.md` | Deskripsi kebutuhan Sistem Perpustakaan yang harus dianalisis. |

## D. Langkah kerja

Kerjakan berurutan. Jangan melanjutkan ke langkah berikutnya sebelum checkpoint terpenuhi.

### Langkah 1 — Analisis kebutuhan

- Baca deskripsi-kebutuhan.md dengan saksama. Deskripsi ini akan menjadi dasar seluruh pekerjaan Anda sampai sesi 14.
- Buat sesi-07/analisis.md berisi: daftar kata benda (calon kelas), daftar kata kerja (calon method), dan daftar aturan bisnis (invariant).
- Coret kata benda yang ternyata hanya atribut, bukan kelas. Tuliskan alasan pencoretannya.

> **Checkpoint —** analisis.md memuat minimal enam calon kelas dan minimal lima aturan bisnis.

### Langkah 2 — Use case diagram

- Buat sesi-07/uml/usecase.puml dengan minimal dua aktor (Anggota, Petugas) dan enam use case.
- Gunakan relasi include atau extend setidaknya satu kali, dan jelaskan pilihannya di analisis.md.

**Terminal**

```bash
plantuml -tsvg sesi-07/uml/usecase.puml
```

> **Checkpoint —** Diagram ter-render dan menampilkan aktor serta batas sistem.

### Langkah 3 — Class diagram — bagian tersulit

- Salin perpustakaan-kerangka.puml ke sesi-07/uml/class.puml dan lengkapi.
- Untuk SETIAP relasi, tentukan derajat keterikatannya dan tuliskan multiplicity di kedua ujungnya.
- Minimal harus ada: satu komposisi, satu agregasi, satu asosiasi, dan satu dependensi.
- Uji setiap relasi dengan pertanyaan: "Kalau induknya dihapus, apakah bagian ini masih masuk akal?"

> **Checkpoint —** class.puml memuat keempat jenis relasi dengan notasi dan multiplicity yang benar.

### Langkah 4 — Sequence diagram untuk satu alur

- Buat sesi-07/uml/sequence-pinjam.puml yang menggambarkan alur peminjaman buku dari Anggota sampai data tersimpan.
- Tampilkan minimal empat objek dan satu alur alternatif (misalnya buku tidak tersedia).

> **Checkpoint —** Diagram menampilkan pesan bolak-balik antar objek, bukan sekadar daftar langkah.

### Langkah 5 — Menerjemahkan diagram menjadi kode

- Salin kerangka java/ dan php/ ke sesi-07/. Lengkapi TODO sehingga strukturnya SAMA PERSIS dengan class diagram Anda.
- Aturan yang wajib dipatuhi:
  • Komposisi: objek bagian dibuat DI DALAM induk, dan getter mengembalikan salinan tak-berubah.
  • Agregasi: objek bagian diterima dari luar lewat constructor atau method.
  • Multiplicity 1..*: constructor menolak koleksi kosong.
- Isi method boleh minimal, tetapi struktur, tipe, dan tanda tangan harus lengkap.

**Terminal**

```bash
cd sesi-07/java
javac -d out $(find . -name "*.java")
java -cp out Main
```

> **Checkpoint —** Program berjalan, dan setiap kelas di diagram ada di kode dengan nama yang sama persis.

### Langkah 6 — Membuktikan komposisi benar-benar komposisi

- Buat sesi-07/bukti-relasi.md.
- Untuk relasi komposisi Anda, tulis kode di Main yang MENCOBA mengubah koleksi internal dari luar. Catat hasilnya.
- Untuk relasi agregasi, tunjukkan bahwa objek bagian tetap hidup setelah dilepas dari induknya.

> **Checkpoint —** bukti-relasi.md menunjukkan koleksi komposisi menolak diubah, dan objek agregasi tetap ada setelah dilepas.

## E. Latihan mandiri di lab

_Dikerjakan bila langkah kerja selesai lebih awal. Tidak wajib, tetapi menambah nilai pada aspek penerapan konsep._

1. Ambil satu relasi yang Anda tandai agregasi, lalu ubah menjadi komposisi. Catat berapa baris kode yang berubah dan mengapa.
2. Buat activity diagram untuk alur pengembalian buku, termasuk cabang perhitungan denda.

## F. Tugas rumah

1. TUGAS 2 (mata kuliah teori): modelkan Sistem Presensi Kuliah dengan minimal enam kelas, mencakup satu komposisi, satu agregasi, dan satu asosiasi, lengkap dengan multiplicity. Implementasikan kerangka kodenya di Java DAN PHP.
2. Untuk setiap relasi pada tugas tersebut, tulis satu kalimat justifikasi: mengapa agregasi dan bukan komposisi, atau sebaliknya.
3. Persiapan sesi 8: pelajari kembali seluruh materi sesi 1–7. Ujian praktik tengah semester berbentuk live coding di hadapan dosen.

## G. Luaran yang dikumpulkan

Seluruh luaran diserahkan melalui repositori Git pribadi Anda, dengan riwayat commit yang menunjukkan proses pengerjaan — bukan satu commit tunggal di akhir.

| Berkas / folder | Keterangan |
|---|---|
| `sesi-07/analisis.md` | Analisis kata benda, kata kerja, aturan bisnis, dan alasan pencoretan. |
| `sesi-07/uml/` | usecase.puml, class.puml, sequence-pinjam.puml beserta hasil render. |
| `sesi-07/java/ dan sesi-07/php/` | Kerangka kode Sistem Perpustakaan yang konsisten dengan diagram. |
| `sesi-07/bukti-relasi.md` | Bukti bahwa komposisi dan agregasi diterapkan benar di kode. |

## H. Rubrik penilaian sesi ini

| Aspek | Bobot | Kriteria |
|---|---|---|
| Kebenaran fungsional | 35% | Seluruh perintah pada langkah kerja berjalan dan menghasilkan keluaran yang diminta. |
| Penerapan konsep sesi ini | 30% | Konsep yang menjadi Sub-CPMK sesi ini diterapkan dengan tepat, bukan sekadar membuat program berjalan. |
| Keterbacaan dan konvensi | 10% | Penamaan bermakna, format konsisten, komentar seperlunya, riwayat commit wajar. |
| Demo dan pertanyaan lisan | 25% | Mampu menjelaskan setiap baris kode sendiri dan menjawab pertanyaan demo. MENGGUGURKAN: tanpa demo, tugas tidak dinilai. |

## I. Pertanyaan demo

> Pertanyaan berikut akan diajukan saat Anda mendemokan pekerjaan sesi ini. Daftar ini sengaja dibuka agar Anda mempersiapkan **pemahaman**, bukan hafalan. Anda boleh memakai alat bantu apa pun saat mengerjakan — tetapi kode yang tidak dapat Anda jelaskan sendiri tidak dinilai.

1. Tunjuk satu komposisi di diagram Anda, lalu tunjuk baris kode yang mewujudkannya. Apa yang membuatnya komposisi dan bukan agregasi?
2. Kalau saya menghapus objek induk pada relasi komposisi Anda, apa yang terjadi pada bagiannya? Tunjukkan di kode.
3. Mengapa getter untuk koleksi komposisi mengembalikan salinan? Apa yang terjadi kalau tidak?
4. Di diagram Anda tertulis multiplicity 1..*. Baris kode mana yang menjamin angka minimalnya satu?
5. Ada kelas di diagram Anda yang tidak ada di kode, atau sebaliknya? Kita periksa satu per satu.
6. Pada sequence diagram, objek mana yang memutuskan bahwa peminjaman ditolak? Mengapa bukan objek yang lain?
7. Saya ingin menambah aturan: satu anggota maksimum meminjam tiga buku. Di diagram, itu muncul di mana? Di kode, di mana?

## J. Kesalahan yang sering terjadi

| Gejala | Penyebab yang lazim | Cara memperbaiki |
|---|---|---|
| Semua relasi digambar dengan garis polos | Belum membedakan derajat keterikatan. | Tentukan tiap relasi dengan pertanyaan "kalau induknya dihapus...". Gunakan *-- , o-- , --> , ..> sesuai hasilnya. |
| Multiplicity tidak ditulis | Dianggap detail yang bisa ditambahkan nanti. | Multiplicity adalah aturan bisnis. Tanpa itu, model kehilangan informasi penting dan tidak bisa diterjemahkan ke validasi. |
| Diagram menyebut komposisi, tetapi getter membocorkan koleksi | Diagram dibuat menurut niat, kode ditulis menurut kebiasaan. | Kembalikan List.copyOf(...) di Java; jadikan objek bagian readonly di PHP. |
| PlantUML gagal merender | Kesalahan sintaks, atau Graphviz belum terpasang. | Periksa pesan kesalahannya; untuk diagram tertentu PlantUML membutuhkan Graphviz (dot). |
| Kompilasi Java gagal karena struktur folder tidak cocok dengan package | Nama package tidak sama dengan jalur folder. | Samakan keduanya, atau kompilasi dengan menyebut seluruh berkas: javac -d out $(find . -name "*.java"). |

## Lembar verifikasi demo

Diisi oleh dosen atau asisten pada saat demo. Tugas tanpa lembar terverifikasi tidak dinilai.

|  |  |
|---|---|
| Nama / NIM |  |
| Tanggal demo |  |
| Nilai sesi ini |  |
| Catatan penguji |  |
| Paraf penguji |  |

---

_Modul ini disusun 10 September 2026. Versi teknologi yang dirujuk (Java 25 LTS, PHP 8.4/8.5, Laravel 13, Spring Boot 4.1) diverifikasi pada tanggal tersebut dan perlu diperiksa ulang sebelum semester berjalan._
