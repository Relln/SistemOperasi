# JOBSHEET - 13

## LATIHAN
### Latihan 13.1 Implementasi Sistem Backup Lengkap
1. Buat struktur direktori simulasi dengan minimal 10 file yang tersebar di tiga subdirektori:
dokumen/, konfigurasi/, dan media/.
= <img width="1089" height="335" alt="image" src="https://github.com/user-attachments/assets/1b0e384d-e0af-48e7-ae50-d4d3a2938ae3" />

2. Buat skrip backup-harian.sh yang menggunakan rsync dengan –link-dest untuk membuat snapshot harian. Skrip harus mencatat log dengan timestamp ke backup-harian.log.
= <img width="593" height="408" alt="image" src="https://github.com/user-attachments/assets/165d698e-5fb0-4ce0-9419-da3420dba189" />

3. Buat skrip backup-mingguan.sh yang menggunakan tar -czf untuk membuat arsip terkompresi. Skrip harus membuat checksum MD5 dari setiap arsip yang dibuat.
= <img width="490" height="792" alt="image" src="https://github.com/user-attachments/assets/61c790b4-52e3-4798-a7f0-5604de360e2f" />

4. Daftarkan keduanya ke crontab dengan jadwal yang berbeda. Jalankan masing-masing secara
manual dan verifikasi log dan output yang dihasilkan.
= <img width="958" height="209" alt="image" src="https://github.com/user-attachments/assets/c61c8055-a1e2-4441-a3cc-036decd45110" />
<img width="573" height="795" alt="image" src="https://github.com/user-attachments/assets/4327a57e-3084-4232-bb4d-2b9f7abb72c9" />

5. Simulasikan kehilangan file dan lakukan restore dari kedua jenis backup. Dokumentasikan
langkah-langkah restore dan waktu yang dibutuhkan.
= <img width="745" height="177" alt="image" src="https://github.com/user-attachments/assets/cb242742-88bb-47ab-b405-66891f23381c" />

### Latihan 12.2 Analisis Kompresi dan Performa Backup
1. Buat direktori dengan tiga jenis file: teks biasa (10 file .txt masing-masing 100 baris), file konfigurasi .conf, dan file biner simulasi menggunakan dd if=/dev/urandom of=biner.bin
bs=1M count=5.
= <img width="942" height="449" alt="image" src="https://github.com/user-attachments/assets/b4cdcbfb-dcd3-4554-985e-f121fbfdb65c" />

2. Buat tiga arsip dari direktori yang sama menggunakan gzip (-z), bzip2 (-j), dan xz (-J). Ukur
waktu setiap proses menggunakan time.
3. Bandingkan ukuran ketiga arsip dengan ls -lh dan hitung rasio kompresi masing-masing
terhadap ukuran asli.
jawaban no 2 dan 3 =
<img width="528" height="79" alt="image" src="https://github.com/user-attachments/assets/8169a649-b41c-4067-bcd4-4f81c25ffdd5" />
<img width="540" height="146" alt="image" src="https://github.com/user-attachments/assets/027ff45d-8eb1-4e37-bb8d-c3ee97f856c6" />
<img width="509" height="147" alt="image" src="https://github.com/user-attachments/assets/b3d82826-4b9a-4d0e-b73d-a0ba0178ddb8" />
<img width="1077" height="112" alt="image" src="https://github.com/user-attachments/assets/1f7060dd-0c28-4932-a6d9-effa9fd4c0df" />

4. Buat tabel di file analisis-kompresi.txt yang merangkum: jenis kompresi, waktu kompres,
ukuran hasil, dan rasio kompresi
= <img width="644" height="63" alt="image" src="https://github.com/user-attachments/assets/bcdd2ee7-08e3-46b2-bc3f-92564ca38a20" />

Berdasarkan data tersebut, rekomendasikan kompresi yang paling tepat untuk: backup harian
otomatis, arsip jangka panjang, dan backup file biner. Berikan alasan untuk setiap rekomendasi.
= 1. Backup Harian Otomatis: Direkomendasikan menggunakan Gzip (-z)
Alasan: Backup harian membutuhkan kecepatan eksekusi yang tinggi agar tidak membebani performa server saat cron job berjalan di latar belakang.

