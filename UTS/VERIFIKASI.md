# Verifikasi perangkat lunak dan kit

Tanggal pemeriksaan: **18 Oktober 2026**. Pemeriksaan ini dilakukan tanpa upload ke kit dan tanpa menjalankan motor.

## Kompilasi Arduino

Lingkungan: Windows, Arduino CLI 1.3.1, ESP32 core 2.0.17, FQBN `esp32:esp32:esp32`, Blynk 1.3.2. Sketch disalin ke direktori build sementara; `secrets.h` dan `template_config.h` hanya berisi placeholder dari berkas contoh. Tidak menggunakan token/password materi dosen.

| Sketch | Hasil | Ukuran program / batas slot | RAM global |
| --- | --- | --- | --- |
| `imclab_basic` | Lulus kompilasi | 275.885 / 1.310.720 byte | 22.084 byte |
| `imclab_blynk` | Lulus kompilasi | 889.349 / 1.310.720 byte | 47.456 byte |
| `imclab_ota` | Lulus kompilasi | 985.025 / 1.310.720 byte | 51.020 byte |

Ukuran dapat berubah mengikuti compiler, konfigurasi, dan perubahan kode. Hasil ini tidak membuktikan kecocokan revisi kit, koneksi Blynk, gerak motor, pembacaan RPM, atau keberhasilan OTA. Core ESP32 3.x belum dikompilasi dalam pemeriksaan ini.

## Python

Lima tes otomatis lulus dengan Python 3.10:

1. Batas PWM dan penolakan perintah tidak valid/multi-baris.
2. Penolakan pemilihan port otomatis yang ambigu.
3. Newline dan penerimaan ACK setelah baris telemetri.
4. Error perangkat atau timeout tidak dianggap sukses.
5. Parsing data mempertahankan durasi sampel aktual.

Jalankan ulang dari root:

```powershell
python -m unittest discover -s UTS/tests -v
```

Pengujian memakai objek serial tiruan, bukan sambungan fisik. Angka yang muncul dari tes adalah fixture software dan tidak termasuk data eksperimen.

## Dokumen dan gambar

- Empat laporan riset memiliki tujuan, dasar teori, metode, gambar, analisis, tabel hasil kosong, dan simpulan sementara.
- Lima diagram SVG telah dirender dan diperiksa secara visual.
- Seluruh tautan lokal Markdown diperiksa dan tidak ada target yang hilang.
- Salinan `ImcCore.h` pada ketiga sketch identik saat pemeriksaan.
- Bagian hasil eksperimen masih jelas ditandai belum diuji/dicatat.

## Pengujian lanjutan yang diperlukan

Isi bukti pada [checklist](CHECKLIST.md): verifikasi pin kit, upload USB, respons motor, kalibrasi PPR, komunikasi serial asli, koneksi Blynk, dan pengiriman OTA dari versi 1.0.0 ke 1.0.1. Identitas serta simpulan empiris baru dilengkapi dari hasil tersebut.

## Pembaruan Modul 01 - 18-19 Oktober 2026

Catatan di atas merupakan pemeriksaan awal sebelum pengujian fisik. Setelah itu, driver USB CP210x terpasang, COM5 terdeteksi, dan firmware basic diunggah dengan hash flash terverifikasi.

- Tersimpan 14 log asli dan 135 sampel DATA, termasuk hasil gagal start dan penghentian dini.
- Uji PWM 200 sempat gagal dan sempat berhasil tanpa bantuan. Tidak disimpulkan ambang start yang pasti.
- Rangkaian saat berjalan menyelesaikan PWM 200 dan 160 selama sekitar 15 detik. Fase 120 dihentikan saat pulsa hilang; fase 80 dilewati.
- Sepuluh pemeriksaan parser pada kit lulus dengan keluaran motor tetap OFF.
- Analisis mengecualikan ACK/STATUS dan memeriksa konsistensi RPM terhadap pulsa, DT_MS, serta asumsi PPR=2. Sensor belum dikalibrasi.
- Sketch aktif untuk screenshot diverifikasi melalui Show Sketch Folder. Screenshot akhir memperlihatkan imclab_basic.ino, ESP32 Dev Module, COM5, 115200 baud, dan PWM=0.
- PDF Modul 01 hasil pembaruan terdiri dari 6 halaman dan seluruh halaman diperiksa melalui render PNG; label grafik yang semula berdekatan sudah diperbaiki. Salinan di folder modul dan output/pdf identik.
- Seluruh tautan lokal pada 10 dokumen terkait valid. CSV memuat tepat 135 DATA, dan ringkasan laju pulsa sesuai hasil analisis. Foto identifikasi memiliki hash yang sama dengan lampiran asli pengguna.

