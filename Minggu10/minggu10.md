# JOBSHEET-10

## Tugas Praktikum
### Tugas 10.1 Audit Penggunaan Memori Sistem
Instruksi Umum: Kerjakan seluruh tugas pada direktori berikut.
mkdir -p ~/ praktikum - os / week10 - memory
cd ~/ praktikum - os / week10 - memory
<img width="971" height="619" alt="image" src="https://github.com/user-attachments/assets/8fa30217-3738-4dd1-8fd0-1763c5b5d72a" />
chmod + x ~/ praktikum - os / week10 - memory / memory - audit . sh
cd ~/ praktikum - os / week10 - memory
bash memory - audit . sh
<img width="974" height="770" alt="image" src="https://github.com/user-attachments/assets/0deb12fc-3e7e-4439-bb43-02d6f5e460c9" />

Analisis
1. Hitung persentase memori tersedia (available / total × 100%). Apakah
sistem dalam kondisi normal?
= Hasil presentase memori tersedia adalah 26.32% jika dibulatkan, kondisi ini adalah normal namun mulai mendekati batas penggunaan yang intensif

2. Mengapa buff/cache tidak dihitung sebagai memori yang terpakai dari sudut
pandang ketersediaan untuk aplikasi?
= Dalam sistem operasi Linux, buff/cache adalah memori yang digunakan oleh kernel untuk mempercepat akses data (seperti membaca file dari disk).
Alasannya adalah karena Sifatnya yang Reclaimable (Dapat Diambil Kembali), Optimalisasi RAM, Perspektif Aplikasi


3. Dari /proc/meminfo, apakah SwapTotal lebih besar dari 0? Berapa nilai SwapFree?
= SwapTotal tidak lebih besar dari 0 dan untuk nilai SwapFree adalah 0 kb


### Tugas 10.2 Identifikasi Proses dengan Memori Tertinggi
Instruksi: Simpan daftar 10 proses pengguna memori terbesar ke file.
ps aux -- sort = -% mem | head -n 10 > top - memory - process . txt
cat top - memory - process . txt
<img width="952" height="231" alt="image" src="https://github.com/user-attachments/assets/e3a1be5b-89de-4d1a-9d25-96481fda88e1" />



Analisis
1. Proses apa di urutan pertama? Catat nilai %MEM dan RSS.
= Nama Proses (COMMAND): ps aux
Nilai %MEM: 0.0
Nilai RSS: 3920 (dalam satuan KB)


2. Konversikan RSS ke MB (bagi 1024). Apakah wajar?
= Rumus: MB = RSS/1024, Perhitungan 3920/1024 = 3.82 MB
Sangat wajar. Perintah ps aux adalah utilitas sistem kecil yang hanya bertugas membaca informasi dari direktori /proc dan menampilkannya.
Penggunaan memori sebesar 3.82 MB tergolong sangat ringan untuk sistem modern yang memiliki total RAM sekitar 4GB.

3. Jumlahkan %MEM dari 5 proses teratas. Berapa persen RAM yang mereka
gunakan bersama?
= ps aux: 0.0%
-bash: 0.0%
head -: 0.0%



### Tugas 10.3 Membuat dan Memverifikasi Swap File
Instruksi: Buat swap file khusus tugas sebesar 256 MB dan verifikasi.
sudo fallocate -l 256 M / swapfile - tugas - week10
sudo chmod 600 / swapfile - tugas - week10
sudo mkswap / swapfile - tugas - week10
sudo swapon / swapfile - tugas - week10
<img width="713" height="97" alt="image" src="https://github.com/user-attachments/assets/48db4aba-3231-4f26-81c4-290d4d92e300" />


Analisis
1. Identifikasi kolom NAME, TYPE, SIZE, dan USED pada output swapon –show.
= /swap.img: Ini adalah swap utama sistem sebesar 2G. Terlihat sudah digunakan (USED) sebanyak 44M.
/swapfile-tugas-week10: Ini adalah file swap yang baru saja kamu buat untuk tugas sebesar 256M. Terlihat USED masih 0B, yang artinya swap baru ini belum terisi data karena swap utama masih sangat luas.
PRIO (Priority): Swap pertama punya prioritas -2 dan yang kedua -3. Linux akan mengisi swap dengan prioritas tertinggi (angka lebih besar) terlebih dahulu.

2. Apakah nilai total pada baris Swap di free -h bertambah 256 MB?
= Total: 2.2Gi.
Logikanya: 2G (dari swap.img) + 256M (dari swapfile-tugas) ~ 2.2Gi
Analisis: Ini membuktikan bahwa tugas kamu berhasil. Nilai total swap bertambah tepat sebesar 256 MB setelah kamu mengaktifkan swapfile-tugas-week10.

3. Mengapa permission 600 penting? Apa risiko jika diatur ke 644?
= Total Mem: 1.9Gi
Available: 914Mi
Analisis: Sistem kamu sangat stabil. Meskipun RAM terpakai sekitar $1\text{G}$, kamu punya "napas" tambahan dari dua file swap tersebut yang totalnya mencapai $2.2\text{Gi}$. Ini adalah konfigurasi yang sangat aman; jika RAM habis, sistem punya banyak ruang cadangan untuk memindahkan data.


