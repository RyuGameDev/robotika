# Serah-terima UTS Robotika iMCLab untuk Gemini

Pembaruan: **19 Oktober 2026, WIB**. Workspace: `D:\ryudev_area\robotika`, Windows PowerShell.

## Mulai di sini

Lanjutkan **Modul 03 Blynk**, kemudian **Modul 04 OTA**. Pada pemeriksaan terakhir tidak ada port serial; pengguna mengonfirmasi kit belum disambungkan kembali dan meminta serah-terima dahulu. **Belum ada upload firmware 03 atau pengujian motor melalui Blynk.** Periksa ulang port ketika pengguna siap melanjutkan praktik.

Baca [log progres dan daftar pekerjaan](UTS/PROGRES_03_04.md), [panduan praktik](UTS/PANDUAN_PRAKTIK.md), dan [verifikasi](UTS/VERIFIKASI.md). Jangan mengulang eksperimen 01–02 atau membuat duplikat template 03 tanpa alasan. Login Blynk pada browser Codex tidak otomatis tersedia di Gemini; pengguna mungkin perlu membuka akun yang sama di browser yang dapat diakses Gemini.

## Permintaan dan preferensi pengguna

- UTS memerlukan laporan riset per modul, gambar dan analisis, pemrograman basic, Python, Blynk, serta OTA Blynk. Materi dosen ada di `materi/`.
- Folder penyerahan: `Robotika/UTS/` dengan empat subfolder. `UAS/` sudah disiapkan; AI kendali/surveillance, paper, presentasi dan demo adalah lingkup UAS, belum dikerjakan di sini.
- Nama, NIM, kelas **tetap kosong** sesuai permintaan. Jangan menebak identitas dari akun Windows.
- Pengguna masih belajar Arduino/Blynk dan menginginkan bantuan eksekusi serta instruksi fisik singkat. Laporan harus tersedia sebagai PDF, dengan Markdown sebagai sumber edit.
- Screenshot harus asli, terbaca, dan membuka sketch/file yang tepat. Pengguna meminta screenshot pengaturan Blynk juga. Jangan membuat data, screenshot, foto, atau klaim pengujian yang tidak terjadi.

## Status modul

| Modul | Status saat serah-terima | Berkas utama |
| --- | --- | --- |
| 01 Basic | Uji awal selesai; log, grafik, screenshot, PDF 6 halaman. Start belum selalu konsisten; PPR belum dikalibrasi. | [Laporan](UTS/01_Basic_iMCLab/LAPORAN.pdf) |
| 02 Python | Uji awal selesai; 4 CSV utama, 67 DATA; bukti OFF/ON tambahan dan foto kit; PDF 8 halaman. | [Laporan](UTS/02_Python_iMCLab/LAPORAN.pdf) |
| 03 Blynk | Template/datastream/dashboard/device dibuat; 4 screenshot pengaturan; konfigurasi lokal siap; kompilasi lulus. Belum upload/online/uji motor. | [Sketch](UTS/03_Blynk_iMCLab/program/imclab_blynk/imclab_blynk.ino) |
| 04 OTA | Source Edgent lengkap; baseline kompilasi dengan placeholder tersedia. Belum konfigurasi akun OTA/provisioning/shipping. | [Sketch](UTS/04_OTA_Blynk/program/imclab_ota/imclab_ota.ino) |

Markdown dan PDF laporan 03–04 masih rancangan awal. Progres setup terbaru ada di dokumen ini dan `UTS/PROGRES_03_04.md`; **belum dimasukkan ke PDF 03**. Setelah pengujian, integrasikan bukti/hasil lalu regenerasi PDF.

## Perangkat dan lingkungan

- Kit **iMCLab ESP32**, bukan Arduino Uno. Board **ESP32 Dev Module**; FQBN `esp32:esp32:esp32`.
- USB CP210x sudah bekerja pada **COM5** sebelumnya; nomor port dapat berubah. Driver tidak perlu dipasang ulang sebelum memeriksa sambungan/port.
- Label adaptor menurut pengguna **5 V, 2 A**. USB untuk pemrograman/serial, catu motor terpisah. Pastikan operator siap, kit di meja dan poros bebas saat memulai uji fisik baru.
- GPIO PWM=12, IN1=27, IN2=26, sensor RPM=13, LED=2. PWM 30 kHz, 8 bit, 0–255. Serial **115200 baud**.
- Sensor FALLING, debounce 500 us, **PPR=2 masih asumsi dan belum dikalibrasi**. RPM menggunakan pulsa dan durasi aktual. Laporkan pulsa/detik dan “RPM estimasi”.
- Arduino IDE 2.3.10 teramati pada screenshot; CLI lokal 1.3.1; ESP32 core **2.0.17**; Blynk **1.3.2**. Pertahankan baseline ini selama eksperimen.
- Python kendali: `.venv\Scripts\python.exe`, Python 3.12.14, pySerial 3.5.
- CLI `.tools\arduino-cli.exe`; gunakan **`.tools/arduino-cli.yaml`** yang menunjuk `.arduino-data`, `.arduino-downloads`, `.arduino-user`. Konfigurasi CLI milik Arduino IDE berbeda direktori library.

