# Proxy Worker Cloudflare yang Ditingkatkan

Proyek ini adalah Cloudflare Worker yang berfungsi sebagai proxy serbaguna, didasarkan pada fungsionalitas dari [Nautica oleh FoolVPN-ID](https://github.com/FoolVPN-ID/Nautica). Worker ini menyediakan antarmuka pengguna yang modern dan futuristik untuk menjelajahi web melalui proxy, serta menyertakan fitur optimasi unik untuk mengurangi jumlah permintaan (request) dan mempercepat waktu muat.

## Fitur Utama

- **Antarmuka Proxy Interaktif**: Halaman arahan yang modern dan efisien memungkinkan Anda memasukkan URL situs web apa pun untuk diakses melalui proxy.
- **Proxy Multi-Protokol**: Tetap mendukung VLESS, Trojan, dan Shadowsocks untuk koneksi klien tingkat lanjut.
- **Penyematan Aset**: Mengurangi waktu muat halaman dengan menyematkan aset seperti CSS, JavaScript, dan gambar langsung ke dalam HTML.
- **Daftar Proxy yang Dapat Disesuaikan**: Ganti daftar server proxy default dengan mudah melalui variabel lingkungan.

---

## Cara Kerja Optimasi Request (Penyematan Aset)

Fitur ini, ketika diaktifkan, secara signifikan mengurangi jumlah permintaan HTTP yang harus dibuat oleh browser untuk memuat sebuah halaman web.

### 1. Perilaku Normal Browser (Tanpa Optimasi)
Biasanya, browser Anda meminta file HTML, lalu memindainya untuk menemukan dan meminta setiap aset (CSS, JS, gambar) satu per satu. Ini bisa sangat lambat.

### 2. Perilaku dengan Optimasi (Penyematan Aset Aktif)
Ketika `EMBED_ASSETS` diaktifkan, skrip Worker akan mengambil semua aset di sisi server dan menyematkannya langsung ke dalam satu file HTML. Hasilnya, browser Anda hanya perlu membuat **satu permintaan** untuk mendapatkan seluruh konten halaman, membuatnya lebih cepat dan efisien.

---

## Cara Menggunakan

1.  **Buka URL Worker Anda**
    Cukup navigasikan ke URL worker Anda (misalnya, `proxy-saya.nama-subdomain.workers.dev`).
2.  **Masukkan URL**
    Di halaman arahan "Proxy Gateway", ketik atau tempel URL situs web yang ingin Anda kunjungi.
3.  **Klik "Go"**
    Worker akan memuat versi proksi dari situs web yang Anda minta.

---

## Cara Deploy di Cloudflare

1.  **Masuk ke Akun Cloudflare Anda**
    Buka [dash.cloudflare.com](https://dash.cloudflare.com/) dan masuk.

2.  **Buat Worker Baru**
    -   Navigasikan ke **Workers & Pages** > **Create application** > **Create Worker**.
    -   Beri nama Worker Anda dan klik **Deploy**.

3.  **Salin dan Tempel Kode**
    -   Klik **Edit code**.
    -   **Hapus semua isi skrip default** yang ada di editor.
    -   Salin seluruh isi kode dari file `_worker.js` dalam repositori ini.
    -   Tempelkan kode tersebut ke dalam editor.

4.  **Konfigurasi Variabel Lingkungan (Opsional)**
    -   Di dalam editor, pergi ke tab **Settings** > **Variables**.
    -   Di bawah **Environment Variables**, tambahkan variabel berikut. **Gunakan tipe `Text` untuk semua variabel**.

| Nama Variabel  | Tipe Variabel | Deskripsi                                                                                                  | Contoh Nilai                                                                    |
| -------------- | ------------- | ---------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `EMBED_ASSETS` | `Text`        | Atur ke `true` untuk mengaktifkan fitur penyematan aset dan mengurangi jumlah permintaan.                  | `true`                                                                          |
| `PRX_BANK_URL` | `Text`        | (Opsional) URL ke file `.txt` daftar proxy kustom Anda. Jika tidak diatur, akan menggunakan daftar default. | `https://raw.githubusercontent.com/user/repo/main/myproxies.txt`                |

    -   Klik **Save** untuk setiap variabel yang Anda tambahkan.

5.  **Simpan dan Terapkan**
    -   Kembali ke editor kode dan klik **Save and deploy**.

Worker Anda sekarang aktif dan siap digunakan.
