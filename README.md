# Instruksi untuk Proxy Worker dengan Penyematan Aset

Worker ini telah dimodifikasi untuk menyertakan fitur penyematan aset, yang dapat secara signifikan mengurangi jumlah permintaan yang diperlukan untuk memuat halaman web dengan menyematkan gambar, CSS, dan JavaScript langsung ke dalam dokumen HTML.

## Cara Menggunakan Fitur Penyematan Aset

Fitur ini dikontrol oleh variabel lingkungan di pengaturan Cloudflare Worker Anda.

- **Untuk mengaktifkan penyematan aset:**
  1. Buka dasbor Cloudflare Anda dan navigasikan ke Worker Anda.
  2. Pergi ke **Settings** > **Variables**.
  3. Di bawah **Environment Variables**, tambahkan variabel baru:
     - **Variable name:** `EMBED_ASSETS`
     - **Value:** `true`
  4. Simpan dan terapkan perubahan Anda.

- **Untuk menonaktifkan penyematan aset:**
  - Hapus variabel lingkungan `EMBED_ASSETS`.
  - Atau, atur nilainya ke `false` (atau nilai apa pun selain `'true'`).

Secara default, jika variabel `EMBED_ASSETS` tidak diatur, fitur ini akan dinonaktifkan.
