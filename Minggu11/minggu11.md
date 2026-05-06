# JOBSHEET - 11

## Latihan
### Latihan 9.A — Audit dan Kolaborasi
1. Temukan file SUID aktif dengan find / -perm -4000 -type f 2>/dev/null, lalu jelaskan
tiga file yang Anda kenali beserta alasannya.
<img width="657" height="578" alt="image" src="https://github.com/user-attachments/assets/48527d8b-9fb8-4eae-ad71-cda7f1ef0a21" />
/usr/bin/passwd : File ini digunakan oleh user untuk mengganti kata sandi mereka sendiri. Karena kata sandi yang terenkripsi disimpan di /etc/shadow (yang hanya bisa ditulis oleh root), file passwd memerlukan bit SUID agar bisa menjalankan proses penulisan ke file sensitif tersebut atas nama user biasa.
/usr/bin/sudo   : Ini adalah alat utama untuk eskalasi hak akses. Bit SUID pada sudo memungkinkan user yang terdaftar dalam grup sudo atau wheel untuk menjalankan perintah dengan hak akses superuser (root) setelah memverifikasi identitas mereka.
/usr/bin/mount  : Digunakan untuk memasang (mount) sistem berkas atau perangkat (seperti USB drive atau ISO) ke dalam hirarki direktori Linux. Operasi ini secara teknis memerlukan hak akses root untuk berinteraksi langsung dengan kernel dan perangkat keras, sehingga bit SUID disematkan agar user biasa bisa melakukan mount pada titik-titik tertentu yang diizinkan.


2. Cari direktori world-writable dan tentukan mana yang valid dan mana yang berisiko.
<img width="1169" height="366" alt="image" src="https://github.com/user-attachments/assets/19fda520-ff49-403f-bb6c-666107057b06" />
Direktori yang valid:
/tmp dan /var/tmp
/dev/shm
/run/lock

Direktori yang berisiko:
/home
Direktori dibawah /var/snap/microk8s/


3. Rancang konfigurasi permission standar dan ACL untuk direktori proyek /srv/webapp/ agar
group webapp-team dapat menulis, user deploy hanya membaca, dan file baru selalu mewarisi
group proyek.
<img width="812" height="368" alt="image" src="https://github.com/user-attachments/assets/a9af7aaa-2f35-432f-bb9c-cad17372351a" />
Karena perintah setfacl tidak tersedia di linux saya, jadi saya menggunakan Standard Permissions yang memenuhi kriteria
- Group webapp-team dapat menulis: Berhasil diatur dengan sudo chown root:webapp-team /srv/webapp dan sudo chmod 775 /srv/webapp/.
- User deploy hanya membaca: Berhasil karena user deploy (sebagai others) mendapatkan akses r-x.
- Pewarisan Group (Inheritance): Berhasil diatur dengan perintah terakhir Anda: sudo chmod g+s /srv/webapp/.



###  Latihan 9.B — Kebijakan Akun dan Quota
Tuliskan langkah untuk membuat user intern, menambahkannya ke group labgroup, memaksa pergantian password tiap 45 hari (warning 7 hari), memberi izin sudo hanya untuk systemctl status, dan
menetapkan quota ruang serta inode sederhana pada /home/.
<img width="626" height="47" alt="image" src="https://github.com/user-attachments/assets/19cfb497-21d2-4d11-a929-21ca00d1b12a" />

Izin Sudo Terbatas (systemctl status)
<img width="954" height="802" alt="image" src="https://github.com/user-attachments/assets/fc44d7e7-5eed-496d-922c-f29b8f8bb450" />

Menetapkan Quota Ruang & Inode
<img width="759" height="608" alt="image" src="https://github.com/user-attachments/assets/e0960409-b100-4ccc-a243-6674bf62513b" />
<img width="953" height="801" alt="image" src="https://github.com/user-attachments/assets/39f60919-baca-4a55-80e9-ad71545cec39" />
<img width="667" height="78" alt="image" src="https://github.com/user-attachments/assets/b699a7a4-0281-40b1-a999-822ed0ba5fc3" />


