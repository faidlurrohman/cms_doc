# Log Email

Selalu cek `Log Email` pada aplikasi CMS, beberapa data yang bisa dicek dan diperhatikan seperti :

- `SMPT Email`
- `Request dan Response WSBP (Uper Hold / Update Hold / Payment)`
- `Request dan Response BNI WEBDAV (Uper Hold / Update Hold / Payment)`
- `DLL`

## Lokasi Log Email

Untuk melihat `Log Email` bisa menggunakan FTP/SFTP/SSH dsb. Jika sudah berhasil terhubung ke VPS Server, bisa dilanjutkan dengan command bash dibawah ini :

```bash
cat /var/mail/log
```
