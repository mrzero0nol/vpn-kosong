# Proxy Worker Cloudflare yang Ditingkatkan

Proyek ini adalah Cloudflare Worker yang berfungsi sebagai proxy serbaguna, didasarkan pada fungsionalitas dari [Nautica oleh FoolVPN-ID](https://github.com/FoolVPN-ID/Nautica). Worker ini tidak hanya menyediakan fungsionalitas proxy untuk protokol seperti VLESS, Trojan, dan Shadowsocks, tetapi juga menyertakan fitur optimasi unik untuk mengurangi jumlah permintaan (request) saat menjelajahi web.

## Fitur Utama

- **Proxy Multi-Protokol**: Mendukung VLESS, Trojan, dan Shadowsocks, memungkinkan koneksi yang fleksibel.
- **Reverse Proxy**: Dapat berfungsi sebagai *reverse proxy* untuk situs web apa pun.
- **Penyematan Aset (Fitur Baru)**: Secara opsional dapat menyematkan aset seperti gambar, CSS, dan JavaScript langsung ke dalam dokumen HTML.
- **Daftar Proxy yang Dapat Disesuaikan**: Daftar proxy yang digunakan oleh worker dapat dengan mudah diubah melalui variabel lingkungan.

---

## Cara Kerja Optimasi Request (Penyematan Aset)

Fitur ini, ketika diaktifkan, secara signifikan mengurangi jumlah permintaan HTTP yang harus dibuat oleh browser untuk memuat sebuah halaman web.

### 1. Perilaku Normal Browser (Tanpa Optimasi)
Biasanya, saat Anda mengunjungi sebuah situs, browser Anda melakukan banyak permintaan terpisah:
1.  **Permintaan Pertama:** Meminta file HTML utama.
2.  Setelah menerima HTML, browser memindai isinya.
3.  **Permintaan Berikutnya:** Browser menemukan tautan ke file lain dan meminta masing-masing file tersebut satu per satu (misalnya, file CSS untuk gaya, file JavaScript untuk fungsionalitas, dan setiap gambar).

Proses ini dapat menghasilkan puluhan atau bahkan ratusan permintaan hanya untuk satu halaman, yang dapat memperlambat waktu muat, terutama pada koneksi internet yang lambat.

### 2. Perilaku dengan Optimasi (Penyematan Aset Aktif)
Ketika Anda mengaktifkan `EMBED_ASSETS`, skrip Cloudflare Worker mengubah proses ini:
1.  **Satu Permintaan:** Browser Anda tetap membuat satu permintaan awal untuk file HTML.
2.  **Pemrosesan di Server:** Sebelum mengirimkan HTML ke browser Anda, Worker akan:
    -   Mencari semua tautan ke aset eksternal (CSS, JS, gambar).
    -   Mengambil konten dari setiap aset tersebut di sisi server.
    -   **Menyematkan (embed)** konten tersebut langsung ke dalam file HTML. Tautan CSS menjadi kode CSS di dalam tag `<style>`, tautan JS menjadi kode di dalam tag `<script>`, dan gambar diubah menjadi data base64.
3.  **Satu Respons:** Worker mengirimkan satu file HTML tunggal yang sudah berisi semua yang dibutuhkan ke browser Anda.

Dengan cara ini, browser hanya perlu menangani **satu permintaan** untuk mendapatkan seluruh konten halaman, yang membuat proses pemuatan menjadi jauh lebih cepat dan efisien.

---

## Cara Deploy di Cloudflare

1.  **Masuk ke Akun Cloudflare Anda**
    Buka [dash.cloudflare.com](https://dash.cloudflare.com/) dan masuk.

2.  **Buat Worker Baru**
    -   Di menu sebelah kiri, navigasikan ke **Workers & Pages**.
    -   Klik **Create application**, lalu pilih **Create Worker**.
    -   Beri nama untuk Worker Anda (misalnya, `proxy-saya`) dan klik **Deploy**.

3.  **Salin dan Tempel Kode**
    -   Setelah Worker dibuat, klik **Edit code**.
    -   **Hapus semua isi skrip default** yang ada di editor.
    -   Salin seluruh isi kode dari file `_worker.js` dalam repositori ini.
    -   Tempelkan kode tersebut ke dalam editor.

4.  **Konfigurasi Variabel Lingkungan (Opsional)**
    -   Di dalam editor, pergi ke tab **Settings** > **Variables**.
    -   Di bawah **Environment Variables**, tambahkan variabel berikut sesuai kebutuhan Anda. **Untuk semua variabel ini, gunakan tipe `Text`**.

| Nama Variabel          | Tipe Variabel | Deskripsi                                                                                                  | Contoh Nilai                                                                                      |
| ---------------------- | ------------- | ---------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| `EMBED_ASSETS`         | `Text`        | Atur ke `true` untuk mengaktifkan fitur penyematan aset dan mengurangi jumlah permintaan.                  | `true`                                                                                            |
| `PRX_BANK_URL`         | `Text`        | (Opsional) URL ke file `.txt` daftar proxy kustom Anda. Jika tidak diatur, akan menggunakan daftar default. | `https://raw.githubusercontent.com/user/repo/main/myproxies.txt`                                  |
| `REVERSE_PROXY_TARGET` | `Text`        | (Opsional) Domain target default untuk fungsionalitas *reverse proxy*.                                      | `example.com`                                                                                     |

    -   Klik **Save** untuk setiap variabel yang Anda tambahkan.

5.  **Simpan dan Terapkan**
    -   Kembali ke editor kode dengan mengklik tab **Code**.
    -   Klik tombol **Save and deploy** di pojok kanan atas.

Worker Anda sekarang aktif dan siap digunakan di URL yang disediakan.
