# Candrawama Sastra

**Candrawama Sastra** adalah aplikasi perpustakaan sekolah berbasis web untuk mengelola koleksi buku, keanggotaan, peminjaman-pengembalian buku, denda keterlambatan, serta rating & ulasan buku — dengan tiga tingkat akses pengguna: **Admin**, **Petugas**, dan **Siswa/Anggota**.

Dibangun dengan **PHP Native (MySQLi)**, **MySQL**, **Bootstrap 5**, **Tailwind CSS**, dan **JavaScript**.

**Live Demo:** [https://perpustakaandigitalilmu.infinityfreeapp.com](https://perpustakaandigitalilmu.infinityfreeapp.com)

**Mockup:** [Lihat Mockup](https://raw.githubusercontent.com/Annajwaannuryaqni/readme-perpustakaandigitalilmu/refs/heads/main/mockup_ukk.jpg)

**Use Case Diagram:** [Lihat Use Case Diagram](https://raw.githubusercontent.com/ReniUmiKhasanah/readme.md_perpustakaan/refs/heads/main/usecase.jpg)

**Flowchart:** [Lihat Flowchart](https://raw.githubusercontent.com/ReniUmiKhasanah/readme.md_perpustakaan/refs/heads/main/flow.jpg)

**ERD:** [Lihat ERD](https://raw.githubusercontent.com/Annajwaannuryaqni/readme-perpustakaandigitalilmu/refs/heads/main/ERD-PERPUSTAKAAN.png)


---

## 1. Halaman Publik (Landing Page)

`index.php` bisa diakses tanpa login dan menampilkan:
- Statistik perpustakaan real-time: total koleksi buku, anggota aktif, jumlah peminjaman bulan ini, tren peminjaman 6 bulan terakhir, dan kategori terpopuler (semua diambil langsung dari database).
- Katalog buku dengan pencarian dan filter genre.
- Background video profil perpustakaan.
- Info jam operasional dan tautan sosial media (Instagram, Facebook, WhatsApp) di footer.

## 2. Autentikasi

- `login.php` — satu halaman login dengan 3 tab akses: Admin, Petugas, Siswa.
- `register.php` — pendaftaran akun anggota (siswa) secara mandiri.
- `logout.php` — akhiri sesi.
- Password disimpan menggunakan `password_hash()` dan diverifikasi dengan `password_verify()`.

## 3. Modul Admin

| File | Fungsi |
|---|---|
| `dashboard-admin.php` | Ringkasan statistik perpustakaan |
| `kelola-buku-admin.php` | Tambah/ubah/hapus buku, kategori, stok, cover, sinopsis |
| `kelola-pengguna.php` | Tambah/ubah/hapus data anggota |
| `data-petugas.php` | Tambah/ubah data akun petugas |
| `laporan-transaksi.php` | Melihat laporan transaksi peminjaman |

## 4. Modul Petugas

| File | Fungsi |
|---|---|
| `dashboard-petugas.php` | Ringkasan sirkulasi (total buku, sedang dipinjam, dll) |
| `kelola-buku.php` | Kelola koleksi buku |
| `kelola-pengguna-petugas.php` | Kelola data anggota |
| `kelola-transaksi.php` | Melihat riwayat seluruh transaksi peminjaman beserta status otomatis (Dipinjam / Terlambat / Dikembalikan) |
| `pengembalian-buku.php` | Memproses pengembalian buku (update status, catat tanggal kembali, stok bertambah otomatis) |
| `denda.php` | Menghitung denda keterlambatan secara otomatis, bisa menandai lunas |
| `laporan-transaksi.php` | Laporan transaksi |
| `profil-petugas.php` | Kelola profil & ganti password sendiri |

## 5. Modul Siswa/Anggota

| File | Fungsi |
|---|---|
| `dashboard-user.php` | Katalog buku lengkap dengan rata-rata rating |
| `daftar-buku.php` | Daftar/pencarian buku |
| `proses-pinjam.php` | Ajukan peminjaman — buku langsung berstatus "dipinjam" (tanpa approval petugas), stok otomatis berkurang |
| `riwayat-pinjam.php` | Riwayat peminjaman pribadi + form beri rating & ulasan buku yang pernah dipinjam |

## 6. Denda Keterlambatan

Dihitung otomatis di `petugas/denda.php`:

```
denda = jumlah hari telat × Rp1.000
```

Petugas bisa menandai transaksi sebagai lunas (jika kolom `status_denda` tersedia di tabel `peminjaman`).

## 7. Rating & Ulasan

Siswa memberi rating 1–5 bintang beserta komentar opsional pada buku yang pernah dipinjam, disimpan ke tabel `ulasan`. Rata-rata rating tiap buku ditampilkan di katalog.

## 8. Struktur Folder

```
Candrawama_Sastra/
│
├── admin/
├── petugas/
├── siswa/
├── assets/
│   ├── images/
│   ├── js/
│   ├── video/
│   └── style.css
├── uploads/
│   ├── buku/       → cover buku
│   └── petugas/    → foto profil petugas
├── index.php
├── login.php
├── register.php
├── logout.php
└── koneksi.php
```

## 9. Teknologi

| Teknologi | Penggunaan |
|---|---|
| PHP Native | Backend & logika aplikasi |
| MySQL | Database |
| MySQLi (procedural + prepared statement) | Koneksi & query |
| Bootstrap 5 | UI halaman login/register |
| Tailwind CSS | UI dashboard admin/petugas |
| JavaScript | Interaksi tampilan (highlight menu, filter katalog) |
| PHP Session | Autentikasi berbasis role |
| `password_hash()` / `password_verify()` | Hashing & verifikasi password |

## 10. Tabel Database

Tabel yang dipakai (`koneksi.php` → database `if0_42792703_candrawama_sastra`):

- `admin`
- `petugas`
- `anggota`
- `buku`
- `kategori` (relasi ke `buku` lewat `id_kategori`)
- `peminjaman` (relasi ke `anggota` dan `buku`)
- `ulasan` (relasi ke `buku` dan `anggota`)

## 11. Instalasi Lokal

1. Salin folder project ke `htdocs` (XAMPP) atau `www` (Laragon).
2. Jalankan Apache & MySQL.
3. Buat database baru di phpMyAdmin, lalu buat/import tabel: `admin`, `petugas`, `anggota`, `buku`, `kategori`, `peminjaman`, `ulasan`.
4. Ubah kredensial koneksi di `koneksi.php`:
   ```php
   $host     = "localhost";
   $username = "root";
   $password = "";
   $database = "nama_database_kamu";
   ```
5. Akses lewat browser: `http://localhost/Candrawama_Sastra/`

> ⚠️ **Catatan keamanan:** `koneksi.php` saat ini masih berisi kredensial database hosting (InfinityFree) dalam bentuk plain text. Jangan upload file ini apa adanya ke repository publik — ganti dulu dengan kredensial lokal/placeholder, dan sebaiknya ganti password database aslinya kalau sudah pernah ter-commit.

## 12. Kontributor

*reni *