2. Arsip Jangka Panjang (Long-term Archive): Direkomendasikan menggunakan XZ (-J)
Alasan: Untuk arsip jangka panjang (seperti backup bulanan/tahunan), efisiensi ruang penyimpanan (storage) jauh lebih diprioritaskan daripada kecepatan proses.

3. Backup File Biner: Direkomendasikan menggunakan Gzip atau Tanpa Kompresi (tar biasa)
Alasan: Berkas biner (seperti gambar, video, atau data terenkripsi acak dari /dev/urandom) memiliki tingkat entropi yang sangat tinggi. Berdasarkan hasil pengujian, baik gzip, bzip2, maupun xz tidak mampu memperkecil ukurannya secara signifikan (tertahan di kisaran ~5 MB).

### Latihan 12.3 Disaster Recovery Drill
1. Buat direktori “produksi” dengan struktur lengkap: file konfigurasi, dokumen, dan skrip. Buat
backup penuh menggunakan tar dan simpan checksumnya
=<img width="1343" height="244" alt="image" src="https://github.com/user-attachments/assets/e1ad7c82-be20-4ea4-8994-4af0ebc1760c" />


2. Dokumentasikan kondisi awal: daftar file, ukuran, dan checksum semua file menggunakan find
dan md5sum
= <img width="1282" height="131" alt="image" src="https://github.com/user-attachments/assets/0ebb47fd-32fe-49f3-b7a3-e33b7d227e74" />

3. Simulasikan bencana: hapus seluruh direktori produksi dengan rm -rf.
= <img width="727" height="81" alt="image" src="https://github.com/user-attachments/assets/c37f6d22-8b7d-443e-a1ae-2c1cc0ad0a7e" />

4. Catat waktu mulai pemulihan, lakukan restore lengkap dari backup, dan catat waktu selesai. Hitung RTO aktual
= <img width="829" height="139" alt="image" src="https://github.com/user-attachments/assets/29f38672-52b0-4d83-aaaa-48bc457caac1" />

5. Verifikasi semua file pulih dengan benar menggunakan checksum yang disimpan di langkah 2.
=<img width="827" height="115" alt="image" src="https://github.com/user-attachments/assets/9680a13d-9e32-4411-b72f-d73526c89ec1" />

6. Bandingkan RTO aktual dengan target yang kamu tentukan. Jika lebih lama, identifikasi
bottleneck dan usulkan cara mempercepatnya.
= Analisis Evaluasi Disaster Recovery (Latihan 12.3)
1. Target RTO yang Ditentukan (SLA): * Target: < 5 Detik (Batas toleransi maksimal pemulihan otomatis untuk skala server lokal kecil).

2. RTO Aktual (Hasil Uji Coba):

Berdasarkan output pengujian eksekusi time tar -xzf, waktu riil (Real Time) pemulihan yang tercatat di terminal adalah sebesar 0m0.004 detik (4 milidetik).

3. Perbandingan & Evaluasi Bottleneck:

Hasil: RTO Aktual jauh lebih cepat daripada Target RTO yang ditentukan (4 milidetik vs 5 detik). Pemulihan dinyatakan Sangat Sukses.

Analisis Bottleneck: Pada skala pengujian simulasi ini, tidak ditemukan adanya bottleneck (hambatan) karena ukuran arsip backup yang sangat kecil (<1 MB) dan media penyimpanan menggunakan SSD lokal pada VM, sehingga kecepatan I/O operasi baca-tulis disk berjalan maksimal.

4. Usulan Cara Mempercepat Jika Data Berskala Besar (Skenario Real-World):
Jika di kemudian hari ukuran folder produksi/ membengkak hingga ratusan Gigabyte, proses enkapsulasi .tar.gz akan melambat. Berikut beberapa rekomendasi optimalisasinya:

Gunakan Multi-threading Compression: Mengganti kompresor bawaan tar dengan pigz (Parallel Gzip) agar proses dekompresi memanfaatkan semua core CPU yang tersedia di server.

Arsitektur Hot-Standby (Rsync Mirroring): Dibandingkan mengekstrak file arsip besar dari nol saat bencana, lebih baik memelihara direktori replika di server cadangan menggunakan rsync berkala, sehingga RTO bisa ditekan mendekati 0 detik tinggal mengalihkan trafik (failover).
