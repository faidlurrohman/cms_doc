# Cron Job

Cron Job berjalan berdasarkan waktu interval yang di tentukan di pengaturan crontab `Linux`, bisa disesuaikan atau diatur sesuai kebutuhan. Untuk melihat comfigurasi Cron Job yang berjalan pada `CMS Server` bisa menggunakan beberapa command bash dibawah ini :

- Untuk melihat Cron Job

```bash
crontab -l
```

- Untuk mengedit Cron Job

```bash
crontab -e
```

Untuk melihat Cron Job File yang berjalan pada `CMS Server` bisa menggunakan FTP/SFTP/SSH dsb. Jika sudah berhasil terhubung ke VPS Server, bisa dilanjutkan dengan command bash dibawah ini :

```bash
cd /var/www/cms.scnport.com/public_html/cron
```

Ada beberapa Cron Job File yang berjalan pada `CMS Server` untuk kebutuhan integrasi data dengan `WSBP Server` dan `Bank BNI`, antara lain :

- `respond_hold.php`

  mengelola file balikan dari `Bank BNI` yang berisi data hasil proses `Hold` Agent / PBM.

- `respond_update_hold.php`

  mengelola file balikan dari `Bank BNI` yang berisi data hasil proses `Update Hold` Agent / PBM.

- `respond_payment.php`

  mengelola file balikan dari `Bank BNI` yang berisi data hasil proses `Payment` Agent / PBM.

- `bpbatam_hold.php`

  mengelola data `Hold` Agent / PBM yang ada di `CMS` kemudian dikirim ke `WSBP Server`.

- `bpbatam_payment.php`

  mengelola data `Payment` Agent / PBM yang ada di `CMS` kemudian dikirim ke `WSBP Server`.

- `update_kursBI.php`

  mengelola data balikan terbaru dari `KURS BI`.
