# JOBSHEET-2

## Praktikum 2.1 — Identifikasi CPU dan Memori
1. ![Alt text for screen readers]()
2. ![Alt text for screen readers]()
3. ![Alt text for screen readers]()

### Latihan 2.1
Catat: (1) jumlah CPU(s), core/thread, (2) total RAM, (3) total swap. Jelaskan perbedaan RAM vs swap dalam 2–3 kalimat.
- Jumlah CPU(s): 1 unit.
- Core per Socket: 1 Core.
- Thread per Core: 1 Thread.
- Total RAM: 1.9 GiB (terdeteksi sebagai 1.92 GB pada htop).
- Total Swap: 2.0 GiB.
= Perbedaan RAM vs Swap
RAM (Random Access Memory) adalah memori fisik utama yang bekerja sangat cepat untuk menyimpan data aplikasi yang sedang aktif digunakan oleh CPU. Sementara itu, Swap adalah area di dalam hard drive atau SSD yang digunakan oleh sistem operasi sebagai "memori cadangan" ketika kapasitas RAM fisik sudah penuh, namun dengan kecepatan akses yang jauh lebih lambat dibandingkan RAM.

## Praktikum 2.2 — Identifikasi Perangkat PCI/USB dan Driver
1. ![Alt text for screen readers]()
2. ![Alt text for screen readers]()
3. ![Alt text for screen readers]()
4. ![Alt text for screen readers]()
5. ![Alt text for screen readers]()

### Latihan 2.2
Temukan 1 perangkat PCI (misal NIC) dan tuliskan: Vendor:Device ID (angkaheksadesimal), nama driver/modul kernel, dan deskripsi singkat fungsinya.
- Vendor:Device ID: 8086:100e
Keterangan: 8086 adalah ID untuk Intel Corporation.
- Nama Driver/Modul Kernel: e1000
- Deskripsi Fungsi:
Perangkat ini adalah Intel Corporation 82540EM Gigabit Ethernet Controller. Fungsinya adalah sebagai antarmuka jaringan fisik yang memungkinkan sistem operasi untuk terhubung ke jaringan kabel (Ethernet) dan mengirim/menerima data pada kecepatan Gigabit melalui driver e1000.

## Praktikum 2.3 — Identifikasi Storage dan Filesystem


## Praktikum Praktikum 2.4 — Melihat Modul Aktif dan Informasinya

## Praktikum 2.5 — Konfigurasi Auto-load dan Blacklist

## Praktikum 2.6 — Mengenali Block vs Character Device

### Latihan 2.3
Dari output ls -l, jelaskan perbedaan penanda file untuk block device dan character device. (Hint: karakter pertama pada permission string)
Pada output perintah ls -l yang Anda jalankan, terdapat dua contoh perangkat dengan penanda yang berbeda pada karakter pertama string perizinan (permission string):
- Block Device (/dev/sda)
Penanda: Karakter pertama adalah b (seperti pada brw-rw----).
Deskripsi: Simbol b menunjukkan bahwa /dev/sda adalah block device. Perangkat ini mengirimkan data dalam bentuk blok (sekumpulan byte) dan biasanya memiliki kemampuan buffering, seperti pada Hard Disk atau SSD.
- Character Device (/dev/tty)
Penanda: Karakter pertama adalah c (seperti pada crw-rw-rw-).
Deskripsi: Simbol c menunjukkan bahwa /dev/tty adalah character device. Perangkat ini berkomunikasi dengan mengirimkan data karakter demi karakter secara berurutan tanpa proses buffering yang besar, contohnya adalah terminal atau keyboard.

## Praktikum 2.7 — Melihat Informasi udev

## Praktikum 2.8 — Membuat Workspace Praktikum

## Praktikum 2.9 — Pencarian Pola dengan grep

### Latihan 2.4
Gunakan grep untuk menampilkan hanya baris yang mengandung INFO atau WARN dari data.log. (Hint: gunakan grep -E dengan pola alternatif)

## Praktikum 2.10 — Substitusi dengan sed (Aman di File Latihan)

## Praktikum 2.11 — Ekstraksi Kolom dengan awk

## Praktikum 2.12 — Melihat Proses dengan ps

## Praktikum 2.13 — Monitoring Real-time dengan top

## Praktikum 2.14 — Menghentikan Proses dengan kill

## Praktikum 2.15 — Cek Disk, Load, dan Service

## Praktikum 2.16 — Monitoring Port dan Koneksi (Network Basics)

### Latihan 2.5
Pilih satu port yang listening dari output ss -tulpn(misal port 22), lalu tuliskan service/proses yang membukanya. Jelaskan kegunaan port tersebut
secara singkat. 
Analisis Port 22
Berikut adalah rincian mengenai port tersebut berdasarkan data pada terminal:
- Port: 22
- Protokol: TCP
- Service/Proses: systemd (dengan PID 1)
- Kegunaan Port 22 secara Singkat
Port 22 adalah port standar yang digunakan untuk layanan SSH (Secure Shell). Kegunaan utamanya meliputi:
> Remote Access: Memungkinkan pengguna untuk masuk (login) dan mengakses terminal komputer atau server dari jarak jauh secara aman.
> Enkripsi Data: Seluruh komunikasi antara klien dan server dienkripsi, sehingga data seperti password tidak bisa dicuri oleh pihak lain di jaringan.
> Transfer File: Port ini juga digunakan oleh protokol SFTP (Secure File Transfer Protocol) dan SCP untuk mengirim file antar perangkat dengan tingkat keamanan yang tinggi.

## Latihan
### Latihan 2.A
Jalankan lspci -nnk. Pilih 1 perangkat PCI dan tuliskan: nama perangkat, ID vendor:device, dan kernel driver in use.
Detail Perangkat PCI Terpilih
- Nama Perangkat: Intel Corporation 82540EM Gigabit Ethernet Controller
- ID Vendor:Device: [8086:100e]
- Kernel driver in use: e1000

### Latihan 2.B
Tentukan device root filesystem dengan findmnt /. Lalu cocokkan dengan lsblk -f dan tuliskan tipe filesystem serta UUID-nya.
 = Berdasarkan perintah findmnt /, terlihat bahwa target / (root) bersumber dari device berikut:
Source Device: /dev/mapper/ubuntu--vg-ubuntu--lv. Setelah mencocokkan source device tersebut dengan tabel output lsblk -f, didapatkan rincian sebagai berikut:
Tipe Filesystem (FSTYPE): ext4
UUID: 809f6ff3-bc95-409f-a3a4-3a4de3ea0511

### Latihan 2.C
Buat file server.log berisi minimal 10 baris dengan variasi kata: INFO, WARN, ERROR. Gunakan grep untuk menampilkan hanya baris ERROR.

### Latihan 2.D
Gunakan sed untuk mengganti semua kata server menjadi node pada file latihan. Tunjukkan sebelum dan sesudah.

### Latihan 2.E
Gunakan df -h lalu awk untuk menampilkan filesystem yang penggunaan disk di atas 70%.

### Latihan 2.F
Jalankan sleep 600 &. Temukan PID-nya dengan ps. Hentikan dengan SIGTERM. Jelaskan beda SIGTERM vs SIGKILL.

### Latihan 2.G
Gunakan systemctl –failed. Jika tidak ada yang gagal, pilih satu service aktif (misal ssh) dan tampilkan status serta 30 baris log terakhirnya.

 

