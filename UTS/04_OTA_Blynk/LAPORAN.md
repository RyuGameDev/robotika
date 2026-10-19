# Modul 04 - Pembaruan Firmware iMCLab melalui Blynk.Air

Identitas: [identitas mahasiswa](../IDENTITAS.md). Tanggal eksperimen: **belum diisi**.

## Ringkasan riset

Modul ini menyiapkan pembaruan firmware iMCLab melalui jaringan menggunakan Blynk.Edgent dan Blynk.Air. Firmware pertama dipasang melalui USB, kemudian versi berikutnya dikirim melalui layanan OTA. Eksperimen berfokus pada identitas versi, proses pembaruan, dan keadaan motor sebelum serta sesudah reboot. Materi lokal belum memuat program OTA, sehingga bagian ini dikembangkan dari contoh resmi Edgent v1.3.2. Hasil pengiriman OTA belum tersedia dan tidak dinyatakan berhasil sebelum ada bukti dari perangkat.

## Tujuan dan pertanyaan penelitian

Tujuannya adalah memasang firmware baru tanpa upload USB kedua, mempertahankan fungsi dashboard, dan memastikan keluaran motor tidak aktif selama pembaruan. Pertanyaan penelitian: apakah perangkat benar-benar menjalankan versi baru setelah menerima shipping OTA, serta apakah ON/OFF kembali berfungsi setelah reconnect?

Hipotesisnya, perangkat dengan koneksi tersedia, binary sesuai board, dan ruang OTA cukup dapat menjalankan versi baru setelah reboot. Pengujian versi pada perangkat diperlukan untuk membedakan keberhasilan kompilasi, pengunggahan binary ke cloud, dan keberhasilan instalasi firmware.

## Dasar teori dan batas eksperimen

