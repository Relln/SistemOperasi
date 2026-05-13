# JOBSHEET - 12

## Latihan
### Latihan 10.1 Audit Layanan dan Analisis Boot
Lakukan audit menyeluruh terhadap layanan yang berjalan di sistem.
1. Jalankan systemctl list-units –type=service –state=running dan catat semua layanan aktif. Pilih tiga layanan yang kamu kenal, periksa status masing-masing dengan
systemctl status, dan jelaskan fungsinya.
= <img width="529" height="274" alt="image" src="https://github.com/user-attachments/assets/bbc36a1d-bb2d-41ee-b4c7-b109b1106670" />
- cron.service = Fungsinya Layanan ini bertanggung jawab untuk menjalankan perintah atau skrip secara otomatis pada waktu yang telah ditentukan (penjadwalan tugas).
- systemd-logind.service = Fungsinya Mengelola proses login pengguna ke dalam sistem.
- snapd.service = Fungsinya Merupakan layanan latar belakang (daemon) yang mengelola paket aplikasi berformat Snap di sistem Linux.


2. Jalankan systemd-analyze blame dan identifikasi lima layanan dengan waktu inisialisasi terlama. Tampilkan hasilnya menggunakan pipeline: systemd-analyze blame | head -5.
<img width="301" height="49" alt="image" src="https://github.com/user-attachments/assets/3bb7a079-5113-4017-b663-15f72937e1f9" />
- cloud-config.service: Memakan waktu 2 menit 49.341 detik. Ini adalah layanan yang biasanya digunakan pada lingkungan cloud untuk mengatur konfigurasi awal sistem saat booting.
- fwupd-refresh.service: Memakan waktu 12.369 detik. Layanan ini bertugas untuk menyegarkan metadata guna memeriksa pembaruan firmware pada perangkat keras kamu.
- man-db.service: Memakan waktu 7.630 detik. Layanan ini berfungsi memperbarui indeks database untuk halaman manual (man pages), sehingga pencarian dokumentasi perintah Linux tetap akurat.
- logrotate.service: Memakan waktu 6.331 detik. Digunakan untuk mengelola file log agar tidak memenuhi ruang penyimpanan dengan cara mengompres, menghapus, atau merotasi log lama.
- apt-daily-upgrade.service: Memakan waktu 5.172 detik. Layanan otomatis yang memeriksa dan mengunduh pembaruan paket aplikasi di latar belakang.


3. Jalankan systemctl –failed dan dokumentasikan hasilnya. Jika ada layanan yang gagal, cari tahu penyebabnya dengan journalctl -u nama-layanan -n 30.
<img width="253" height="34" alt="image" src="https://github.com/user-attachments/assets/00e69211-ead9-4aa6-8edf-a1eb4c188cee" />
Tidak ada layanan yang gagal, Semua unit sistem yang dimuat (loaded) berjalan dengan sebagaimana mestinya atau berhenti dengan normal tanpa menimbulkan eror.


### Latihan 10.2 Layanan Kustom dengan Restart Otomatis
Buat layanan systemd kustom yang mendemonstrasikan fitur restart otomatis.
1. Buat skrip Bash (referensi Bab 7) bernama monitor-disk.sh yang setiap 30 detik menuliskan penggunaan disk ke berkas log. Gunakan df -h dan date.
= <img width="464" height="389" alt="image" src="https://github.com/user-attachments/assets/5c9e8851-c3ed-4d77-b2a6-2e53575a9131" />


2. Buat berkas unit /etc/systemd/system/monitor-disk.service untuk menjalankan skrip tersebut dengan konfigurasi: Restart=always, RestartSec=5s, dan berjalan sebagai pengguna kamu sendiri.
=<img width="464" height="391" alt="image" src="https://github.com/user-attachments/assets/1c3463d3-e467-44b1-8230-d94932ef90f5" />


3. Aktifkan dan jalankan layanan. Verifikasi dengan systemctl status dan pastikan log masuk ke journal.
= <img width="640" height="184" alt="image" src="https://github.com/user-attachments/assets/f8cb5605-7d55-4579-8755-a4c980e4e080" />


4. Simulasikan crash dengan membunuh proses secara paksa (kill -9), tunggu 10 detik, dan verifikasi bahwa layanan hidup kembali secara otomatis.
= <img width="640" height="191" alt="image" src="https://github.com/user-attachments/assets/d7ad4a20-9216-4bbc-b72f-36308a66cfd3" />
PID Awal = 1242728 menjadi PID = 1256769

5. Bersihkan: nonaktifkan layanan dan hapus berkas unit setelah selesai.
= <img width="368" height="41" alt="image" src="https://github.com/user-attachments/assets/c883e2b4-d370-47c4-9902-019bea56c270" />


### Latihan 10.3 Investigasi Log dan Keamanan SSH
Analisis log sistem dan tingkatkan keamanan konfigurasi SSH.
1. Gunakan journalctl -b -p err untuk menemukan semua error sejak boot terakhir. Simpan hasilnya ke berkas dan hitung jumlah baris dengan wc -l.
= <img width="320" height="23" alt="image" src="https://github.com/user-attachments/assets/99591719-f382-404c-b322-651b699331f5" />


2. Lakukan tiga perubahan keamanan pada /etc/ssh/sshd_config: tambahkan PermitRootLogin no, MaxAuthTries 3, dan LoginGraceTime 30. Ikuti alur aman: backup, edit, validasi sshd-t, reload.
= <img width="464" height="400" alt="image" src="https://github.com/user-attachments/assets/f7e57db9-04b8-4332-8ab1-3568f517ed9f" />


3. Setelah reload, verifikasi tiga hal: layanan masih berjalan (systemctl status ssh), port masih mendengarkan (ss -tlnp | grep ssh), dan konfigurasi baru terbaca (grep -E
"PermitRoot|MaxAuth|GraceTime" /etc/ssh/sshd_config).
= <img width="413" height="50" alt="image" src="https://github.com/user-attachments/assets/2ce2f34b-6662-43c2-98b0-331a268c749c" />


4. Kembalikan konfigurasi SSH ke kondisi semula menggunakan berkas backup.
= <img width="385" height="14" alt="image" src="https://github.com/user-attachments/assets/7687e40a-a434-4a4a-8c57-9843c1d91ade" />


