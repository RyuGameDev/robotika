# UTS Robotika - Eksperimen Kit iMCLab

**Status:** Modul 01 dan 02 sudah memuat hasil uji kit, log asli, grafik, dan analisis. Modul 02 memiliki empat CSV utama dengan 67 DATA, pemeriksaan OFF dan ON tambahan, screenshot terminal asli, serta foto susunan kit dari operator. Kalibrasi RPM masih perlu dilakukan; start yang belum konsisten tetap dicatat. Modul 03-04 masih draf yang menunggu pengujian fisik. Identitas sengaja dikosongkan sesuai permintaan.

Tujuan rangkaian eksperimen adalah mengembangkan kendali motor DC dari perintah lokal, komunikasi Python, kendali IoT, sampai pembaruan firmware melalui Blynk. Semua analisis hasil yang belum diukur dinyatakan sebagai hipotesis atau analisis kode.

## Modul dan syarat pengumpulan

| Syarat UTS | Berkas utama | Bukti yang perlu dilengkapi |
| --- | --- | --- |
| Basic robotika dengan Kit iMCLab | [01 - Laporan basic](01_Basic_iMCLab/LAPORAN.md) | Data dan screenshot tersedia; lengkapi foto susunan praktik dan kalibrasi sensor |
| iMCLab menggunakan Python | [02 - Laporan Python](02_Python_iMCLab/LAPORAN.md) | Port, konsol teks, CSV, grafik, screenshot terminal OFF/ON, dan foto susunan kit tersedia |
| iMCLab berbasis Blynk | [03 - Laporan Blynk](03_Blynk_iMCLab/LAPORAN.md) | Datastream, dashboard, pembacaan RPM |
| OTA berbasis Blynk | [04 - Laporan OTA](04_OTA_Blynk/LAPORAN.md) | Versi sebelum/sesudah dan hasil Blynk.Air |
| Ringkasan riset per modul | Keempat `LAPORAN.md` | Lengkapi pembahasan dari pengamatan |
| Gambar dan analisis | Diagram dalam tiap laporan | Tambahkan foto/screenshot eksperimen sendiri |

## Mulai dari sini

Untuk langkah pertama yang singkat, buka [Mulai Tes](MULAI_TES.md).

1. Baca [panduan praktik](PANDUAN_PRAKTIK.md), terutama pin, setpoint, dan tombol ON/OFF.
2. Isi [identitas](IDENTITAS.md) dan catat versi perangkat lunak.
3. Jalankan Modul 01, lalu Modul 02 menggunakan firmware yang sama.
4. Buat template dan datastream Blynk untuk Modul 03; isi konfigurasi lokal sendiri.
5. Jalankan Modul 04 setelah modul sebelumnya berhasil. Firmware awal OTA dipasang melalui USB.
6. Simpan foto asli di `bukti/`, data asli di `data/`, lalu lengkapi laporan.
7. Periksa [checklist pengumpulan](CHECKLIST.md) sebelum menyalin `UTS/` dan `UAS/` ke repositori GitHub Robotika.

## Struktur

```text
Robotika/
  UTS/
    IDENTITAS.md
    PANDUAN_PRAKTIK.md
    REFERENSI.md
    CHECKLIST.md
    01_Basic_iMCLab/   # laporan, program Arduino, diagram, data, bukti
    02_Python_iMCLab/  # laporan, program Python, diagram, data, bukti
    03_Blynk_iMCLab/   # laporan, program Arduino, diagram, data, bukti
    04_OTA_Blynk/      # laporan, program Arduino + Edgent, diagram, data, bukti
    tests/            # pengujian perangkat lunak tanpa motor
    tools/            # pembuat diagram
  UAS/
    README.md
```

## Dasar penyusunan

Pin dan alur eksperimen mengikuti `.ino` serta notebook yang diberikan dosen. E-book Jilid 1 membahas **iTCLab untuk suhu**; konsep PWM dan komunikasi serial digunakan kembali untuk iMCLab, tetapi pin pemanas dan sensor suhu tidak digunakan. Jilid 2 dipetakan sebagai bahan lanjutan AI untuk UAS. Lihat [referensi dan peta materi](REFERENSI.md).

Diagram merupakan ilustrasi rancangan yang dibuat untuk laporan ini. Diagram bukan foto alat, bukan tangkapan aplikasi, dan bukan bukti eksperimen telah berhasil. Catatan verifikasi perangkat lunak ada di [VERIFIKASI.md](VERIFIKASI.md).

## Laporan versi PDF

PDF tersedia di masing-masing folder. Modul 01-02 berisi hasil uji awal; Modul 03-04 masih draf sebelum pengujian:

- [Modul 01 - Basic](01_Basic_iMCLab/LAPORAN.pdf)
- [Modul 02 - Python](02_Python_iMCLab/LAPORAN.pdf)
- [Modul 03 - Blynk](03_Blynk_iMCLab/LAPORAN.pdf)
- [Modul 04 - OTA](04_OTA_Blynk/LAPORAN.pdf)

Versi Markdown dipertahankan agar laporan bisa diedit. [Petunjuk memperbarui PDF](tools/README.md) menjelaskan sumber dan proses ekspornya.
