# Panduan Deployment cPanel melalui SSH

Repository: `iqbalyfarisy/puskesmas-wedung-1`  
Branch produksi: `main`  
Metode: GitHub Actions → SCP melalui SSH → cPanel `public_html`

## 1. Siapkan file website di repository

File `puskesmas-wedung-i-site-source-v2.zip` sudah tersedia di root repository. Workflow sekarang menerima nama tersebut maupun nama alternatif `site-source-v2.zip`; tidak perlu mengunggah ulang atau mengganti nama.

Paket ZIP berisi folder `puskesmas-wedung-i-website/` dengan halaman HTML, aset, foto gedung, logo konsep, dan halaman admin.

## 2. Siapkan SSH key di cPanel

Dari cPanel:
1. Buka **Security → SSH Access → Manage SSH Keys**.
2. Generate pasangan key (Ed25519 bila didukung, RSA 4096 bila tidak).
3. Gunakan key tanpa passphrase untuk penggunaan non-interaktif GitHub Actions, atau siapkan format OpenSSH tanpa passphrase.
4. Klik **Manage → Authorize** pada public key.
5. Download/copy private key untuk disimpan sebagai GitHub Secret. Jangan kirim private key melalui chat.
6. Catat hostname server SSH, port SSH (umumnya 22, tetapi ikuti nilai dari provider), username cPanel, dan path dokumen website (umumnya `/home/USERNAME/public_html`).

## 3. Siapkan host key yang tepercaya

Dari komputer admin atau terminal tepercaya, jalankan:

```bash
ssh-keyscan -p 22 HOSTNAME_CPANEL
```

Ganti port dan hostname sesuai informasi provider. Verifikasi fingerprint host key dengan provider hosting sebelum dipakai, lalu simpan seluruh baris hasilnya sebagai secret `CPANEL_KNOWN_HOSTS`. Jangan menonaktifkan pemeriksaan host key.

## 4. Tambahkan GitHub Actions Secrets

Pada repository GitHub, buka **Settings → Secrets and variables → Actions → New repository secret**, lalu buat:

| Secret | Isi |
|---|---|
| `CPANEL_HOST` | Hostname SSH server, bukan URL panel |
| `CPANEL_PORT` | Port SSH dari provider, biasanya 22 |
| `CPANEL_USERNAME` | Username akun cPanel |
| `CPANEL_SSH_PRIVATE_KEY` | Isi private key SSH yang diunduh dari cPanel |
| `CPANEL_KNOWN_HOSTS` | Baris host key hasil ssh-keyscan yang fingerprint-nya sudah diverifikasi |
| `CPANEL_TARGET_DIR` | Path absolut document root, misalnya `/home/USERNAME/public_html` |

Jangan memasukkan password cPanel. Jangan menyimpan private key di file repository.

## 5. Aktifkan deployment

1. Pastikan branch `main` berisi `puskesmas-wedung-i-site-source-v2.zip` atau `site-source-v2.zip`.
2. Buat keenam secrets di atas. Bila belum lengkap, workflow akan melewati langkah deploy tanpa gagal dan dapat dijalankan ulang setelah semuanya tersedia.
3. Buka tab **Actions** dan jalankan workflow **Deploy website to cPanel via SSH** dengan **Run workflow**, atau lakukan perubahan kecil pada file yang menjadi trigger.
4. Periksa log workflow. Jika sukses, file website disalin ke document root cPanel.
5. Buka domain website dan periksa halaman utama, navigasi, aset gambar, /admin/, serta halaman 404.

## 6. Cara kerja pembaruan konten

Decap CMS menyimpan perubahan konten dan media ke repository GitHub. Perubahan pada `data/content.json`, `admin/config.yml`, dan `assets/uploads/` di branch `main` memicu workflow yang menyalin ulang situs terbaru ke cPanel.

## 7. Jika deployment gagal

- **Permission denied (publickey):** pastikan public key sudah di-authorize pada cPanel, private key secret benar, dan username cPanel tepat.
- **Host key verification failed:** perbarui `CPANEL_KNOWN_HOSTS` setelah verifikasi fingerprint dengan provider.
- **No such file or directory:** pastikan `CPANEL_TARGET_DIR` adalah path absolut document root yang benar.
- **Connection timed out/refused:** pastikan host dan port SSH benar dan akses SSH dari IP GitHub Actions tidak diblokir firewall provider.
- **CMS tidak bisa login:** site ID belum diganti atau URL /admin/ belum didaftarkan pada Decap Turbo.
- **Pembaruan CMS tidak tampil:** pastikan perubahan terjadi di branch `main` dan workflow selesai sukses.

## Catatan keamanan

GitHub-hosted runners memakai alamat IP yang dapat berubah. Jika provider membatasi SSH berdasarkan allowlist IP, tanyakan dukungan deploy key / IP ranges kepada provider. Alternatifnya gunakan self-hosted runner pada server yang diizinkan provider. Jangan membuka akses SSH lebih luas dari yang diperlukan.

Website ini statis. Tidak ada database atau backend pasien yang diperlukan untuk halaman profil dan konten publik.