Pengguna pernah membantu awal putaran saat tes manual. Pada pilot otomatis berikutnya dan sweep Python, pengguna mengonfirmasi motor berputar/berhenti tanpa bantuan. Kegagalan start sebelumnya tetap dicatat; jangan menyimpulkan motor selalu normal atau PWM 200 selalu menjamin start. Untuk uji berikutnya mulai dengan pilot singkat PWM 200, kemudian turunkan saat berjalan. Jangan meminta pengguna menyentuh poros saat diberi daya.

LED menurut pengamatan pengguna: hijau catu sensor, hijau deteksi pulsa, merah USB, biru perintah motor. Ini bukan verifikasi skema. LED ON saja tidak membuktikan poros bergerak.

## Hasil 01–02 yang harus dipertahankan

Modul 01: 14 log asli, 135 DATA, termasuk gagal start/penghentian dini. Sweep menyelesaikan 200 dan 160; fase 120 dihentikan saat pulsa hilang, 80 dilewati. Sepuluh pemeriksaan parser saat OFF lulus. Lihat [ringkasan log](UTS/01_Basic_iMCLab/data/ringkasan_log.md).

Modul 02: empat `python_*.csv` memiliki 67 DATA. Sweep `python_sweep_20261019_061241.csv` berisi 15 DATA aktif masing-masing pada 200/160/120. Rata-rata lima sampel akhir **267,2 / 133,8 / 20,8 pulsa/detik**, setara estimasi PPR=2 **8016 / 4014 / 624 rpm**. PWM 120 tidak memenuhi kriteria rentang relatif <=5%, walaupun operator mengonfirmasi tetap berputar pelan tanpa bantuan. Jangan menyebutnya stabil.

Bukti tambahan 02 terpisah dari 67 DATA utama:

- OFF 11:28:27 WIB: screenshot terminal dan log asli tersedia.
- Percobaan 11:33:48 WIB gagal membuka COM5 (`Access is denied`), tanpa TX/RX; bukan uji motor berhasil. Pertahankan log kegagalan.
- ON 11:35:27 WIB: `bukti_on200_20261019_113527.csv`, 11 DATA dengan 5 aktif PWM 200. Pulsa aktif 69, 203, 250, 258, 268 per 1000 ms; estimasi RPM 2070, 6090, 7500, 7740, 8040. Jarak TX ON–OFF 5,198435 detik. DATA akhir PWM=0, RPM=0; exit 0.
- ACK OFF sempat memuat RPM lama; DATA sesudahnya 285, 3, 0 pulsa. ACK/STATUS bisa mengulang sampel, jangan dimasukkan sebagai pengukuran baru.
- [Indeks bukti 02](UTS/02_Python_iMCLab/bukti/README.md) memuat screenshot ON/OFF dan foto kit asli. Foto statis saja tidak membuktikan putaran/RPM.

## Modul 03: akun Blynk yang sudah dikonfigurasi

| Komponen | Nilai |
| --- | --- |
| Organisasi / region | 1175763 / SGP1 |
| Template | iMCLab UTS |
| ID template pada URL | 2581286 |
| Template ID firmware | TMPL6YpVE_Y1G |
| Hardware / koneksi | ESP32 / WiFi |
| Device | iMCLab Modul 03 |
| Device ID | 4277857 |
| Status terakhir | Offline; firmware 03 belum dipasang |

