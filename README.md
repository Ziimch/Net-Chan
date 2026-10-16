# Net-Chan

Aplikasi Android untuk menguji dan memantau jaringan secara rinci, dalam satu aplikasi ringan.
Dibuat dengan Kotlin dan Jetpack Compose, tampilan bertema biru dengan background payung di tengah hujan.

## Fitur

| Tab | Fungsi |
|---|---|
| **Info** | Tipe koneksi, IP, gateway, DNS, MTU, Private DNS, detail Wi-Fi (SSID, BSSID, RSSI, frekuensi, kanal, standar Wi-Fi), operator dan jenis jaringan seluler, IP publik, ISP, ASN |
| **Ping** | Min / rata-rata / max, jitter, standar deviasi, packet loss, dan penilaian kualitas |
| **Traceroute** | Daftar hop sampai ke tujuan |
| **Speed Test** | Latensi, jitter, download, dan upload (4 koneksi paralel, server Cloudflare) |
| **DNS** | Waktu resolusi, record A/AAAA, reverse DNS |
| **Port Scan** | Port umum, daftar port, atau rentang (misalnya `1-1024`) |
| **LAN Scan** | Mencari perangkat aktif di subnet /24 |
| **Wi-Fi** | Daftar AP sekitar, kekuatan sinyal, keamanan, kepadatan kanal 2.4 GHz, saran kanal terbaik |
| **Cek Layanan** | Waktu DNS dan respons HTTP ke situs populer |
| **Monitor** | Kecepatan unduh dan unggah real-time |

Hasil setiap tes bisa disalin lewat tombol **Salin**.

## Instalasi

1. Buka tab **Releases** di repo ini, atau tab **Actions** lalu pilih build terbaru.
2. Unduh `Net-Chan-release.apk` (di Actions, artifact diunduh sebagai `.zip`, ekstrak dulu).
3. Buka APK di HP Android (minimal Android 8.0 / API 26) dan izinkan instalasi dari sumber tidak dikenal.

APK ditandatangani dengan debug key, jadi Play Protect bisa menampilkan peringatan. Pilih "Install saja".

## Izin yang diminta

| Izin | Kegunaan |
|---|---|
| Internet, status jaringan | Seluruh tes jaringan |
| Lokasi | Membaca SSID dan memindai Wi-Fi (diwajibkan Android, lokasi tidak disimpan atau dikirim) |
| Phone State | Menampilkan jenis jaringan seluler (4G/5G) |
| Wi-Fi state | Membaca info dan memindai Wi-Fi |

## Koneksi keluar

Aplikasi hanya menghubungi layanan berikut saat fitur terkait dipakai:

- `speed.cloudflare.com` untuk Speed Test
- `ipwho.is` untuk info IP publik
- Host atau situs yang kamu masukkan sendiri, serta daftar situs di tab Cek Layanan

Tidak ada akun, iklan, atau pengumpulan data.

## Build lewat GitHub Actions

1. Push seluruh isi proyek ini ke repo GitHub (`settings.gradle.kts`, `build.gradle.kts`, `gradle.properties`, folder `app/`, dan `.github/workflows/build.yml`).
2. Buka tab **Actions**. Build berjalan otomatis saat push, atau jalankan manual lewat **Run workflow**.
3. Setelah selesai, unduh APK dari bagian **Artifacts**: `Net-Chan-debug` dan `Net-Chan-release`.
4. Untuk membuat rilis otomatis, push tag versi:

```bash
git tag v1.0
git push origin v1.0
```

APK `Net-Chan-release.apk` akan terlampir di halaman Releases.

## Build lokal

Butuh JDK 17, Android SDK 34, dan Gradle 8.9:

```bash
gradle assembleDebug     # app/build/outputs/apk/debug/
gradle assembleRelease   # app/build/outputs/apk/release/
```

## Struktur proyek

```
.
├── .github/workflows/build.yml
├── settings.gradle.kts
├── build.gradle.kts
├── gradle.properties
└── app/
    ├── build.gradle.kts
    └── src/main/
        ├── AndroidManifest.xml
        ├── java/com/netlab/app/
        │   ├── MainActivity.kt   (UI Compose)
        │   ├── NetTools.kt       (ping, traceroute, DNS, port scan, speed test, dll.)
        │   └── NetInfo.kt        (info jaringan, IP publik, scan Wi-Fi)
        └── res/                  (ikon dan background)
```

## Batasan

- Ping dan traceroute memakai perintah `ping` bawaan sistem. Jika perangkat tidak menyediakannya, ping beralih ke TCP connect dan traceroute tidak tersedia.
- Waktu per hop di traceroute hanya perkiraan.
- Android membatasi scan Wi-Fi sekitar 4 kali per 2 menit.
- LAN Scan mungkin tidak mendeteksi perangkat yang memblokir semua koneksi.
- Speed Test memakai kuota data.

## Penggunaan yang bertanggung jawab

Port Scan dan LAN Scan hanya boleh dipakai pada perangkat atau jaringan milik sendiri, atau yang kamu punya izin untuk mengujinya.
