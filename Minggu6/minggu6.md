# JOBSHEET-6

## Praktikum 6.1 — Melihat Proses dan Thread
### Latihan 6.1
Jalankan ps aux dan amati outputnya:
1. Berapa total proses yang berjalan? Proses apa yang memiliki PID terkecil?
= bash dengan PID 13712
![Alt text](images/Latihan-6.1.1.png)

2. Jalankan pstree -p dan temukan proses bash Anda. Proses apa yang menjadi induk (PPID) dari bash tersebut?
= PID bash 13712, dan induk PPID 13687
![Alt text](images/Latihan-6.1.2.png)

3. Bandingkan output ps aux dan ps aux -L. Apa perbedaan yang Anda lihat?
= munculnya dua kolom yaitu LWP dan NLWP kemudian ps aux hanya menampilkan daftar proses sedangkan ps aux -L menampilkan setiap thread secara individu
![Alt text](images/Latihan-6.1.3.png)

## Praktikum 6.2 — Mengamati Siklus Hidup Proses
### Latihan 6.2
1. Jalankan sleep 120 & dan amati kolom STAT pada ps aux. Kondisi
apa yang ditampilkan? Mengapa proses sleep berada di kondisi tersebut?
= Pada kolom STAT untuk proses sleep (PID 32076), terlihat kode S, karena Perintah sleep 120 secara sengaja menginstruksikan sistem untuk menangguhkan (menidurkan) proses tersebut selama 120 detik.
![Alt text](images/Latihan-6.2.1.png)

2. Jalankan beberapa perintah yang berhasil dan yang gagal, lalu catat exit
code masing-masing. Pola apa yang Anda temukan?
= ![Alt text](images/Latihan-6.2.2.png)
![Alt text](images/Latihan-6.2.2(1).png)

## Praktikum 6.3 — Mengatur Prioritas Proses
### Latihan 6.3
1. Jalankan nice -n 5 sleep 200 & dan verifikasi nilai NI-nya dengan ps.
=![Alt text](images/Latihan-6.3.1.png)

2. Ubah nilai nice menjadi 10 menggunakan renice, lalu verifikasi kembali.
=![Alt text](images/Latihan-6.3.2.png)

3. Coba ubah nilai nice menjadi -5 tanpa sudo. Apa yang terjadi? Mengapa
Linux membatasi hal ini untuk user biasa?
= karena proses dengan PID 14059 sudah selesai atau tidak ada saat Anda menjalankan perintah renice kemudian dibatasi karena untuk Mencegah Monopoli CPU, Mencegah DOS (Denial of Service) Internal, dan Filosofi "Berbagi"
![Alt text](images/Latihan-6.3.3.png)

## Praktikum 6.4 — Mengirim Sinyal ke Proses
### Latihan 6.4
1. Jalankan sleep 400 &, kirim SIGSTOP, dan amati perubahan kolom STAT. Kondisi apa yang muncul?
= Kondisi yang muncul kode pada kolom STAT akan berubah menjadi T
![Alt text](images/Latihan-6.4.1.png)

2. Kirim SIGCONT dan verifikasi proses kembali berjalan.
= ![Alt text](images/Latihan-6.4.2.png)

3. Hentikan proses dengan SIGTERM lalu verifikasi sudah tidak ada. Kapan
Anda memilih SIGKILL daripada SIGTERM?
![Alt text](images/Latihan-6.4.3.png)
= SIGTERM : Digunakan sebagai pilihan pertama. Memberi waktu bagi aplikasi untuk menyimpan data, menutup file, dan membersihkan memori sebelum mati.
SIGKILL : Digunakan sebagai pilihan terakhir jika SIGTERM gagal. Ini akan memaksa sistem operasi untuk langsung mencabut proses dari memori tanpa peringatan.

## Praktikum 6.5 — Manajemen Job Foreground dan Background
### Latihan 6.5
1. Jalankan top di foreground. Apa yang terjadi di terminal?
= ![Alt text](images/Latihan-6.5.1.png)

2. Tekan Ctrl+Z dan cek statusnya dengan jobs. Kondisi apa yang ditampilkan?
= ![Alt text](images/Latihan-6.5.2.png)

