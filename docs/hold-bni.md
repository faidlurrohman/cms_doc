# Hold Bank BNI

Pada saat verifikator ke 2 (Port Manager / Satker) melakukan verifikasi maka Sistem `CMS` akan melakukan pengiriman Request `Hold` ke `Bank BNI`.

## Dana Tidak Mencukupi

Pada Menu `Berthing List` perhatikan dibagian kolom `Bank Status` = `Waiting` dan `Status` = `Verified`, kemudian adanya notifikasi E-mail dari `Bank BNI` statusnya `Insufficinet Fund` seperti dibawah ini :

![Insufficient Hold](_media/insufficient-hold.png ":size=600")

Dari notifikasi E-mail diatas menunjukkan bahwa proses `Hold` Agent / PBM mengalami kegagalan dikarenakan dana yang tidak mencukupi. Pada kasus ini harus dibuat file request `Hold` secara manual dan ada beberapa step-step yang harus dilakukan antara lain :

### Step 1

Hubungi `Team MTO` untuk menginformasikan ke Agent / PBM bahwa proses `Hold` dana mengalami kegagalan karena dana tidak mencukupi.

### Step 2

`Pastikan` bahwa Agent / PBM sudah melakukan Top Up dana terlebih dahulu, jika sudah dipastikan maka bisa dilanjutkan ke [Step 3](hold-bni.md#step-3).

### Step 3

Catat `Refference No` yang tertera pada E-mail notifikasi, dalam contoh kasus ini adalah `230013541111`.

Masuk ke `VPS CMS Server` dengan menggunakan FTP/SFTP/SSH dsb. Jika sudah berhasil terhubung ke `VPS CMS Server`, pergi ke direktori dengan menggunakan perintah dibawah ini :

```bash
cd /var/www/cms.scnport.com/public_html/bni/block/archive
```

### Step 4

Temukan file dengan nama yang sama dengan `Refference No` pada di direktori `archive`, dalam contoh kasus ini adalah `230013541111.txt`.

Kemudian salin file `230013541111.txt` ke direktori user dengan menggunakan perintah dibawah ini :

```bash
cp 230013541111.txt /home/[user]
```

```bash
cd /home/[user]
```

### Step 5

Setelah [Step 4](hold-bni.md#step-4) berhasil, selanjutnya adalah merubah nama file yang sebelumnya sudah tersalin.

Untuk standar format penamaan file adalah sebagai berikut :

- YYYYMMDDhhmmss_RequestHold.txt

Untuk merubah nama format file sebelumnya bisa menggunakan perintah di bawah ini :

```bash
mv 230013541111.txt YYYYMMDDhhmmss_RequestHold.txt
```

!> sesuaikan standar format penamaan dengan waktu sekarang, pada contoh kasus ini adalah `20231020144960_RequestHold.txt`

### Step 6

`Enkripsi` file yang sebelumnya sudah di ubah nama dengan menggunakan perintah dibawah ini :

```bash
gpg -o [target].txt --encrypt --recipient paruhum.aritonang@bni.co.id [result].gpg
```

!> Ubah `[target]` dan `[result]` dengan standar format sebelumnya, dalam contoh kasus ini adalah `20231020144960_RequestHold`

### Step 7

Jika proses `Enkripsi` sudah berhasil, maka dilanjutkan dengan menyalin file hasil `Enkripsi` tersebut ke direktori `outgoing` dengan menggunakan perintah dibawah ini :

```bash
cp [target].gpg /var/www/cms.scnport.com/public_html/bni/block/outgoing
```

!> Ubah `[target]` nama file hasil enkripsi sebelumnya, dalam contoh kasus ini adalah `20231020144960_RequestHold`

### Step 8

Setelah [Step 7](hold-bni.md#step-7) sudah berhasil, kurang dari 15 menit maka file request akan diambil dan dihapus oleh `Host Bank BNI`.
Jika lebih dari 15 Menit file request tersebut masih ada maka bisa dipastikan ada masalah pengambilan file request, bisa dilihat pada [File Request Masih Ada di Outgoing](hold-bni.md#file-request-masih-ada-di-outgoing)

### Step 9

Jika setelah 15 Menit dan file request yang di direktori `Outgoing` sudah tidak ada, maka berarti `Host Bank BNI` sudah mengambil dan menghapus file request tersebut. Kemudian menunggu maksimal 15 menit, `Host Bank BNI` akan mengirimkan file balikan ke direktori `incoming` atau `/var/www/cms.scnport.com/public_html/bni/block/incoming`.

### Step 10

Secara paralel sambil menunggu file balikan bisa dilakukan pengecekan apakah ada notifikasi E-mail dari `Bank BNI`.
Setelah 15 menit menunggu file balikan dari `Host Bank BNI`, ada beberapa kasus yang bisa terjadi antara lain :

- Di direktori `incoming` masih belum ada balikan yang dikirim oleh `Host Bank BNI`, selanjutnya bisa dilihat [File Balikan Tidak Ada Di Incoming](hold-bni.md#file-balikan-tidak-ada-di-incoming).

- Di direktori sudah ada file balikan akan tetapi `Sistem CMS` tidak bisa membaca file balikan tersebut dikarenakan format tidak sesuai standar, sedangkan sudah ada E-mail notifikasi dari host `BNI` yang meninformasikan bahwa proses `Hold` Agent / PBM sukses. Pada kasus ini maka harus dilakukan update secara manual, bisa dilihat [Update Status Hold (Manual)](hold-bni.md#update-status-hold-manual).

- Di direktori sudah ada file balikan oleh `Host Bank BNI` dan `Sistem CMS` bisa membaca format file balikan, jika balikan tersebut meninformasikan bahwa `Hold` dana sukses maka `Sistem CMS` akan secara otomatis mengupdate data `(tabel uper_detail dan lain-lainnya)`, kemudian pada tampilan `Berthing List` perhatikan kolom `Bank Status` akan berubah menjadi `Hold` dan kolom `Status` akan berubah menjadi `Uper Hold`.

### Step 11

Selesai.

## File Request Masih Ada Di Outgoing

File request yang ada di direktori `outgoing` masih ada setelah menunggu 15 menit, bisa disebabkan ada masalah jaringan atau kemungkinan ada penyebab lain.

Untuk pengecekkan masalah bisa dilakukan beberapa step-step dibawah ini:

### Step 1

Periksa koneksi antara host `CMS` ke host `BNI` dengan menggunakan IP `182.16.167.165`, bisa dilakukan `ping` terhadap IP tersebut dengan perintah dibawah ini:

```bash
ping 182.16.167.165
```

### Step 2

Jika gagal `ping` host `BNI`, periksa lebih lanjut masalah koneksinya.

- Pastikan host `CMS` dapat ping ke host lain di LAN /Internet.

  - Jika gagal, berarti host `CMS` bermasalah koneksi jaringannya. Segera informasikan ke Team IT lain terkait adanya koneksi jaringan pada host `CMS`.
  - Jika berhasil, berarti host `BNI` bermasalah dan segera hubungi pihak `BNI` melalui E-mail `tbs_sat@bni.co.id`. Informasikan kepada mereka bahwa host `BNI` IP `182.16.167.165` tidak dapat di-`ping` atau tidak dapat dihubungi oleh `CMS`.

### Step 3

Jika berhasil `ping` host `BNI`, berarti penyebab masalah adalah host `BNI` tidak mengambil file. Reguler otomatis `Web DAV` untuk cek ke direktori `/var/www/cms.scnport.com/public_html/bni/block/outgoing/` tidak berjalan. Normalnya, setiap 15 menit host `BNI` akan memeriksa/cek file di direktori `outgoing`.

Segera hubungi pihak `BNI` melalui E-mail `tbs_sat@bni.co.id`, dengan menyertakan bukti `Log` yang menunjukkan kapan host `BNI` terakhir mengunjungi/mengecek ke host `CMS`. Ini dapat dilihat di `Log Server` pada direktori `var/log/apache2/cms.scnport.com`, atau dengan perintah di bawah ini :

```bash
cat access.log | grep '182.16.167.165'
```

### Step 4

Setelah pihak `BNI` dihubungi, langkah selanjutnya `IT` adalah menunggu respon / reply komunikasi dari mereka.

Secara paralel, periksa direktori `/var/www/cms.scnport.com/public_html/bni/block/outgoing` dan `/var/www/cms.scnport.com/public_html/bni/block/incoming`. Jika folder outgoing telah kosong, berarti file Perintah telah dibaca oleh host `BNI` dan `CMS` tinggal menunggu file balikan yang diletakan di direktori incoming oleh host `BNI`.

### Step 5

Selesai.

## File Balikan Tidak Ada Di Incoming

Setelah host `BNI` berhasil mengambil dan menghapus file request di direktori `outgoing`, selanjutnya maksimal 15 menit host `BNI` akan mengirim file balikan di direktori `incoming`. Apabila tidak ada file balikan dari host `BNI` pada direktori `incoming` maka bisa dipastikan ada kemungkinan host `BNI` tidak mengirim balikan atau `CMS` tidak menerima file balikan.

Lakukan cek notifikasi E-mail dari host `BNI` sebagai berikut :

- Jika E-mail host `BNI` seperti gambar dibawah ini maka bisa dipastikan bahwa proses `Hold` Agent / PBM gagal karena dana tidak mencukupi, bisa dilihat [Dana Tidak Mencukupi](hold-bni.md#dana-tidak-mencukupi).

![Insufficient Hold](_media/insufficient-hold.png ":size=600")

- Jika E-mail host `BNI` seperti gambar dibawah ini maka bisa dipastikan bahwa proses `Hold` Agent / PBM berhasil, akan tetapi `Sistem CMS` tidak bisa merubah status secara otomatis dikarenakan tidak adanya file balikan dati host `BNI`, untuk itu harus dilakukan update data secara manual, bisa dilihat [Update Status Hold (Manual)](hold-bni.md#update-status-hold-manual).

![Success Hold](_media/success-hold.png ":size=600")

## Update Status Hold (Manual)

Jika secara otomatis `Sistem CMS` tidak bisa membaca file balikan yang dikirim oleh host `BNI` di direktori `incoming`, sedangkan sudah ada notifikasi E-mail yang menginformasikan bahwa proses `Hold` Agent / PBM sukses.

Maka disini harus dilakukan update data secara manual dengan step-step sebagai berikut :

### Step 1

Pastikan notifikasi E-mail yang diterima host `BNI` statusnya `Bloking Ok` seperti dibawah ini :

![Success Hold](_media/success-hold.png ":size=600")

Kemudian bisa dilanjutkan dengan menjalankan SQL Query dibawah ini :

```SQL
UPDATE uper_detail
    SET status = 'HOLD',
        ref = '#JOURNAL_NO#'
WHERE code = '#REF_NO#'
```

!> ubah format `#JOURNAL_NO#` dengan `Journal No` dan `#REF_NO#` dengan `Refference No`, perhatikan saat melakukan perubahan format harus sesuai dengan yang tertera pada notifikasi E-mail dari host `BNI`.

Hasil perubahan format SQL Query pada contoh kasus ini adalah :

```SQL
UPDATE uper_detail
    SET status = 'HOLD',
        ref = '455222'
WHERE code = '230013541111'
```

Jika SQL Query diatas sukses dijalankan maka pada tampilan `Berthing List` perhatikan kolom `Bank Status` akan berubah menjadi `Hold` dan kolom `Status` akan berubah menjadi `Uper Hold`.

### Step 2

Selesai.
