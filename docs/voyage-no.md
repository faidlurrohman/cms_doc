# Voyage No

Saat `Admin` pertama kali registrasi terkadang harus menambahkan `Voyage No` jika diperlukan untuk kebutuhan print report.

Biasanya `Admin` terlewat atau miss untuk memasukkan `Voyage No` pada saat registrasi, jadi `IT` harus menambahkan datanya secara manual menggunakan SQL Query. Untuk step-step menambahkan datanya secara manual ada dibawah ini:

## Step 1

Minta kepada admin `No. PUK` dan `Voyage No` yang akan ditambahkan, jika sudah mendapatkan `No. PUK` dan `Voyage No` maka bisa dilanjutkan untuk merubah format SQL Query dibawah, rubah data `#NO_PUK#` dengan `No. PUK` yang sudah didapatkan dari admin.

```SQL
SELECT * FROM berthing
WHERE puk = '#NO_PUK'
ORDER BY id DESC
```

## Step 2

Setelah Execute SQL Query [Step 1](voyage-no.md#step-1), cari data yang akan ditambahkan. Jika sudah, salin field `id` kemudian ubah format SQL Query dibawah, rubah data `#ID#` dengan `id` yang sudah disalin sebelumnya dan rubah data `#VOYAGE_NO#` dengan data `Voyage No` yang sudah didapat dari `admin`.

```SQL
INSERT INTO berthing_voyage (berthing_id, voyane_no)
VALUES(#ID#,'#VOYAGE_NO#')

```

## Step 3

Selesai.