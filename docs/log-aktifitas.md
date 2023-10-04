# Log Aktifitas

Selalu cek `Log Aktifitas` pada aplikasi CMS, beberapa data yang bisa dicek dan diperhatikan seperti :

- `Data Aktifitas Pengguna`
- `IP Login Pengguna`
- `Request dan Response Uper Hold dari Kustomer (Agent / PBM)`
- `Request dan Response Update Hold dari Kustomer (Agent / PBM)`
- `Request dan Response Payment dari Kustomer (Agent / PBM)`
- `Error Query`
- `DLL`

## Lokasi Lok Aktifitas

Untuk melihat `Log Aktifitas` bisa menggunakan FTP/SFTP/SSH dsb. Jika sudah berhasil terhubung ke VPS Server, bisa dilanjutkan dengan command bash dibawah ini :

```bash
cd /var/log/apache2
```

```bash
cd /var/log/postgresql
```

```bash
cd /var/www/cms.scnport.com/public_html/scnport/logs
```