- [Template datastream](https://blynk.cloud/dashboard/1175763/templates/2581286/datastreams)
- [Template dashboard](https://blynk.cloud/dashboard/1175763/templates/2581286/dashboard)
- [Actual device](https://blynk.cloud/dashboard/1175763/global/devices/465755/organization/1175763/devices/4277857/dashboard)

Angka 465755 adalah bagian URL induk. **Device target iMCLab Modul 03 adalah 4277857**; periksa nama/identitas di UI sebelum mengoperasikan.

| Pin | Nama | Tipe / batas | Default | Widget |
| --- | --- | --- | --- | --- |
| V0 | RPM | Double 0–10000, rpm | 0 | Label RPM estimasi, rata tengah |
| V1 | PWM aktual | Integer 0–255 | 0 | Label PWM aktual, rata tengah |
| V2 | Setpoint PWM | Integer 0–255 | 200 | Slider; send on release ON, handle step 1, fine controls ON step 10 |
| V3 | Motor enable | Integer 0–1 | 0 | Switch Motor ON/OFF, ON=1/OFF=0, label ON/OFF tampil |

Datastream/widget sudah disimpan hingga **Changes saved**. Tidak ada chart/history eksperimen yang disiapkan; penyimpanan masih Last Value. Sync latest server value on reconnect tidak aktif. Nilai acak/status Online pada **template preview adalah simulasi UI**, bukan telemetri kit. Nilai 200/0 pada actual device yang offline dapat berasal dari default.

Empat screenshot asli ada di [bukti 03](UTS/03_Blynk_iMCLab/bukti/README.md). Diambil saat modal setting terbuka sebelum Save; sesudahnya widget dan template disimpan. Belum ada screenshot overview datastream lengkap, actual device Online, ON/OFF atau reconnect. Panel browser sempit membuat overview terpotong; gunakan tampilan yang terbaca untuk bukti lanjutan.

## Konfigurasi privat dan kompilasi 03

`UTS/03_Blynk_iMCLab/program/imclab_blynk/secrets.h` **sudah terisi** template, token device milik pengguna, SSID dan password. Jangan meminta kredensial lagi jika file masih tersedia. Periksa hanya status terisi; jangan mencetak isinya ke chat/log/laporan. Nama SSID tidak membuktikan band; verifikasi jaringan nyata 2,4 GHz bila gagal tersambung.

`.gitignore` mengecualikan private header, `.venv/`, `.tools/`, `.arduino-*`, `tmp/`, binary dan `materi/`. **Binary yang dikompilasi dari secrets.h juga mengandung kredensial**; hanya untuk flash lokal, jangan dibagikan ke GitHub. Materi dosen memuat kredensial pihak lain; jangan dicetak, digunakan atau dipublikasikan. Ignore tidak melindungi penyalinan manual.

Kompilasi **19 Oktober 2026 lulus**, exit 0: program **889.333/1.310.720 byte (67%)**, RAM **47.456/327.680 byte (14%)**. [Log sukses](UTS/03_Blynk_iMCLab/bukti/compile_blynk_20261019_host.txt). Percobaan sebelumnya gagal mengekstrak `VCRUNTIME140.dll` karena izin runtime sandbox; [log gagal](UTS/03_Blynk_iMCLab/bukti/compile_blynk_20261019.txt) dipertahankan. Kompilasi host berikutnya berhasil; tidak ada error source yang belum terselesaikan dari proses tersebut.

Build: `tmp/build-imclab-blynk/`; aplikasi `imclab_blynk.ino.bin` **895.904 byte**. Ukuran file bin berbeda dari ukuran program compiler. **Belum di-upload**. Firmware terakhir yang diketahui di kit adalah basic untuk 01/02.

Source 03 startup OFF, terkunci saat Wi-Fi/cloud belum terhubung. V2 mengubah setpoint; V3 enable; V0–V3 dipublikasikan sekitar setiap detik. Saat reconnect motor dimatikan dan status lokal dipublikasikan. Kehilangan koneksi mematikan motor setelah terdeteksi; timeout protokol dapat menunda deteksi. **Belum ada batas latensi terukur**; jangan menyebutnya emergency stop dengan waktu pasti. Serial `0`/`R` masih tersedia, tetapi kendali eksperimen 03 harus berasal dari dashboard.

## Langkah Gemini: upload dan uji 03

Jalankan per tahap, bukan sekaligus. Mulai dengan USB dan enumerasi; motor OFF sampai operator siap.

```powershell
Set-Location 'D:\ryudev_area\robotika'
& '.\.venv\Scripts\python.exe' 'UTS/02_Python_iMCLab/program/kontrol_imclab.py' --list
& '.\.tools\arduino-cli.exe' board list --config-file '.tools/arduino-cli.yaml'
```

Jika tidak ada port, periksa kabel/sambungan bersama pengguna. Jika Access is denied, tutup Serial Monitor/Plotter atau klien Python yang memakai port. Jangan menghentikan semua proses tanpa mengetahui pemiliknya.

Build sudah ada; kompilasi ulang hanya bila source/config berubah atau build hilang:

```powershell
& '.\.tools\arduino-cli.exe' compile --config-file '.tools/arduino-cli.yaml' --fqbn esp32:esp32:esp32 --build-path 'tmp/build-imclab-blynk' 'UTS/03_Blynk_iMCLab/program/imclab_blynk'
```

Isi `$kitPort` berdasarkan enumerasi terbaru (COM5 hanya contoh port sebelumnya), lalu upload. Sintaks `--input-dir` telah diperiksa pada bantuan CLI 1.3.1; upload di bawah **belum dijalankan**.

```powershell
$kitPort = 'COM5'
& '.\.tools\arduino-cli.exe' upload --config-file '.tools/arduino-cli.yaml' --fqbn esp32:esp32:esp32 --port $kitPort --input-dir 'tmp/build-imclab-blynk' 'UTS/03_Blynk_iMCLab/program/imclab_blynk'
```

1. Simpan hasil upload. Buka serial 115200, cari `READY iMCLab Blynk 1.0.0`, verifikasi actual device Online dan keluaran OFF. Screenshot IDE harus membuka `imclab_blynk.ino`.
2. Rekam serial bertimestamp dan screenshot startup OFF. Logger pasif khusus 03 **belum dibuat**. `kontrol_imclab.py --capture` mengirim perintah motor: jangan memakai hasil capture itu sebagai bukti kendali dari dashboard.
3. Saat V3 OFF, ubah V2 ke 120/160 lalu kembali 200; V1 harus tetap 0. Catat hasil actual device.
4. Dengan operator siap, pilot ON **PWM 200 sekitar 2–5 detik**, lalu OFF. Konfirmasi mulai sendiri, pulsa sensor dan berhenti. Jika gagal start/pulsa hilang, OFF dan catat; jangan terus menahan motor macet.
5. Jika pilot berhasil, variasikan saat berjalan 200 -> 160 -> 120, sekitar 15 detik tiap tahap untuk memperoleh minimal 10 DATA pascatransisi bila memungkinkan. Hentikan bila pulsa hilang/ada masalah; jangan membantu putaran dengan tangan.
6. Simpan screenshot dashboard OFF/ON berisi PWM/RPM, foto praktik dan konfirmasi operator. Foto statis bukan pengukuran RPM.
7. Uji putus/sambung jaringan secara terkontrol bila memungkinkan; catat waktu, OFF setelah deteksi, dan tetap OFF setelah reconnect. Jangan memutus jaringan bersama seluruh kampus/rumah. Jika tidak dilakukan, tulis belum diuji.
8. Akhiri OFF. Simpan raw log, analisis hanya DATA (bukan ACK/STATUS), pertahankan kegagalan. Perbarui laporan, pengamatan, bukti, checklist, verifikasi dan PDF.

## Modul 04: rencana setelah 03

1. Baca [asal source dan adaptasi](UTS/04_OTA_Blynk/program/imclab_ota/README.md). Header Edgent, Settings, ImcCore, lisensi dan hash upstream tersedia. Jangan menimpa hook penghentian motor tanpa peninjauan.
2. Menu **Blynk.Air terlihat**, tetapi hak akses/kuota/shipping akun belum diuji. Periksa UI dan akun aktual; jangan mengasumsikan fitur tersedia atau membeli upgrade otomatis.
3. Tentukan template/device OTA. Bila kuota memungkinkan, gunakan target terpisah agar bukti 03 tetap utuh. Jika memakai template yang sama, catat keputusan/provisioning; jangan menghapus device 03 tanpa kebutuhan.
4. Pertahankan V0–V3; tambah **V4 String versi firmware** dan label. Rekam pengaturan serta ID target OTA.
5. Buat `template_config.h` dari [contoh](UTS/04_OTA_Blynk/program/imclab_ota/template_config.example.h); file privat ini **belum ada**. Isi template yang dipilih. **Jangan menyalin token statis/SSID/password 03 ke Edgent**; kredensial didapat melalui provisioning.
6. Versi source awal **1.0.0**. Kompilasi dengan baseline; verifikasi flash dan partisi memiliki dua slot OTA serta binary muat. Jangan gunakan No OTA/Huge APP. Kompilasi placeholder 18 Oktober: 985.025/1.310.720 byte, RAM 51.020; ini bukan bukti upload/provisioning.
7. Upload **1.0.0 melalui USB sekali**, lakukan provisioning akun/Wi-Fi lewat alur Blynk yang tersedia. Verifikasi Online, V4/serial 1.0.0 dan motor OFF. Konsol serial dikelola Edgent; perintah motor basic 0/1 tidak tersedia.
8. Simpan baseline 1.0.0, ubah versi ke **1.0.1**, kompilasi ke folder build berbeda dan catat SHA-256 binary aplikasi. **Jangan meng-upload 1.0.1 melalui USB**: target eksperimen adalah OTA.
9. Motor OFF; pilih **aplikasi `imclab_ota.ino.bin`**, bukan bootloader/partitions/merged. Shipping Blynk.Air ke satu device terverifikasi. Catat versi, waktu WIB, nama/ukuran/hash bin, target dan status.
10. Sesudah reboot, verifikasi dari perangkat: V4/serial **1.0.1**, Online, motor tetap OFF. Status sukses cloud saja belum cukup. Uji ON/OFF singkat setelah operator siap; akhiri OFF.
11. Source mengunci/mematikan motor sebelum OTA dan ketika keluar MODE_RUNNING; perilaku masih perlu diuji. Jika fitur/provisioning/shipping gagal, simpan error dan tulis belum berhasil. Upload USB versi baru bukan bukti OTA.
12. Lengkapi laporan 04, screenshot versi sebelum/sesudah, log asli dan hasil setelah reboot; ekspor serta periksa PDF.

[REFERENSI.md](UTS/REFERENSI.md) berisi dokumentasi resmi yang digunakan saat penyusunan. Periksa dokumentasi/UI terkini bila alur Blynk berubah.

## PDF dan penyerahan

Sumber `UTS/<modul>/LAPORAN.md`; generator [buat_pdf.py](UTS/tools/buat_pdf.py). Pilih modul agar PDF 01/02 tidak terganti. `.venv` kendali hanya dijamin memiliki pySerial. Runtime PDF tersedia:

```powershell
$pdfPython = 'C:\Users\NICO\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe'
& $pdfPython 'UTS/tools/buat_pdf.py' --modul 03
# Jalankan untuk 04 setelah sumber laporannya lengkap:
& $pdfPython 'UTS/tools/buat_pdf.py' --modul 04
```

Fallback paket `.tools/pdf-python` dijelaskan di [panduan ekspor](UTS/tools/README.md). Output `output/pdf/Laporan_UTS_03_Blynk_iMCLab.pdf` atau `Laporan_UTS_04_OTA_Blynk.pdf`, dengan salinan ke folder modul `LAPORAN.pdf`. Render dan periksa semua halaman, caption/tabel/gambar serta kecocokan salinan. Poppler lokal: `C:\Users\NICO\.cache\codex-runtimes\codex-primary-runtime\dependencies\native\poppler\Library\bin\pdftoppm.exe`.

Tidak perlu mengulang tes software 01/02 untuk perubahan dokumentasi. Jika kode kendali berubah, jalankan tes relevan; fixture software bukan data kit. Jangan menandai 03/04 selesai hanya karena kompilasi lulus.

Pemeriksaan akhir menemukan **repositori Git sudah ada**, branch `main`, remote bernama `origin`, commit terakhir `0dfd51e` (First commit). Catatan lama yang mengatakan belum ada Git sudah tidak berlaku. URL/status push remote belum diverifikasi; jangan menginisialisasi ulang atau menimpa remote. Dokumen serah-terima dan bukti 03 masih perubahan lokal/untracked; tidak dibuat commit atau push dalam penyusunan serah-terima ini.

Sebelum penyerahan, periksa identitas (tetap kosong sampai diminta), batas PPR, bukti asli, tautan, dan rahasia. Jangan sertakan materi dosen, private header, log rahasia, cache/build atau binary. Tidak ada dokumen/pesan yang dikirim otomatis ke Gemini; pengguna membuka dokumen ini di sana.

## Prompt singkat untuk Gemini

> Bantu lanjutkan UTS Robotika saya di D:\ryudev_area\robotika. Baca HANDOFF_GEMINI.md dan UTS/PROGRES_03_04.md dahulu. Modul 01–02 sudah diuji dan PDF tersedia. Modul 03 sudah dikonfigurasi di akun Blynk dan berhasil dikompilasi, tetapi belum di-upload karena USB sebelumnya belum tersambung. Lanjutkan dari pemeriksaan port dan upload 03, lalu uji dashboard dengan log/screenshot asli dan perbarui laporan/PDF. Setelah 03 selesai, kerjakan OTA 04 dari 1.0.0 ke 1.0.1 lewat Blynk.Air. Konfigurasi rahasia 03 sudah ada di secrets.h lokal; jangan tampilkan atau unggah isinya. Jangan mengulang setup akun/template, mengarang hasil uji, atau mengisi identitas yang masih kosong. Beri saya instruksi fisik singkat saat diperlukan.
