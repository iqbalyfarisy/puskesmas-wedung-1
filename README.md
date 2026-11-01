# Puskesmas Wedung I — Website

Website profil dan pusat informasi pelayanan kesehatan Puskesmas Wedung I, Kabupaten Demak.

## Struktur repository

- `site-source-v2.zip` — paket website statis. Unggah paket dari lampiran percakapan ke root repository dengan nama persis ini.
- `admin/config.yml` — konfigurasi Decap CMS (Decap Turbo + GitHub).
- `data/content.json` — data publikasi yang dikelola CMS.
- `assets/uploads/` — media yang diunggah melalui CMS.
- `.github/workflows/deploy-cpanel.yml` — deployment otomatis ke cPanel melalui SSH/SCP.

## Deployment cPanel

Setelah `site-source-v2.zip` diunggah dan GitHub Actions secrets dibuat, setiap perubahan pada paket situs, konfigurasi CMS, data konten, atau upload media di branch `main` akan memicu deployment ke direktori hosting.

Lihat [CPANEL-DEPLOYMENT.md](CPANEL-DEPLOYMENT.md) untuk langkah pengaturan SSH dan daftar secrets.

## CMS

Dashboard situs berada di `/admin/` dan memakai Decap CMS dengan backend Decap Turbo + GitHub. Untuk mengaktifkannya:
1. Buat organisasi dan hubungkan GitHub di Decap Turbo.
2. Install Decap Turbo GitHub App pada akun pemilik repository dan izinkan repository ini.
3. Buat Site untuk repo `iqbalyfarisy/puskesmas-wedung-1`, branch `main`, config path `admin/config.yml`, dan admin URL domain yang digunakan.
4. Ganti `GANTI_DENGAN_TURBO_SITE_ID` di `admin/config.yml` dengan Site ID pada tab Overview.
5. Undang petugas yang berwenang sebagai editor.

Dokumentasi resmi:
- https://decapcms.org/docs/turbo-getting-started/
- https://decapcms.org/docs/turbo-connecting-a-site/

## Catatan publikasi

- Validasi telepon, email, jam layanan, visi-misi, daftar layanan aktif, struktur organisasi, statistik, dan foto sebelum pengumuman publik.
- Logo Puskesmas yang disertakan adalah konsep visual untuk pengembangan dan masih perlu pengesahan instansi.
- Jangan mempublikasikan rekam medis, data pasien, NIK, nomor BPJS, atau informasi kesehatan pribadi.
- Jangan menyimpan password, private key, atau kredensial hosting di repository. Semua kredensial deployment harus disimpan di GitHub Actions Secrets.
