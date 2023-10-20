# Log Email

Lakukan cek `Log Email` pada aplikasi CMS, beberapa data yang bisa dicek dan diperhatikan seperti :

- `SMPT Email`.
- `Request dan Response WSBP (Uper Hold / Update Hold / Payment)`.
- `Request dan Response BNI WEBDAV (Uper Hold / Update Hold / Payment)`.
- `DLL`.

Untuk melihat `Log Email` bisa menggunakan FTP/SFTP/SSH dsb. Jika sudah berhasil terhubung ke VPS Server, bisa dilanjutkan dengan perintah dibawah ini :

```bash
cd /var/mail
```

```bash
cat /var/mail/root
```

```bash
cat /var/mail/root.0
```

```bash
cat /var/mail/root.1
```
