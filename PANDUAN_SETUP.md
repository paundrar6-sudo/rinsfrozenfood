# Panduan Setup — Rins Frozen Food (Web Utama + Admin Panel)

Ada 3 file yang saling terhubung:

| File | Fungsi |
|---|---|
| `rins-frozen-food.html` | Website utama untuk customer (lihat menu, pesan via WhatsApp) |
| `admin.html` | Panel internal untuk kelola stok & produk |
| `setup_database.sql` | Script sekali-jalan untuk membuat database di Supabase |

Keduanya (web utama & admin) terhubung ke **database Supabase yang sama**, jadi begitu stok diubah di admin, web utama otomatis ikut update (real-time, tanpa refresh).

---

## Langkah 1 — Buat Akun & Project Supabase

1. Buka **https://supabase.com** → Sign up (gratis, bisa pakai akun Google)
2. Klik **New Project**
3. Isi nama project (misal `rins-frozen-food`), buat password database (simpan baik-baik), pilih region terdekat (Singapore biasanya paling cepat untuk Indonesia)
4. Tunggu 1–2 menit sampai project selesai dibuat

## Langkah 2 — Jalankan Setup Database

1. Di dashboard Supabase, buka menu **SQL Editor** (ikon di sidebar kiri)
2. Klik **New query**
3. Buka file `setup_database.sql`, salin **seluruh isinya**, tempel ke SQL Editor
4. Klik **Run** (atau Ctrl+Enter)
5. Tunggu sampai selesai — akan muncul tabel `products` dengan 800 baris produk template (nama "Produk Baru 001" dst, harga default Rp 15.000, stok 50)

## Langkah 3 — Ambil URL & Anon Key

1. Di dashboard Supabase, buka **Project Settings** (ikon gear) → **API**
2. Salin dua nilai ini:
   - **Project URL** (bentuknya seperti `https://xxxxxxxxxxxx.supabase.co`)
   - **anon public** key (kode panjang dimulai dengan `eyJ...`)

## Langkah 4 — Pasang ke Kedua File

Buka `rins-frozen-food.html` dengan text editor (Notepad, VS Code, dll), cari baris ini (sekitar pertengahan kode, di bagian `<script>`):

```js
const SUPABASE_URL = 'https://idznfxlkxbjixjhxgktc.supabase.co';
const SUPABASE_KEY = 'sb_secret_nLXVQbBihXaSNhHrzLPbDg_S4ZAlbWD';
```

Ganti dengan nilai yang Anda salin di Langkah 3. Contoh:

```js
const SUPABASE_URL = 'https://abcdefghijk.supabase.co';
const SUPABASE_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...';
```

**Lakukan hal yang sama di `admin.html`** — cari baris yang sama persis di bagian bawah kode, dan isi dengan **nilai yang sama** (URL & key harus identik di kedua file, karena keduanya harus terhubung ke database yang sama).

## Langkah 5 — Pasang Logo

Letakkan file `logo.png` Anda di folder yang sama dengan `rins-frozen-food.html` dan `admin.html`. Logo ini otomatis dipakai sebagai:
- Logo di navbar
- Ikon di tab browser (favicon)

## Langkah 6 — Edit 800 Produk

Ada dua cara mengisi data 800 produk template:

**Cara A — Lewat Admin Panel (lebih mudah, disarankan)**
1. Buka `admin.html` di browser
2. Cari produk lewat kolom pencarian atau filter kategori
3. Klik langsung pada nama, harga, kategori, atau stok untuk mengedit — otomatis tersimpan setelah beberapa saat

**Cara B — Lewat Supabase Table Editor (lebih cepat untuk banyak data sekaligus)**
1. Di dashboard Supabase, buka menu **Table Editor** → pilih tabel `products`
2. Edit langsung di tabel, atau gunakan fitur **Import** untuk upload data dari Excel/CSV

---

## Cara Kerja "Stok Habis"

- Saat stok produk diset ke **0** di Admin Panel, web utama otomatis menampilkan badge merah **"Stok Habis"** pada produk tersebut dan tombol "+ Tambah" berubah jadi nonaktif
- Saklar **Aktif** di admin panel berfungsi untuk menyembunyikan produk sepenuhnya dari web utama (berbeda dari stok habis — produk nonaktif tidak akan muncul sama sekali, sedangkan produk stok habis tetap muncul tapi tidak bisa dibeli)
- Saklar **Unggulan** menentukan apakah produk muncul di bagian "Menu Unggulan" di halaman beranda

## Tentang Keamanan

Setup ini menggunakan kebijakan akses terbuka (siapa saja dengan link `admin.html` bisa mengubah data) — ini cocok untuk tahap awal atau pemakaian internal toko. Jika nanti ingin admin panel dilindungi password/login, beri tahu saya dan saya bantu tambahkan Supabase Auth.

## Tentang Rencana APK

Sesuai permintaan, pembuatan APK (aplikasi Android) untuk web utama ini di-pending dulu. Saat siap dilanjutkan, opsi yang bisa dipakai nanti antara lain membungkus website ini dengan WebView (Android Studio) atau menggunakan layanan pembungkus PWA-to-APK. Beri tahu saya kapan ingin lanjut ke tahap ini.
