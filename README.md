<div align="center">
# FRSIS
 
**Buku alamat berbasis web untuk kurir paket**
Simpan alamat sulit dicari beserta foto rumah, patokan, dan petunjuk arah.
 
![Single file](https://img.shields.io/badge/arsitektur-single--file-306230)
![Dependencies](https://img.shields.io/badge/dependensi-0-306230)
![Storage](https://img.shields.io/badge/penyimpanan-lokal%20(IndexedDB)-306230)
![Language](https://img.shields.io/badge/bahasa-Indonesia-306230)
 
</div>
---
 
## Daftar Isi
 
- [Ringkasan](#ringkasan)
- [Fitur Utama](#fitur-utama)
- [Memulai](#memulai)
- [Panduan Penggunaan](#panduan-penggunaan)
- [Impor Data J&T](#impor-data-jt)
- [Model Data](#model-data)
- [Cadangan dan Pemulihan](#cadangan-dan-pemulihan)
- [Privasi dan Keamanan Data](#privasi-dan-keamanan-data)
- [Persyaratan Teknis](#persyaratan-teknis)
- [Struktur Proyek](#struktur-proyek)
- [Keterbatasan](#keterbatasan)
- [Lisensi](#lisensi)
## Ringkasan
 
Kurir sering menghadapi alamat yang sulit ditemukan: gang sempit, kompleks tanpa penanda jelas, atau rumah tanpa nomor. FRSIS membantu mendokumentasikan alamat tersebut sekali, lalu memakainya kembali pada pengantaran berikutnya.
 
Seluruh aplikasi dikemas dalam **satu berkas HTML** (`FRSIS.html`). Tidak ada server, akun pengguna, maupun proses build. Data disimpan di perangkat pengguna.
 
## Fitur Utama
 
| Kategori | Kemampuan |
|---|---|
| **Pencatatan alamat** | Nama penerima/tempat, alamat, area atau kompleks, ciri rumah, patokan, petunjuk arah berurutan, nomor HP, tautan peta, dan catatan khusus |
| **Dokumentasi visual** | Hingga 4 foto per alamat dari kamera atau galeri, dikompresi otomatis, dengan tampilan perbesar |
| **Pencarian dan filter** | Pencarian teks pada seluruh bidang, filter per area dengan penghitung, dan empat mode urutan (Terbaru, Nama A-Z, Per area, Sering diantar) |
| **Aksi cepat** | Buka navigasi Google Maps, kirim pesan WhatsApp, salin alamat, salin petunjuk arah, dan catat pengantaran |
| **Pelacakan pengantaran** | Jumlah kunjungan dan tanggal pengantaran terakhir per alamat |
| **Impor massal** | Dari teks template J&T, tabel, atau berkas CSV/TSV/TXT/XLSX/XLS, dengan deteksi duplikat |
| **Cadangan** | Ekspor dan impor seluruh data dalam format JSON |
| **Asisten** *(opsional)* | Perintah cepat di perangkat, serta fitur berbasis AI bila lingkungan menyediakannya |
| **Antarmuka** | Tema terang dan gelap otomatis, tata letak satu kolom untuk ponsel, dukungan *safe area* dan *reduced motion* |
 
## Memulai
 
1. Unduh atau salin `FRSIS.html` ke perangkat.
2. Buka berkas tersebut di browser modern.
3. Ketuk **Tambah alamat** untuk menyimpan alamat pertama.
### Pemasangan sebagai aplikasi (opsional)
 
Unggah `FRSIS.html` ke layanan hosting statis (misalnya GitHub Pages atau Netlify), buka tautannya di ponsel, lalu pilih **Tambahkan ke Layar Utama** pada menu browser.
 
## Panduan Penggunaan
 
**Menambah alamat.** Ketuk *Tambah alamat*, lalu isi nama, alamat, dan bidang pendukung. Isian *Patokan* ditulis satu per baris, sedangkan *Petunjuk arah* ditulis satu langkah per baris sesuai urutan.
 
**Mencari alamat.** Gunakan kolom pencarian atau pilih area pada deretan filter. Urutan daftar dapat diubah melalui menu urutan.
 
**Mengantar paket.** Ketuk kartu alamat untuk membuka foto, patokan, dan petunjuk arah. Gunakan *Buka navigasi* untuk rute, lalu catat pengantaran setelah paket diterima.
 
## Impor Data J&T
 
Aplikasi ini tidak terhubung langsung ke sistem J&T. Data dimasukkan dengan menyalin teks atau memilih berkas ekspor melalui tombol **Impor J&T**.
 
| Format masukan | Keterangan |
|---|---|
| Pesan template COD | Teks berawalan "Halo …, kami dari J&T Express …"; dapat berisi beberapa pesan sekaligus |
| Tabel berjudul kolom | CSV, TSV, XLSX, atau XLS; pemetaan kolom dapat disesuaikan secara manual |
| Teks berlabel | Baris seperti `Penerima:`, `Alamat:`, `No HP:`, dengan pemisah baris kosong antarpenerima |
| Teks bebas | Dikenali secara heuristik; dapat dibantu fitur AI bila tersedia |
 
Ketentuan impor:
 
- Nomor resi dan nominal COD dikenali agar kolom tidak salah terbaca, tetapi **tidak disimpan**.
- Alamat yang sudah ada ditandai *Sudah tersimpan* dan hanya diperbarui bila disertai gambar baru.
- Baris tanpa alamat tidak diimpor.
- Nomor telepon dinormalisasi ke format lokal (`+62` dan `62` menjadi `0`).
## Model Data
 
Setiap alamat disimpan sebagai satu objek dengan struktur berikut:
 
| Bidang | Tipe | Deskripsi |
|---|---|---|
| `id` | string | Pengenal unik |
| `nama` | string | Nama penerima atau tempat |
| `alamat` | string | Alamat lengkap |
| `area` | string | Area atau kompleks untuk pengelompokan |
| `ciri` | string | Ciri fisik rumah |
| `patokan` | string | Patokan, satu per baris |
| `arah` | string | Petunjuk arah, satu langkah per baris |
| `telp` | string | Nomor HP |
| `maps` | string | Tautan atau koordinat peta |
| `catatan` | string | Catatan khusus |
| `foto` | string[] | Maksimal 4 gambar (data URL JPEG) |
| `kunjungan` | number | Jumlah pengantaran |
| `lastVisit` | number | Waktu pengantaran terakhir (epoch ms) |
| `updated` | number | Waktu pembaruan terakhir (epoch ms) |
 
## Cadangan dan Pemulihan
 
Buka **Cadangan** untuk menyimpan data ke berkas `frsis-cadangan-YYYY-MM-DD.json` atau memulihkannya dari berkas cadangan. Berkas yang bukan cadangan FRSIS yang valid akan ditolak.
 
```json
{
  "app": "frsis",
  "versi": 1,
  "dibuat": "2026-11-23T00:00:00.000Z",
  "places": [ { "id": "…", "nama": "…", "alamat": "…" } ]
}
```
 
> **Rekomendasi:** lakukan pencadangan secara berkala, terutama sebelum mengganti perangkat atau membersihkan data browser.
 
## Privasi dan Keamanan Data
 
- Data alamat dan foto disimpan **secara lokal** pada IndexedDB browser (database `patokan`). Tidak ada data yang dikirim ke server oleh fungsi inti aplikasi.
- Preferensi ringan (urutan daftar dan waktu cadangan terakhir) disimpan di `localStorage`.
- Fitur AI (bila tersedia) mengirim data yang relevan ke layanan online untuk diproses. Gunakan dengan memperhatikan sensitivitas data penerima.
- Jika penyimpanan browser tidak tersedia, misalnya pada mode privat, aplikasi memberi peringatan dan data akan hilang saat halaman ditutup.
- Seluruh teks pengguna di-*escape* sebelum ditampilkan untuk mencegah penyisipan HTML.
## Persyaratan Teknis
 
| Komponen | Keterangan |
|---|---|
| Browser | Modern, mendukung IndexedDB, FileReader, dan Canvas |
| Internet | Opsional; dibutuhkan hanya untuk hal berikut |
| Font | *Press Start 2P* dan *Pixelify Sans* (Google Fonts), dengan cadangan monospace |
| Impor Excel | [SheetJS](https://sheetjs.com/) 0.18.5 dari cdnjs, dimuat saat diperlukan |
| Navigasi dan WhatsApp | Membuka Google Maps dan `wa.me` di tab baru |
 
Tanpa framework, tanpa *package manager*, dan tanpa langkah instalasi.
 
## Struktur Proyek
 
```
.
├── FRSIS.html    # Aplikasi lengkap (HTML, CSS, JavaScript)
└── README.md     # Dokumentasi
```
 
## Keterbatasan
 
- Tidak ada sinkronisasi antarperangkat; pemindahan data dilakukan lewat cadangan dan pemulihan.
- Maksimal 4 foto per alamat.
- Tidak ada integrasi langsung dengan aplikasi atau API J&T.
- Pembacaan berkas Excel memerlukan koneksi internet; gunakan CSV untuk penggunaan offline.
## Lisensi
 
Belum ditentukan. Tambahkan berkas `LICENSE` sebelum distribusi publik.
 