Detail pengamatan, batas kesimpulan, dan sumber data ada di [laporan Modul 01](01_Basic_iMCLab/LAPORAN.md). Pada akhir sesi Modul 01, pengujian Modul 02-04 belum dilakukan. Pembaruan Modul 02 tercantum di bawah.

## Pembaruan Modul 02 - 19 Oktober 2026

- Python 3.12.14 dan pySerial 3.5 dijalankan melalui `.venv` proyek. Tidak ada perubahan firmware ESP32.
- Sembilan tes software lulus, termasuk tambahan balasan tidak sesuai, kehilangan DATA/pulsa, capture OFF, serta penolakan durasi non-finite. Tes tiruan bukan data kit.
- Serial Monitor Arduino ditutup. Python mendeteksi COM5 CP210x dan menyelesaikan empat sesi: OFF, pilot PWM 200, sweep 200/160/120, serta uji input saat OFF.
- Empat CSV asli memuat 67 DATA. Seluruh kolom pengukuran cocok dengan raw dan RPM sesuai rumus PPR=2; kalibrasi belum dilakukan.
- Operator mengonfirmasi pilot berputar sendiri lalu berhenti dan tahap akhir sweep tetap berputar pelan lalu berhenti tanpa bantuan.
- Sweep memiliki 45 DATA aktif, masing-masing 15 pada PWM 200/160/120. Tidak ada ERROR/cutoff. Sampel setelah OFF berisi 19, 0, 0 pulsa.
- Rata-rata lima DATA akhir adalah 267,2; 133,8; dan 20,8 pulsa/detik. PWM 120 tidak memenuhi kriteria rentang relatif <=5%, sehingga tidak dilabeli stabil.
- Pemilihan port otomatis dan input R/PWM/+/- berhasil. Input p256 serta ON ditolak Python sebelum TX. Seluruh uji input dilakukan saat motor OFF.
- PDF Modul 02 diperbarui menjadi delapan halaman dengan screenshot OFF, screenshot ON, serta foto susunan kit. Seluruh halaman telah dirender ke PNG dan diperiksa; grafik, tabel, gambar, serta keterangan tidak terpotong atau bertumpang tindih. Salinan di folder modul dan output/pdf identik.
- Dua screenshot dan satu foto asli dari operator disalin tanpa pengubahan. Ketiga hash SHA256 cocok dengan lampiran sumber; asal-usulnya dicatat di bukti/README.md.
- Pemeriksaan tambahan OFF 11:28:27 WIB sesuai screenshot: seluruh DATA melaporkan PWM=0 dan RPM=0. Percobaan 11:33:48 WIB gagal membuka COM5; hanya EVENT ERROR, tanpa TX/RX, sehingga tidak dihitung sebagai uji motor.
- Pengulangan 11:35:27 WIB sesuai screenshot ON: 11 DATA, lima aktif pada PWM 200 dengan pulsa 69, 203, 250, 258, 268 per 1000 ms. Jarak TX ON-OFF 5,198435 detik; DATA akhir PWM=0 dan RPM=0, kode keluar program 0. Sesi tambahan tetap dipisahkan dari 67 DATA untuk analisis utama.
- Tautan lokal pada tujuh dokumen terkait diperiksa; tidak ada target yang hilang. Identitas tetap kosong dan kalibrasi PPR tetap dicatat sebagai keterbatasan.

Hasil dan keterbatasan tercantum pada [laporan Modul 02](02_Python_iMCLab/LAPORAN.md). Modul 03-04 belum diuji pada kit.
