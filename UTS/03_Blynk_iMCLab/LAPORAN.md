# Modul 03 - Kendali dan Monitoring iMCLab Berbasis Blynk

Identitas: [identitas mahasiswa](../IDENTITAS.md). Tanggal eksperimen: **belum diisi**.

## Ringkasan riset

Modul ini menghubungkan kendali motor lokal dengan dashboard Blynk melalui Wi-Fi. Program kelas `imclab_Blynk_kelas.ino` dijadikan dasar pemetaan pin virtual, sensor pulsa, dan PWM. Pengembangan memisahkan setpoint dari status ON/OFF, menghitung RPM menggunakan durasi aktual, serta menghentikan keluaran ketika kehilangan koneksi terdeteksi. Dokumen ini memuat rancangan, analisis kode, dan rencana pengujian; belum ada klaim bahwa dashboard atau motor telah berhasil diuji.

## Tujuan dan pertanyaan penelitian

Tujuannya adalah mengendalikan motor melalui slider dan switch serta memantau RPM dari sensor. Pertanyaan penelitian: apakah setpoint dashboard diterapkan secara konsisten, apakah nilai RPM mencerminkan perubahan kondisi motor, dan bagaimana sistem berperilaku ketika koneksi putus lalu tersambung lagi?

Hipotesis: pada jaringan yang tersedia, perubahan setpoint dan status ON/OFF diterima ESP32 dan tercermin pada telemetri berikutnya. Jeda aktual dan kemungkinan pembacaan yang terlambat harus diamati, bukan diasumsikan nol.

## Dasar teori dan materi

Slide dosen *Integrasi AI dan IoT pada Sistem Kendali Cerdas*, slide 4-5, menjelaskan perangkat fisik, internet, dan cloud serta penggunaan Blynk dengan iTCLab/iMCLab. Pada eksperimen ini motor dan sensor menjadi perangkat fisik, ESP32/Wi-Fi menyediakan koneksi, dan Blynk menghubungkan dashboard dengan perangkat.

Pin virtual berbeda dari GPIO. V2 adalah saluran nilai setpoint pada aplikasi, sedangkan GPIO12 merupakan keluaran PWM listrik pada board. V0 membawa RPM, V1 PWM aktual, V2 setpoint, dan V3 status enable. Tidak ada metode AI pada modul UTS ini; pembacaan sensor dan pengiriman cloud juga belum menjadikan kendalinya loop tertutup.

![Diagram arsitektur Blynk iMCLab](gambar/arsitektur.svg)

**Gambar 1.** Rancangan komunikasi dashboard-cloud-ESP32. Jalur perintah membawa setpoint dan enable, sedangkan jalur monitoring mengembalikan RPM dan keluaran PWM. Diagram tidak menunjukkan latensi terukur dan bukan screenshot dashboard yang telah diuji.

## Metode dan konfigurasi

Alat: Kit iMCLab, catu motor, komputer, Wi-Fi 2,4 GHz, akun Blynk, dan perangkat dashboard. Konfigurasi GPIO mengikuti Modul 01. Gunakan [sketch Blynk](program/imclab_blynk/imclab_blynk.ino) dan isi konfigurasi dari `secrets.example.h` menjadi `secrets.h`.

| Datastream | Tipe | Fungsi |
| --- | --- | --- |
| V0 | Double | RPM sensor |
| V1 | Integer 0..255 | PWM yang benar-benar diminta ke driver |
| V2 | Integer 0..255 | Setpoint dari slider |
| V3 | Integer 0..1 | Switch OFF/ON |

1. Buat template/device sendiri, datastream, dan widget sesuai [panduan](../PANDUAN_PRAKTIK.md).
2. Upload program melalui USB. Pastikan startup OFF dan tunggu status online.
3. Saat OFF, ubah V2 ke 120. Periksa setpoint berubah tetapi V1 tetap nol.
4. Aktifkan V3, amati motor, nilai V1, dan pembacaan V0. Ulangi pada beberapa PWM dan catat minimal 10 sampel setelah transisi.
5. Matikan V3. Periksa V1 menjadi nol, sedangkan RPM dapat turun bertahap karena inersia.
6. Uji putus-sambung Wi-Fi dengan kondisi terkontrol. Catat jeda sampai motor OFF dan keadaan setelah reconnect. Gunakan pencatat waktu/video jika ingin mengukur respons.

Variabel bebas adalah setpoint dan kondisi koneksi; variabel terikat adalah PWM aktual, RPM, dan respons setelah reconnect. Catu, beban, penempatan sensor, dan metode pengukuran waktu harus dijaga konsisten.

## Analisis program

Materi awal menggunakan `rev / HOLES x 60` ketika selang loop setidaknya 1000 ms. Jika perhitungan baru berlangsung setelah 1200 ms, rumus tersebut memperlakukan 1,2 detik sebagai satu detik. Program UTS menggunakan `pulsa x 60000 / (PPR x dt_ms)`. Misalnya secara teori 40 pulsa dan PPR=2 dalam 1200 ms setara 1000 rpm; perhitungan satu detik akan memberikan 1200 rpm, atau 20% lebih tinggi. Ini contoh matematis, bukan data eksperimen.

Program tidak menyamakan setpoint positif dengan status ON. Ketika V3 OFF, V2 masih menyimpan nilai terakhir tetapi PWM aktual tetap nol. Saat reconnect, nilai lokal dan status OFF dipublikasikan sehingga perintah ON lama tidak dipulihkan. Telemetri dijadwalkan kira-kira setiap satu detik; tidak dikirim pada setiap iterasi loop.

Putus cloud dapat baru diketahui setelah timeout protokol. Oleh sebab itu penghentian ketika disconnect terdeteksi tidak boleh disebut emergency stop dengan batas waktu pasti. Pengaruh penundaan pembacaan, noise encoder, nilai PPR, serta kuantisasi sekitar 30 rpm per pulsa pada jendela satu detik harus dibahas ketika data tersedia.

## Hasil pengamatan

**Belum ada hasil koneksi/dashboard atau pengukuran alat.** Lengkapi [lembar pengamatan](data/pengamatan.md) dan sertakan bukti asli.

| Skenario | V2 setpoint | V3 enable | V1 aktual | V0 RPM | Kondisi fisik / waktu respons |
| --- | --- | --- | --- | --- | --- |
| Boot dan online | | | | | |
| Slider saat OFF | | | | | |
| ON pada 120 | | | | | |
| ON pada 200 | | | | | |
| OFF | | | | | |
| Koneksi putus | | | | | |
| Reconnect | | | | | |

Tambahkan gambar pengaturan datastream dan dashboard ke [folder bukti](bukti/README.md), dengan token/password disamarkan.

## Pembahasan dan simpulan sementara

Rancangan memungkinkan kendali serta monitoring melalui cloud dan membedakan nilai setpoint, keluaran PWM, serta kecepatan terukur. Keberhasilan komunikasi, kecenderungan PWM-RPM, dan perilaku reconnect masih harus dibuktikan. Setelah pengujian, gunakan hasil tabel untuk menilai apakah data dashboard sesuai keadaan alat dan jelaskan keterbatasan interval pembaruan.

## Rujukan

`imclab_Blynk_kelas.ino`, slide dosen 4-5, serta [referensi lengkap](../REFERENSI.md). Program UTS merupakan adaptasi dengan perubahan perilaku yang dijelaskan di atas.
