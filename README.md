# Website Puskesmas Wedung I — Kabupaten Demak

Website profil dan pusat informasi pelayanan kesehatan Puskesmas Wedung I.

## Deployment
Situs statis diterbitkan ke GitHub Pages melalui GitHub Actions. Berkas awal website disimpan sebagai paket sumber, lalu workflow menyiapkan direktori publikasi dan menyalin file konten CMS ke lokasi yang sesuai.

## CMS
Dashboard berada di `/admin/` dan menggunakan Decap CMS dengan backend Decap Turbo + GitHub. Sebelum CMS dapat digunakan:
1. Install Decap Turbo GitHub App pada repository ini.
2. Buat Site di Decap Turbo untuk repository `iqbalyfarisy/puskesmas-wedung-1`, branch `main`.
3. Tambahkan URL admin `https://iqbalyfarisy.github.io/puskesmas-wedung-1/admin/`.
4. Salin Site ID ke `admin/config.yml`.
5. Undang petugas editor dari dashboard Decap Turbo.

## Catatan
Jangan menerbitkan data pasien, rekam medis, NIK, nomor BPJS, atau informasi kesehatan pribadi. Kontak, jam layanan, visi-misi, daftar layanan aktif, struktur organisasi, statistik, dan logo instansi induk harus divalidasi oleh Puskesmas sebelum publikasi.

Logo Puskesmas pada versi ini merupakan konsep visual yang disetujui untuk pengembangan dan masih memerlukan pengesahan instansi.