3. Pindahkan ke background dengan bg. Apakah top dapat berjalan dengan baik di background? Mengapa?
= Secara teknis prosesnya berjalan, tetapi tidak akan berfungsi dengan baik. arena top adalah aplikasi interaktif yang membutuhkan akses terus-menerus ke terminal (stdout/stderr) untuk memperbarui tampilan visualnya.
![Alt text](images/Latihan-6.5.3.png)

4. Kembalikan ke foreground dengan fg, lalu keluar dengan q
=![Alt text](images/Latihan-6.5.4.png)

## Praktikum 6.6 — Pemantauan Proses
### Latihan 6.6
1. Gunakan ps aux –sort=%mem untuk menemukan proses yang menggunakan memori paling banyak di VM Anda. Proses apa itu?
Proses sistem seperti containerd, node (jika menjalankan diweb), atau java jika sedang menjalankan aplikasi berbasis Java.
![Alt text](images/Latihan-6.6.1.png)

2. Di dalam top, tekan 1 . Apa yang berubah pada tampilan? Mengapa informasi ini berguna?
= Tampilan ringkasan CPU di bagian atas akan berubah dari satu baris rata-rata menjadi rincian per core/prosesor
Karena kita bisa tahu jika ada satu core yang bekerja 100% sementara core lainnya menganggur dan Sangat berguna bagi programmer untuk melihat apakah aplikasi mereka benar-benar menggunakan multi-threading (menyebar ke semua core) atau hanya terjebak di satu core saja.
![Alt text](images/Latihan-6.6.2.png)

3. Di dalam htop, navigasikan ke proses sshd menggunakan tombol panah. Tekan F9 dan amati opsi sinyal yang tersedia
= ![Alt text](images/Latihan-6.6.3.png)



## Latihan
### Latihan 6.A
Eksplorasi Proses Sistem
1. Jalankan ps aux –forest dan temukan proses dengan PID 1. Apa nama dan fungsi proses tersebut dalam sistem Linux modern?
= ![Alt text](images/Latihan-6A.1.png)
Nama prosesnya adalah systemd
Fungsinya sebagai Init Process, Induk Semua Proses dan manajemen layanan.

2. Hitung berapa proses yang dimiliki oleh user root dan berapa yang dimiliki oleh user Anda. Mengapa root memiliki lebih banyak proses?
= ![Alt text](images/Latihan-6A.2.png)
Karena Layanan latar belakang (Daemons), Manajemen Hardware, dan Infrastruktur Multi-user

3. Temukan semua proses yang berada dalam kondisi S. Mengapa sebagian besar proses di sistem berada dalam kondisi ini?
= ![Alt text](images/Latihan-6A.3.png)
Karena efisiensi sumber daya

### Latihan 6.B
Simulasi Manajemen Job
1. Jalankan tiga perintah sleep dengan durasi 100, 200, dan 300 detik di background. Verifikasi ketiganya dengan jobs.
= ![Alt text](images/Latihan-6B.1.png)

2. Bawa job kedua ke foreground, jeda dengan Ctrl+Z , lalu kembalikan ke background dengan bg.
= ![Alt text](images/Latihan-6B.2.png)

3. Hentikan job pertama dengan kill %1. Tampilkan kembali daftar job. Berapa job yang tersisa?
= ![Alt text](images/Latihan-6B.3.png)

### Latihan 6.C
Prioritas dan Sinyal
1. Jalankan dua proses sleep: satu dengan nice +5 dan satu dengan nice +15. Verifikasi nilai NI keduanya dengan ps.
= ![Alt text](images/Latihan-6C.1.png)


2. Gunakan renice untuk mengubah nice proses pertama menjadi +10. Proses mana yang kini lebih diprioritaskan scheduler?
=![Alt text](images/Latihan-6C.2.png)
Proses dengan nice +10 yang lebih diprioritaskan

3. Kirim SIGSTOP ke salah satu proses, verifikasi kondisi T-nya, lalu kirim SIGCONT. Akhiri semua proses percobaan dengan pkill sleep.
= ![Alt text](images/Latihan-6C.3.png)

