# Data Payment ke WSBP Server

Sering lakukan cek data `Payment` Agent / PBM, pastikan data `Payment` sudah berhasil dikirim oleh `CMS` sekaligus berhasil diproses oleh `WSBP Server`, karena jika tidak melakukan pengecekan akan mengakibatkan missing data atau selisih laporan keuangan antara `SCN` dan `BP Batam`, lebih fatalnya bisa menyebabkan pemblokiran `Rencana Penambatan Kapal dan Rencana Operasi (RPKRO)` atau `Rencana Kegiatan Bongkar Muat (RKBM)` terhadap pihak Agent / PBM terkait oleh `BP Batam` karena dianggap pihutang.

Beberapa kasus yang menyebabkan data `Payment` tidak berhasil dikirim `CMS` atau diproses `WSBP Server`, antara lain :

- Data `Payment` sudah dikirim `CMS` tetapi tidak berhasil diproses oleh `WSBP Server`.
- Jaringan Koneksi antara `CMS` dengan `WSBP Server`, pada kasus ini bisa dilakukan pengiriman data `Payment` secara [Manual](data-payment-wsbp.md#pengiriman-data-manual).

## Tidak Berhasil Diproses

Ketika ada data `Payment` yang tidak berhasil diproses oleh `WSBP Server`, segera hubungi `IT BP` untuk melakukan cek data dan tunggu hasil feedback-nya.

Jika feedback dari `IT BP` menyatakan adanya data dari Agent / PBM yang masih belum lengkap pada `Inaportnet`, segera hubungi `Team Port` untuk follow up langsung ke Agent / PBM terkait untuk melengkapi data pada `Inaportnet` supaya bisa berhasil diproses data `Payment`-nya oleh `WSBP Server`.

## Pengiriman Data (Otomatis)

Perlu diketahui bahwa Pengiriman Data `Payment` ke `WSBP Server` pada `CMS` adalah menggunakan metode [Cron Job](cronjob.md), dengan job waktu interval /10 menit.

## Pengiriman Data (Manual)
