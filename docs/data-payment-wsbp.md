# Data Payment ke WSBP Server

Lakukan cek data `Payment` Agent / PBM, pastikan data `Payment` sudah berhasil dikirim oleh `CMS` sekaligus berhasil diproses oleh `WSBP Server`, karena jika tidak melakukan pengecekan akan mengakibatkan missing data atau selisih laporan keuangan antara `SCN` dan `BP Batam`, lebih fatalnya bisa menyebabkan pemblokiran `Rencana Penambatan Kapal dan Rencana Operasi (RPKRO)` atau `Rencana Kegiatan Bongkar Muat (RKBM)` terhadap pihak Agent / PBM terkait oleh `BP Batam` karena dianggap pihutang.

Beberapa kasus yang menyebabkan data `Payment` tidak berhasil dikirim `CMS` atau diproses `WSBP Server`, antara lain :

- Data `Payment` sudah dikirim `CMS` tetapi tidak berhasil diproses oleh `WSBP Server`.
- Jaringan Koneksi antara `CMS` dengan `WSBP Server`, pada kasus ini bisa dilakukan pengiriman data `Payment` secara [Manual](data-payment-wsbp.md#pengiriman-data-manual).

## Tidak Berhasil Diproses

Ketika ada data `Payment` yang tidak berhasil diproses oleh `WSBP Server`, segera hubungi `IT BP` untuk melakukan cek data dan tunggu hasil feedback-nya.

Jika feedback dari `IT BP` menyatakan adanya data dari Agent / PBM yang masih belum lengkap pada `Inaportnet`, segera hubungi `Team Port` untuk follow up langsung ke Agent / PBM terkait untuk melengkapi data pada `Inaportnet` supaya bisa berhasil diproses data `Payment`-nya oleh `WSBP Server`.

## Pengiriman Data (Otomatis)

Perlu diketahui bahwa Pengiriman Data `Payment` ke `WSBP Server` pada `CMS` adalah menggunakan metode [Cron Job](cronjob.md), dengan job waktu interval /10 menit.

## Pengiriman Data (Manual)

Untuk melakukan pengiriman data `Payment` (Agent / PBM) ke `WSBP Server` secara manual bisa dilakukan hanya saat ada gangguan koneksi jaringan antara `CMS` dengan `WSBP Server`, step-step untuk melakukan data `Payment` secara manual ada dibawah ini :

### Step 1

Execute SQL Query dibawah ini untuk mendapatkan list data `Payment`.

```SQL
SELECT
    transaction_uper_detail.id,
    transaction_uper_detail.wsbp_payment_status
FROM berthing
JOIN berth ON berth.id = berthing.berth_id
LEFT JOIN cargo ON berthing.id = cargo.berthing_id
JOIN uper ON berthing.id = uper.berthing_id
JOIN uper_detail ON uper.id = uper_detail.uper_id
LEFT JOIN transaction_uper_total ON transaction_uper_total.transaction_id = uper_detail.transaction_id
    AND transaction_uper_total.customers_id = uper_detail.customers_id
JOIN transaction_uper_detail ON transaction_uper_detail.transaction_id = uper_detail.transaction_id
    AND ((transaction_uper_detail.customer_type = 'agent' AND transaction_uper_total.customer_type_id = 1) OR transaction_uper_detail.pbm_id = transaction_uper_total.customers_id)
    AND transaction_uper_detail.wsbp_payment_status IN (3)
JOIN service ON service.id = transaction_uper_detail.service_id
JOIN service_cost ON service_cost.id = transaction_uper_detail.service_cost_id
JOIN service_share ON service_share.service_id = service_cost.service_id and service_share.company_id=2
WHERE uper_detail.status = 'PAID'
AND uper_detail.nominal > 0
ORDER BY transaction_uper_detail.id ASC
```

### Step 2

Cek hasil Execute SQL Query [Step 1](data-payment-wsbp.md#step-1), jika menghasilkan list data maka bisa lanjut ke [Step 3](data-payment-wsbp.md#step-3), jika tidak menghasilkan list data maka disimpulkan tidak ada data `Payment` yang perlu dikirim ke `WSBP Server` secara manual atau berhenti pada [Step 2](data-payment-wsbp.md#step-2) ini.

### Step 3

Perhatikan kolom `id`, kemudian `Salin` atau `Catat` bebarapa `id` tersebut jika lebih dari 1.

### Step 4

Ganti format SQL Query `#IDS#` dibawah ini dengan data kolom `id` yang sudah disalin sebelumnya, jika lebih dari satu maka pisahkan dengan tanda `Koma (,)` contoh `11251,11252`, setelah format sudah diubah kemudian Execute SQL Query.

```SQL
UPDATE transaction_uper_detail
    SET wsbp_payment_status = 0
WHERE id IN (#IDS#)
```

### Step 5

Setelah berhasil melakukan Execute SQL Query [Step 4](data-payment-wsbp.md#step-1), maka data yang sudah di-update sebelumnya akan diproses secara [Otomatis](data-payment-wsbp.md#pengiriman-data-otomatis) oleh `CMS`, untuk memastikan data sudah ter-update bisa dilakukan dengan Execute Query [Step 1](data-payment-wsbp.md#step-1), seharusnya hasil dari Execute SQL Query tersebut tidak ada list data.

### Step 6

Selesai.
