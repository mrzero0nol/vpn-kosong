# Proxy Worker Cloudflare yang Ditingkatkan

Proyek ini adalah Cloudflare Worker yang berfungsi sebagai proxy serbaguna, didasarkan pada fungsionalitas dari [Nautica oleh FoolVPN-ID](https://github.com/FoolVPN-ID/Nautica). Worker ini tidak hanya menyediakan fungsionalitas proxy untuk protokol seperti VLESS, Trojan, dan Shadowsocks, tetapi juga menyertakan fitur optimasi unik untuk mengurangi jumlah permintaan (request) saat menjelajahi web.

## Fitur Utama

- **Proxy Multi-Protokol**: Mendukung VLESS, Trojan, dan Shadowsocks, memungkinkan koneksi yang fleksibel.
- **Reverse Proxy**: Dapat berfungsi sebagai *reverse proxy* untuk situs web apa pun.
- **Penyematan Aset (Fitur Baru)**: Secara opsional dapat menyematkan aset seperti gambar, CSS, dan JavaScript langsung ke dalam dokumen HTML. Fitur ini secara drastis mengurangi jumlah permintaan HTTP yang diperlukan untuk memuat halaman, sehingga mempercepat waktu muat di sisi klien.
- **Daftar Proxy yang Dapat Disesuaikan**: Daftar proxy yang digunakan oleh worker dapat dengan mudah diubah melalui variabel lingkungan tanpa perlu mengubah kode.

---

## Cara Deploy di Cloudflare

Men-deploy worker ini sangat mudah. Cukup ikuti langkah-langkah berikut:

1.  **Masuk ke Akun Cloudflare Anda**
    Buka [dash.cloudflare.com](https://dash.cloudflare.com/) dan masuk.

2.  **Buat Worker Baru**
    -   Di menu sebelah kiri, navigasikan ke **Workers & Pages**.
    -   Klik **Create application**, lalu pilih **Create Worker**.
    -   Beri nama untuk Worker Anda (misalnya, `proxy-saya`) dan klik **Deploy**.

3.  **Salin dan Tempel Kode**
    -   Setelah Worker dibuat, klik **Edit code**.
    -   Anda akan melihat editor kode dengan beberapa skrip default. **Hapus semua isi skrip default tersebut.**
    -   Salin seluruh isi kode dari file `_worker.js` dalam repositori ini.
    -   Tempelkan kode tersebut ke dalam editor di Cloudflare.

4.  **Konfigurasi Worker (Opsional tapi Direkomendasikan)**
    -   Di dalam editor, pergi ke tab **Settings** > **Variables**.
    -   Di bawah **Environment Variables**, tambahkan variabel untuk mengonfigurasi worker sesuai kebutuhan Anda:

        -   **Mengaktifkan Penyematan Aset:**
            *   **Variable name**: `EMBED_ASSETS`
            *   **Value**: `true`

        -   **Menggunakan Daftar Proxy Kustom Anda:**
            *   **Variable name**: `PRX_BANK_URL`
            *   **Value**: `https://raw.githubusercontent.com/mrzero0nol/My-v2ray/refs/heads/main/proxyList.txt` (atau URL lain ke daftar proxy Anda).

        -   **Mengatur Target Reverse Proxy Default:**
            *   **Variable name**: `REVERSE_PROXY_TARGET`
            *   **Value**: `example.com` (ganti dengan situs target yang Anda inginkan).

    -   Klik **Save** untuk setiap variabel yang Anda tambahkan.

5.  **Simpan dan Terapkan**
    -   Kembali ke editor kode dengan mengklik tab **Code**.
    -   Klik tombol **Save and deploy** di pojok kanan atas.

Setelah beberapa detik, Worker Anda akan aktif dan siap digunakan di URL yang disediakan (misalnya, `proxy-saya.nama-subdomain.workers.dev`).
