# JOBSHEET - 13

## 1.7 Latihan
### Latihan 10.1 Audit Layanan dan Analisis Boot
1. Jalankan systemctl list-units –type=service –state=running dan catat semua layanan aktif. Pilih tiga layanan yang kamu kenal, periksa status masing-masing dengan
systemctl status, dan jelaskan fungsinya.
= <img width="529" height="272" alt="image" src="https://github.com/user-attachments/assets/cf1b1803-8e19-43ac-9b80-f6faaf29e664" />
3 Layanan yang dipilih:
- cron.service
Status: ```bash
systemctl status cron.service
Fungsi: Layanan ini berfungsi sebagai job scheduler otomatis di latar belakang (daemon). Tugasnya adalah mengeksekusi perintah, skrip, atau tugas-tugas terprogram lainnya pada waktu dan interval yang telah ditentukan melalui konfigurasi crontab (misalnya untuk backup berkala atau pembersihan log otomatis).
- ssh.service
Status: ```bash
systemctl status ssh.service
Fungsi: Layanan ini merupakan OpenBSD Secure Shell server. Fungsinya adalah menyediakan akses masuk (login) dan komunikasi data antar-mesin secara jarak jauh (remote) secara aman dan terenkripsi menggunakan protokol SSH.
- snapd.service
Status: ```bash
systemctl status snapd.service
Fungsi: Layanan Snap Daemon yang bertugas untuk mengelola, memasang, memperbarui, dan menjalankan paket aplikasi bersistem Snap (manajer paket universal dari Canonical) di lingkungan sistem operasi Ubuntu/Linux.

2. Jalankan systemd-analyze blame dan identifikasi lima layanan dengan waktu inisialisasi
terlama. Tampilkan hasilnya menggunakan pipeline: systemd-analyze blame | head -5.
= <img width="301" height="49" alt="image" src="https://github.com/user-attachments/assets/b3a8e28f-3063-4f70-8ec4-82e8b8d154b7" />
NO  Waktu         | Nama Layanan
1) 14min 9.030s   | apt-daily-upgrade.service
2) 10min 53.922s  | apt-daily.service
3) 2min 49.341s   | cloud-config.service
4) 10.255s        | fwupd-refresh.service
5) 7.663s         | motd-news.service

3. Jalankan systemctl –failed dan dokumentasikan hasilnya. Jika ada layanan yang gagal, cari
tahu penyebabnya dengan journalctl -u nama-layanan -n 30
= <img width="248" height="34" alt="image" src="https://github.com/user-attachments/assets/4173235b-d010-4cf5-a19a-13749857b0f5" />
Output di atas menunjukkan bahwa tidak ada (zero) layanan yang berada dalam status gagal (failed) pada sistem. Seluruh unit layanan yang dikelola oleh systemd telah dimuat (loaded) dan beroperasi secara normal tanpa kendala.
Karena tidak ditemukan adanya layanan yang gagal, maka langkah investigasi lanjutan menggunakan perintah journalctl -u nama-layanan -n 30 tidak perlu dilakukan. Sistem dipastikan dalam kondisi bersih dan berjalan dengan stabil.


### Latihan 10.2 Layanan Kustom dengan Restart Otomatis
1. Buat skrip Bash (referensi Bab 7) bernama monitor-disk.sh yang setiap 30 detik menuliskan
penggunaan disk ke berkas log. Gunakan df -h dan date
= <img width="490" height="394" alt="image" src="https://github.com/user-attachments/assets/8e71191b-c5b6-451c-a173-5bd08c6fef70" />

2. Buat berkas unit /etc/systemd/system/monitor-disk.service untuk menjalankan skrip
tersebut dengan konfigurasi: Restart=always, RestartSec=5s, dan berjalan sebagai pengguna kamu sendiri.
= <img width="493" height="392" alt="image" src="https://github.com/user-attachments/assets/a07621b0-d66e-4b38-a087-04f296fc15d5" />

3. Aktifkan dan jalankan layanan. Verifikasi dengan systemctl status dan pastikan log masuk
ke journal
= <img width="643" height="386" alt="image" src="https://github.com/user-attachments/assets/ebd41566-0f32-434d-b34f-952818d3f570" />

4. Simulasikan crash dengan membunuh proses secara paksa (kill -9), tunggu 10 detik, dan
verifikasi bahwa layanan hidup kembali secara otomatis.
= <img width="641" height="202" alt="image" src="https://github.com/user-attachments/assets/f374c852-eb15-4fc1-b077-f0c31a682e1f" />

5. Bersihkan: nonaktifkan layanan dan hapus berkas unit setelah selesai.
=<img width="367" height="47" alt="image" src="https://github.com/user-attachments/assets/b4efcfa6-10b6-4639-8cc7-88d060af59d9" />


### Latihan 10.3 Investigasi Log dan Keamanan SSH
1. Gunakan journalctl -b -p err untuk menemukan semua error sejak boot terakhir. Simpan
hasilnya ke berkas dan hitung jumlah baris dengan wc -l.
= <img width="330" height="24" alt="image" src="https://github.com/user-attachments/assets/4c9e45bc-2884-4364-b8de-748340029098" />
Setelah dihitung, ditemukan jumlah baris sebanyak 69 baris error

2. Lakukan tiga perubahan keamanan pada /etc/ssh/sshd_config: tambahkan PermitRootLogin
no, MaxAuthTries 3, dan LoginGraceTime 30. Ikuti alur aman: backup, edit, validasi sshd
-t, reload
= <img width="476" height="395" alt="image" src="https://github.com/user-attachments/assets/dac9643d-da13-4c62-ba50-cab31d094231" />

3. Setelah reload, verifikasi tiga hal: layanan masih berjalan (systemctl status ssh), port
masih mendengarkan (ss -tlnp | grep ssh), dan konfigurasi baru terbaca (grep -E
"PermitRoot|MaxAuth|GraceTime" /etc/ssh/sshd_config).
= <img width="368" height="202" alt="image" src="https://github.com/user-attachments/assets/1341031c-aadb-4fb1-a276-f28ab137f03d" />
<img width="473" height="41" alt="image" src="https://github.com/user-attachments/assets/2e6f0c88-197b-4441-8a73-6c5168cd056b" />

4. Kembalikan konfigurasi SSH ke kondisi semula menggunakan berkas backup.
= <img width="389" height="23" alt="image" src="https://github.com/user-attachments/assets/4b8650d5-303c-4711-beb5-023c3eb3ea63" />