OTA adalah pembaruan program melalui jaringan. Mengubah slider V2 hanya mengubah nilai kendali saat runtime; tindakan itu bukan OTA. Dalam [API Blynk](https://docs.blynk.io/en/blynk-library-firmware-api/ota-updates), permintaan pembaruan diterima pada pin internal OTA. Edgent menyediakan komponen penerimaan dan pemasangan firmware serta pengaturan koneksi awal.

Paket menggunakan [contoh resmi Edgent v1.3.2](https://github.com/Blynk-Technologies/blynk-library/tree/v1.3.2/examples/Blynk.Edgent/Edgent_ESP32), dengan header lengkap dan lisensi MIT. Perubahan lokal menambahkan penghentian motor ketika permintaan OTA diterima dan ketika status Edgent meninggalkan mode operasi. Motor juga mulai dari OFF setelah reboot. Detail adaptasi dicatat di [catatan program](program/imclab_ota/README.md).

![Diagram tahapan OTA Blynk](gambar/arsitektur.svg)

**Gambar 1.** Rancangan urutan pembaruan. Upload USB dipakai untuk firmware awal. Keberhasilan OTA harus ditunjukkan dengan versi baru pada perangkat dan dashboard, bukan hanya berkas binary yang berhasil dibuat. Diagram bukan log pembaruan nyata.

## Metode eksperimen

Gunakan kit dan jaringan Modul 03. Tambahkan dukungan akun Blynk.Air, partisi yang mendukung OTA, serta cukup ruang untuk image aplikasi baru. Identitas template diisi melalui `template_config.h`, sedangkan Wi-Fi dan token device dikelola Edgent lewat provisioning. Tidak menggunakan `secrets.h` milik Modul 03.

1. Pasang [imclab_ota.ino](program/imclab_ota/imclab_ota.ino) versi 1.0.0 melalui USB, lakukan provisioning, lalu rekam nilai V4 dan status online.
2. Periksa dashboard dan motor; kembalikan motor ke OFF sebelum proses update.
3. Ubah versi firmware menjadi 1.0.1 dan ekspor binary aplikasi dengan board serta partisi yang sama. Jangan upload versi ini lewat USB.
4. Buat shipping Blynk.Air untuk device percobaan tertentu. Catat nama binary, ukuran, target, dan waktu mulai.
5. Tunggu proses update, reboot, dan reconnect. Catat waktu selesai serta versi V4/Serial.
6. Jalankan uji ON/OFF dan monitoring setelah pembaruan, lalu simpan bukti.

Langkah konfigurasi rinci ada di [panduan praktik](../PANDUAN_PRAKTIK.md). Prosedur mengikuti [dokumentasi Blynk.Air](https://docs.blynk.io/en/blynk.edgent/updating-devices-firmwares-ota). Jangan menargetkan seluruh perangkat jika hanya satu kit yang diuji.

## Analisis pemrograman dan kriteria keberhasilan

`BLYNK_FIRMWARE_VERSION` memberikan penanda versi yang berbeda antara build pertama dan kedua. V4 mengirim versi ke dashboard. Ini memudahkan pembuktian bahwa perangkat menjalankan build baru, tetapi tangkapan layar perlu diambil setelah reconnect agar tidak menampilkan nilai tersimpan yang lama.

Fungsi `stopForOta()` mengunci motor sejak permintaan update diterima. Hook perubahan state Edgent menghentikan motor sebelum proses konfigurasi atau penyambungan yang dapat memakan waktu. Jika OTA gagal dalam sesi tersebut, motor tetap terkunci; periksa status, lalu lakukan pemulihan/reboot terkontrol. Sketch tetap menyediakan handler OTA pada versi berikutnya agar jalur update tidak hilang.

Keberhasilan eksperimen mensyaratkan: firmware awal terbukti 1.0.0; pembaruan kedua dikirim lewat Blynk.Air; perangkat kembali online dengan 1.0.1; motor OFF setelah reboot; kendali dashboard kembali bekerja. Sukses kompilasi saja belum memenuhi kriteria tersebut.

Ukuran binary dibandingkan dengan kapasitas slot OTA, bukan seluruh flash. Koneksi terputus, binary keliru, dan keterbatasan akun dapat menjadi penyebab kegagalan yang harus dicatat. Paket memakai mekanisme unduh dari contoh upstream; validasi produksi seperti kebijakan sertifikat dan firmware bertanda tangan berada di luar eksperimen ini. Jangan menyatakan telah menguji ketahanan terhadap listrik padam bila percobaan itu tidak dilakukan.

## Hasil pengamatan

**Belum ada pengiriman OTA ke perangkat.** Isi [lembar pengamatan](data/pengamatan.md).

| Parameter | Hasil aktual |
| --- | --- |
| Board / ukuran flash / partisi | |
| Versi awal yang terbaca | |
| Versi binary baru | |
| Nama dan ukuran binary aplikasi | |
| Device target shipping | |
| Waktu mulai / selesai (zona waktu) | |
| Status shipping | |
| Versi setelah reboot | |
| Motor OFF setelah reboot | |
| Hasil uji dashboard setelah update | |

Tambahkan screenshot sebelum, proses shipping, dan sesudah ke [folder bukti](bukti/README.md). Jika akses Blynk.Air tidak tersedia, tulis kendalanya dan tandai eksperimen belum terlaksana.

## Pembahasan dan simpulan sementara

Secara rancangan, penanda versi dan penguncian motor menyediakan jalur pembuktian pembaruan serta perilaku keluaran yang jelas. Keberhasilan OTA, durasi, dan fungsi setelah reboot belum dapat disimpulkan tanpa eksperimen. Setelah diuji, simpulan harus mengaitkan status shipping dengan versi aktual perangkat serta hasil uji kendali, dan menjelaskan setiap kegagalan yang terjadi.

## Rujukan

Dokumentasi Blynk.Air, API OTA, contoh Edgent v1.3.2, dan [REFERENSI.md](../REFERENSI.md). Pemetaan motor diadaptasi dari `imclab_Blynk_kelas.ino` materi dosen.