### Tugas 10.4 Analisis System Call dengan strace
Instruksi: Analisis system call yang dipanggil perintah ls.
strace -c ls 2 > strace - summary . txt
strace ls / etc 2 > strace - ls - etc . txt
cat strace - summary . txt
<img width="987" height="789" alt="image" src="https://github.com/user-attachments/assets/07bcfdd1-7a20-402c-854a-9c1361b31007" />


Analisis
1. Sebutkan minimal 5 system call dari strace-summary.txt beserta fungsi singkatnya.
= execve : Digunakan untuk menjalankan/mengeksekusi program (dalam hal ini program yang kamu trace).
mmap: Memetakan file atau perangkat ke dalam memori RAM (sering digunakan untuk alokasi memori atau membaca library).
openat: Membuka file atau direktori untuk dibaca atau ditulis.
read: Membaca data dari file descriptor (seperti membaca isi file).
write: Menulis data ke file descriptor (seperti menampilkan teks ke layar terminal)

2. System call mana yang paling sering dipanggil? Mengapa?
= mmap, Karena Berdasarkan kolom calls, mmap dipanggil sebanyak 18 kali (paling tinggi dibanding yang lain). Hal ini terjadi karena saat sebuah program dijalankan, sistem perlu memetakan berbagai library sistem (.so files) dan mengalokasikan ruang memori yang cukup agar program bisa berjalan di RAM.

3. Apakah ada errors lebih dari 0? Apakah program tetap berjalan normal
meskipun ada kegagalan tersebut?
= Ya, terdapat total 4 errors dalam ringkasan tersebut
Ya, program tetap berjalan normal meskipun ada kegagalan


### Tugas 10.5 Studi Kasus Diagnosa Server Lambat
Skenario: Server terasa lambat. Buat script diagnosa yang menggabungkan semua
pemeriksaan dari bab ini menggunakan fungsi Bash.
nano ~/ praktikum - os / week10 - memory / diagnosa - server . sh
<img width="980" height="700" alt="image" src="https://github.com/user-attachments/assets/b62b30e5-0b27-4d24-8acc-d6048ab526c3" />
chmod + x monitor - memori . sh
bash monitor - memori . sh
<img width="981" height="649" alt="image" src="https://github.com/user-attachments/assets/451d33f6-752f-42f8-83ea-1af606a30c59" />


Analisis
1. Jelaskan peran masing-masing fungsi: cek_memori, cek_swap, cek_proses,
cek_paging, dan ringkasan. Mengapa diagnosa dipecah menjadi fungsi
terpisah?
= - cek_memori: Mengambil data penggunaan RAM fisik untuk menghitung persentase ketersediaan.
- cek_swap: Memeriksa apakah ada ruang swap yang digunakan.
- cek_proses: Mengidentifikasi proses yang paling banyak memakan memori menggunakan perintah ps.
- cek_paging: Memantau aktivitas keluar-masuk data antara RAM dan Disk (Swap In/Swap Out) menggunakan vmstat.
- ringkasan: Mengumpulkan hasil dari fungsi-fungsi di atas untuk memberikan status akhir sistem.
dipecah Agar kode lebih modular. Ini memudahkan proses debugging, membuat kode lebih rapi (tidak terjadi pengulangan), dan memungkinkan pengembang untuk memanggil diagnosis tertentu saja tanpa harus menjalankan seluruh script.

2. Berdasarkan bagian RINGKASAN, apakah kondisi sistem normal atau kritis?
Jelaskan berdasarkan nilai threshold yang digunakan script.
= Kondisi tersebut normal
Penjelasan: Di dalam script, biasanya digunakan threshold (ambang batas) sekitar 10% atau 15%. Output kamu menunjukkan 23% memori masih tersedia. Karena $23\% > 15\%$, script mencetak status "Sistem dalam kondisi normal".

3. Mengapa script menggunakan tee "$LAPORAN" bukan redirection biasa >
"$LAPORAN"? Apa keuntungannya?
= Perbedaan: Redirection biasa (>) hanya akan mengirimkan output ke dalam file (layar terminal akan kosong). Sedangkan tee akan membagi output ke dua arah.
Keuntungan: Kamu bisa melihat hasil secara langsung di terminal (real-time) sekaligus menyimpannya ke dalam file laporan secara otomatis. Ini sangat efisien untuk kegiatan monitoring.

4. Dari output cek_paging, apakah ada aktivitas si atau so? Jika ada, apa
implikasinya terhadap performa server?
= Nilai si (Swap-In): 0 dan Nilai so (Swap-Out): 0
Implikasi: Tidak ada aktivitas paging yang terjadi. Artinya, RAM fisik masih cukup untuk menangani semua data aplikasi yang aktif, sehingga sistem tidak perlu memindahkan data ke disk (swap).
Dampaknya ke Performa: Performa server sangat optimal dan cepat. Jika nilai si atau so tinggi, server akan terasa lambat (I/O Wait) karena disk bekerja keras menggantikan peran RAM yang habis.
