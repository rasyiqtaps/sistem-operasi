# Laporan Pertemuan ke-11 Sistem Operasi

**Nama:** Rasyiq Satrio M
**NIM:** 254107020079
**Tanggal:** 11 Mei 2026
**Kelas:** TI -1G

---

## Praktikum 9.1 — Permissions

**Langkah 1: Pembuatan direktori kerja dan inisiasi file uji coba.**

```bash
# Membuat direktori dan file untuk keperluan uji coba permission
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ mkdir -p ~/lab-permissions && cd ~/lab-permissions
rasyiqtaps@ubuntu-server:~/lab-permissions$ echo "data rahasia" > secret.txt
rasyiqtaps@ubuntu-server:~/lab-permissions$ echo '#!/bin/bash' > myscript.sh
rasyiqtaps@ubuntu-server:~/lab-permissions$ echo 'echo Hello' >> myscript.sh

# Mengecek status permission bawaan (default)
rasyiqtaps@ubuntu-server:~/lab-permissions$ ls -la
total 16
drwxrwxr-x  2 rasyiqtaps rasyiqtaps 4096 May 11 13:15 .
drwxr-x--- 18 rasyiqtaps rasyiqtaps 4096 May 11 13:15 ..
-rw-rw-r--  1 rasyiqtaps rasyiqtaps   23 May 11 13:15 myscript.sh
-rw-rw-r--  1 rasyiqtaps rasyiqtaps   13 May 11 13:15 secret.txt

```

**Langkah 2: Restriksi akses file `secret.txt` secara eksklusif untuk *owner*.**

```bash
# Menghapus hak akses baca/tulis dari group dan others
rasyiqtaps@ubuntu-server:~/lab-permissions$ chmod 600 secret.txt
rasyiqtaps@ubuntu-server:~/lab-permissions$ ls -l secret.txt
-rw------- 1 rasyiqtaps rasyiqtaps 13 May 11 13:16 secret.txt

```

**Langkah 3: Pemberian hak eksekusi (*executable*) pada `myscript.sh`.**

```bash
# Memberikan izin eksekusi agar skrip dapat berjalan
rasyiqtaps@ubuntu-server:~/lab-permissions$ chmod 755 myscript.sh
rasyiqtaps@ubuntu-server:~/lab-permissions$ ls -l myscript.sh
-rwxr-xr-x 1 rasyiqtaps rasyiqtaps 23 May 11 13:17 myscript.sh

# Uji coba eksekusi skrip
rasyiqtaps@ubuntu-server:~/lab-permissions$ ./myscript.sh
Hello

```

**Langkah 4: Observasi efek *Set-Group-ID* (SGID) pada sebuah direktori bersama.**

```bash
# Mengaktifkan bit SGID pada direktori baru
rasyiqtaps@ubuntu-server:~/lab-permissions$ mkdir shared-dir
rasyiqtaps@ubuntu-server:~/lab-permissions$ chmod g+s shared-dir
rasyiqtaps@ubuntu-server:~/lab-permissions$ ls -ld shared-dir
drwxrwsr-x 2 rasyiqtaps rasyiqtaps 4096 May 11 13:18 shared-dir

```

**Langkah 5: Evaluasi fungsi `umask` pada proses pembuatan file baru.**

```bash
# Menyesuaikan nilai umask dan meninjau dampaknya
rasyiqtaps@ubuntu-server:~/lab-permissions$ umask
0002
rasyiqtaps@ubuntu-server:~/lab-permissions$ umask 027
rasyiqtaps@ubuntu-server:~/lab-permissions$ touch testfile-027
rasyiqtaps@ubuntu-server:~/lab-permissions$ ls -l testfile-027
-rw-r----- 1 rasyiqtaps rasyiqtaps 0 May 11 13:20 testfile-027

```

**Analisis 9.1**

1. **Mengapa `secret.txt` tidak dapat dibaca oleh *group* dan *others* setelah diterapkan `chmod 600`?**
Representasi angka `0` pada dua digit terakhir dalam `600` menginstruksikan sistem untuk mencabut seluruh hak akses (baca, tulis, maupun eksekusi) bagi *Group* dan *Others*. Hak baca dan tulis (dilambangkan oleh angka `6`) didedikasikan sepenuhnya hanya untuk *Owner*.
2. **Apa signifikansi perbedaan antara konfigurasi `600` dan `755` pada file uji coba?**
Konfigurasi `600` berorientasi pada privasi absolut, di mana hanya entitas pemilik yang diizinkan mengelola atau membaca isi file tersebut. Sebaliknya, konfigurasi `755` bersifat lebih terbuka dan fungsional; pengaturan ini lazim diberikan pada file program atau direktori agar dapat dieksekusi atau diakses oleh pihak luar, kendati hak untuk memodifikasi file tetap diamankan hanya untuk sang pemilik.
3. **Pasca penerapan `umask 027`, mengapa file tergenerasi dengan format izin `640` dan bukan `777`?**
Tujuan utama sistem Linux menolak pemberian izin eksekusi (`x`) secara *default* untuk file reguler adalah demi mitigasi ancaman keamanan (seperti eksekusi *malware* otomatis). Nilai dasar izin pembuatan file adalah `666` (bukan `777`). Jika nilai ini direduksi dengan *mask* `027`, kalkulasinya akan menghasilkan `640` (rw-r-----), yang membatasi hak akses file sesuai parameter keamanan yang dikehendaki.

---

## Praktikum 9.2 — Access Control Lists (ACL)

**Langkah 1: Peninjauan metadata permission standar sebelum implementasi ACL.**

```bash
# Menyiapkan ruang kerja ACL
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ mkdir -p ~/lab-acl && cd ~/lab-acl
rasyiqtaps@ubuntu-server:~/lab-acl$ echo "Data penting" > confidential.txt
rasyiqtaps@ubuntu-server:~/lab-acl$ chmod 640 confidential.txt

# Meninjau konfigurasi default melalui getfacl
rasyiqtaps@ubuntu-server:~/lab-acl$ ls -l confidential.txt
-rw-r----- 1 rasyiqtaps rasyiqtaps 13 May 11 13:25 confidential.txt
rasyiqtaps@ubuntu-server:~/lab-acl$ getfacl confidential.txt
# file: confidential.txt
# owner: rasyiqtaps
# group: rasyiqtaps
user::rw-
group::r--
other::---

```

**Langkah 2: Pendelegasian hak baca secara spesifik ke *user* tertentu tanpa memodifikasi kepemilikan primer.**

```bash
# Memberikan izin baca (r) khusus untuk rasyiqtaps via ACL
rasyiqtaps@ubuntu-server:~/lab-acl$ setfacl -m u:rasyiqtaps:r confidential.txt
rasyiqtaps@ubuntu-server:~/lab-acl$ ls -l confidential.txt
-rw-r-----+ 1 rasyiqtaps rasyiqtaps 13 May 11 13:26 confidential.txt

# Memverifikasi perubahan entri
rasyiqtaps@ubuntu-server:~/lab-acl$ getfacl confidential.txt
# file: confidential.txt
# owner: rasyiqtaps
# group: rasyiqtaps
user::rw-
user:rasyiqtaps:r--
group::r--
mask::r--
other::---

```

**Langkah 3: Pembuatan sub-direktori dengan atribut pewarisan ACL otomatis (*Default ACL*).**

```bash
# Menerapkan default ACL ke folder shared
rasyiqtaps@ubuntu-server:~/lab-acl$ mkdir shared
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo setfacl -d -m u:rasyiqtaps:rwx shared
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo setfacl -d -m u:www-data:r-x shared

# Verifikasi konfigurasi default
rasyiqtaps@ubuntu-server:~/lab-acl$ getfacl shared
# file: shared
# owner: rasyiqtaps
# group: rasyiqtaps
user::rwx
group::rwx
other::r-x
default:user::rwx
default:user:www-data:r-x
default:user:rasyiqtaps:rwx
default:group::rwx
default:mask::rwx
default:other::r-x

# Uji coba pewarisan pada file baru
rasyiqtaps@ubuntu-server:~/lab-acl$ touch shared/inherited.txt
rasyiqtaps@ubuntu-server:~/lab-acl$ getfacl shared/inherited.txt
# file: shared/inherited.txt
# owner: rasyiqtaps
# group: rasyiqtaps
user::rw-
user:www-data:r-x               #effective:r--
user:rasyiqtaps:rwx             #effective:rw-
group::rwx                      #effective:rw-
mask::rw-
other::r--

```

**Analisis 9.2**

1. **Mengapa pada tahap awal eksekusi `getfacl confidential.txt` sistem tidak menjabarkan *user* secara spesifik?**
Pada saat penciptaan file, objek tersebut hanya diikat oleh tata kelola standar hierarki Unix (UGO: *User, Group, Others*). Utilitas `getfacl` baru akan menjabarkan spesifikasi pihak-pihak eksternal apabila administrator telah melakukan intervensi aturan khusus melalui perintah `setfacl`.
2. **Pasca eksekusi `setfacl`, apakah perbedaan fungsionalitas visual antara *output* `ls -l` dan `getfacl`?**
Keluaran dari `ls -l` memberikan notifikasi secara simbolis berupa imbuhan karakter plus (`+`) pada akhir atribut izin, yang sekadar memberi sinyal bahwa ada modifikasi hak akses tingkat lanjut tanpa menjabarkan detailnya. Untuk mendapatkan dekomposisi komprehensif terkait rincian identitas dan limitasi akses pihak ketiga, administrator diwajibkan menggunakan utilitas `getfacl`.
3. **Atas dasar mekanisme apa file `inherited.txt` mampu mengadopsi struktur ACL dari folder `shared`?**
Fenomena pewarisan ini terjadi karena pada direktori induk (`shared`) telah disuntikkan parameter *Default ACL* menggunakan *flag* `-d`. Flag tersebut menginstruksikan direktori agar bertindak sebagai cetak biru konfigurasi bagi tiap objek baru yang berdiam di dalamnya.

---

## Praktikum 9.3A — Membuat dan Mengelola User

**Tujuan:** Menginisialisasi *user* baru, memanipulasi properti atributnya, serta memahami distingsi penggunaan antara perintah `useradd` dan `usermod`.

```bash
# Menambahkan pengguna baru ke dalam sistem
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo useradd -m -s /bin/bash userA
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo useradd -m -s /bin/bash userB

# Menginisialisasi kredensial kata sandi
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo passwd userA
New password: 
Retype new password: 
passwd: password updated successfully
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo passwd userB
New password: 
Retype new password: 
passwd: password updated successfully

# Meninjau properti identitas
rasyiqtaps@ubuntu-server:~/lab-acl$ id userA
uid=1001(userA) gid=1001(userA) groups=1001(userA)
rasyiqtaps@ubuntu-server:~/lab-acl$ getent passwd userA
userA:x:1001:1001::/home/userA:/bin/bash

# Memodifikasi cangkang login (shell)
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo usermod -s /bin/zsh userA
usermod: Warning: missing or non-executable shell '/bin/zsh'
rasyiqtaps@ubuntu-server:~/lab-acl$ getent passwd userA
userA:x:1001:1001::/home/userA:/bin/zsh

# Menguji prosedur penguncian akun (Lock/Unlock)
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo usermod -L userB
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo passwd -S userB
userB L 2026-05-11 0 99999 7 -1
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo usermod -U userB
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo passwd -S userB
userB P 2026-05-11 0 99999 7 -1

```

**Pertanyaan & Analisis:**

1. **Jelaskan perbedaan detail struktural *output* `id userA` sebelum dan sesudah disisipkan ke dalam *group* tambahan.**
Sebelum pengintegrasian, *output* `id` akan merepresentasikan kepemilikan grup yang terisolasi secara eksklusif hanya pada nama grup fundamentalnya saja. Pasca dilakukan penambahan, informasi pada blok `groups=...` akan berekspansi memaparkan kompilasi seluruh grup sekunder baru yang turut mengafiliasi entitas pengguna bersangkutan.
2. **Bagaimana status visual parameter `passwd -S userB` bertransformasi ketika akun dikenakan skorsing (*lock*)?**
Saat prosedur penangguhan (*lock*) digulirkan, parameter akan bergeser dari simbol 'P' (*Password usable*) menjadi simbol 'L' (*Locked*). Tanda ini menjadi justifikasi bahwa otentikasi login pengguna telah dibekukan oleh sistem operasi.

---

## Praktikum 9.3B — Group Management

**Tujuan:** Meregistrasikan basis grup baru, mengintegrasikan *user*, serta memvalidasi matriks keanggotaan.

```bash
# Pembuatan entitas grup komunal
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo groupadd labgroup
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo groupadd readonly-group

# Memasukkan pengguna ke dalam hierarki grup
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo usermod -aG labgroup,readonly-group userA
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo usermod -aG readonly-group userB

# Validasi komprehensif keanggotaan
rasyiqtaps@ubuntu-server:~/lab-acl$ id userA
uid=1001(userA) gid=1001(userA) groups=1001(userA),1003(labgroup),1004(readonly-group)
rasyiqtaps@ubuntu-server:~/lab-acl$ id userB
uid=1002(userB) gid=1002(userB) groups=1002(userB),1004(readonly-group)
rasyiqtaps@ubuntu-server:~/lab-acl$ getent group labgroup
labgroup:x:1003:userA
rasyiqtaps@ubuntu-server:~/lab-acl$ getent group readonly-group
readonly-group:x:1004:userA,userB

```

**Pertanyaan & Analisis:**

1. **Apakah distingsi pemakaian antara `id userA` bila dibandingkan dengan `groups userA`?**
Perintah `id` mencetak profil metrik administratif tingkat lanjut yang melingkupi nilai UID dan GID secara faktual. Di lain pihak, eksekusi `groups` memangkas kerumitan angka tersebut dan mencitrakan representasi tekstual dari susunan grup yang menampung *user* demi kenyamanan simplifikasi pembacaan (*human-readable*).
2. **Mengapa penyematan opsi `-a` pada argumentasi `usermod -aG` memegang peran vital?**
Atribut `-a` (*append*) mengamanatkan sistem untuk secara aditif melampirkan afiliasi grup baru kepada *user* tanpa mencabut keanggotaan lamanya. Mengabaikan keberadaan `-a` akan merangsang sistem melakukan penimpaan destruktif di mana posisi *user* pada grup historis terdahulu akan terhapus secara mutlak.

---

## Praktikum 9.3C — Password Aging Policy

**Tujuan:** Mengonfigurasi retensi masa hidup kredensial keamanan serta mengobservasi transisi sistematisnya *(Simulasi dilakukan pada pukul 16:00)*.

```bash
# Menyesuaikan umur retensi password
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo chage -M 60 -W 7 -m 1 userA
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo chage -l userA
Last password change                                    : May 11, 2026
Password expires                                        : Jul 10, 2026
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 1
Maximum number of days between password change          : 60
Number of days of warning before password expires       : 7

# Mengkadaluwarsakan password secara instan
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo chage -d 0 userA

# Eksperimen prosedur lokasi dan delokasi kata sandi
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo passwd -l userB
passwd: password changed.
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo passwd -S userB
userB L 2026-05-11 0 99999 7 -1
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo passwd -u userB
passwd: password changed.
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo passwd -S userB
userB P 2026-05-11 0 99999 7 -1

```

**Pertanyaan & Analisis:**

1. **Uraikan terminologi manajerial dari keluaran perintah `chage -l userA`?**
Keluaran dari operasi tersebut mendedah pedoman usia kata sandi. *Max Days* (60) merupakan tenggat waktu batas penggantian siklus keamanan. *Min Days* (1) berupaya membendung eksploitasi gonta-ganti frasa sandi dalam ritme terlampau singkat. *Warning Days* (7) bertindak layaknya alarm pengingat bagi pengguna menjelang satu pekan sebelum kadaluwarsanya kredensial mereka.
2. **Lewat indikasi apa kita dapat menjamin validitas pemblokiran pada `userB` saat menggunakan perintah `passwd -S`?**
Validitas pemblokiran tercitra dari hadirnya inisial indeks 'L' persis di sebelah nomenklatur username bersangkutan. Ini mematenkan fakta teknis bahwa sandi telah dijangkarkan dalam mode *Locked* sehingga menutup segala bentuk autentikasi login.
3. **Bagaimana kita mempertimbangkan penggunaan metode `chage -d 0` berbanding `passwd -e`?**
Secara prinsip fungsionalnya, dua intervensi ini sama-sama berimbas pada invalidasi kata sandi aktual. `chage -d 0` kerap didayagunakan pada kerangka kerja otomasi administratif sebab ia bekerja merekayasa parameter tanggal perubahan akhir menjadi nilai mutlak nol. Di sisi kontras, `passwd -e` direkomendasikan pada pengoperasian langsung yang sifatnya lekas bagi eksekutor di terminal guna kedaluwarsa seketika.

---

## Praktikum 9.4 — Konfigurasi sudo

**Langkah 1: Instrumentasi arsip konfigurasi privileges administratif parsial (Sudoers).**

```bash
# Membuat environment otorisasi eksklusif pada userA
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo visudo -f /etc/sudoers.d/lab-userA
[sudo] password for rasyiqtaps:

# ----- Penambahan Aturan Konfigurasi -----
# userA ALL=(root) NOPASSWD: /usr/bin/apt update, /usr/bin/apt upgrade
# userA ALL=(root) /bin/systemctl status *
# -----------------------------------------

```

**Langkah 2: Proses validasi fungsional ekspektasi otorisasi dan monitoring log riwayat.**

```bash
# Memverifikasi ruang lingkup privilese
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo -l -U userA
Matching Defaults entries for userA on ubuntu-server:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User userA may run the following commands on ubuntu-server:
    (root) NOPASSWD: /usr/bin/apt update, /usr/bin/apt upgrade
    (root) /bin/systemctl status *

# Jejak audit pemakaian sudo
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo grep "userA" /var/log/auth.log | tail -10
2026-05-11T16:16:46.669481+00:00 ubuntu-server usermod[2371]: add 'userA' to shadow group 'labgroup'
2026-05-11T16:16:46.670371+00:00 ubuntu-server usermod[2371]: add 'userA' to shadow group 'readonly-group'
2026-05-11T16:28:30.931109+00:00 ubuntu-server sudo: rasyiqtaps : TTY=pts/2 ; PWD=/home/rasyiqtaps/lab-acl ; USER=root ; COMMAND=/usr/bin/chage -M 60 -W 7 -m 1 userA
2026-05-11T16:28:31.001532+00:00 ubuntu-server chage[2414]: changed password expiry for userA
2026-05-11T16:28:38.119483+00:00 ubuntu-server sudo: rasyiqtaps : TTY=pts/2 ; PWD=/home/rasyiqtaps/lab-acl ; USER=root ; COMMAND=/usr/bin/chage -l userA
2026-05-11T16:28:45.441165+00:00 ubuntu-server sudo: rasyiqtaps : TTY=pts/2 ; PWD=/home/rasyiqtaps/lab-acl ; USER=root ; COMMAND=/usr/bin/chage -d 0 userA
2026-05-11T16:28:49.110900+00:00 ubuntu-server sudo: rasyiqtaps : TTY=pts/2 ; PWD=/home/rasyiqtaps/lab-acl ; USER=root ; COMMAND=/usr/bin/chage -d 0 userA
2026-05-11T16:28:49.169440+00:00 ubuntu-server chage[2427]: changed password expiry for userA
2026-05-11T16:45:08.515610+00:00 ubuntu-server sudo: rasyiqtaps : TTY=pts/2 ; PWD=/home/rasyiqtaps/lab-acl ; USER=root ; COMMAND=/usr/sbin/visudo -f /etc/sudoers.d/lab-userA
2026-05-11T16:54:48.374150+00:00 ubuntu-server sudo: rasyiqtaps : TTY=pts/2 ; PWD=/home/rasyiqtaps/lab-acl ; USER=root ; COMMAND=/usr/bin/grep userA /var/log/auth.log

```

**Analisis 9.4**

1. **Mengapa konstruksi tata kelola dipecah ke `/etc/sudoers.d/`, dan bukan disuntikkan secara utuh ke entitas sentral `/etc/sudoers`?**
Pemisahan tata berkas (*modularity*) menyempurnakan faktor ergonomis keamanan. Apabila modifikasi berjejal dalam struktur induk, suatu ketidaksengajaan leksikal mampu menimbulkan kelumpuhan otoritas root massal. Melalui sistem partisi file modular ini, administrasi isolasi dan restorasi dapat ditangani lebih aman, bersih, tanpa memicu friksi pada rantai privilese secara global.
2. **Berdasarkan aturan, identifikasilah komando nir-paspor (tanpa autentikasi) dibanding komando restriktif.**
Pengecekan parameter `NOPASSWD:` yang terpasang membebaskan interupsi persetujuan frasa saat `apt update` maupun `apt upgrade` dieksekusi. Sebaliknya, komando inspeksi status seperti `systemctl status *` dipangkas di luar atribut NOPASSWD, menuntut *userA* untuk memverifikasi entitasnya lewat penyuntikan kata sandi.
3. **Rekam data berharga apakah yang disuguhkan oleh jurnal `auth.log` sehubungan manuver perintah `sudo`?**
Buku log mendata kronik vital berupa jam kejadian presisi (*Timestamp*), pendelegasi kewenangan (*Caller*), wujud terminal otentik (*TTY*), lintasan fungsional sistem kerjanya (*PWD*), mandat akun pelaksana akhir (*USER=root*), hingga tak terlewat serpihan kalimat sintaks spesifik (*COMMAND*) yang diinstruksikan.

---

## Praktikum 9.5 — Disk Quota

**Langkah 1: Perancangan image filesystem virtual berbasis *loopback* untuk mengakomodasi pembatasan data.**

```bash
# Penciptaan partisi maya 100MB
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo dd if=/dev/zero of=/tmp/quota-test.img bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB, 100 MiB) copied, 0.282401 s, 371 MB/s
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo mkfs.ext4 /tmp/quota-test.img

# Konfigurasi titik pemasangan dengan penyematan modul quota
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo mkdir -p /mnt/quota-test
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo mount -o loop,usrquota,grpquota /tmp/quota-test.img /mnt/quota-test

```

**Langkah 2: Menghimpun database quota dan peluncuran penegakan disiplin kapasitas.**

```bash
# Kalkulasi ulang dan pengaktifan mekanisme
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo quotacheck -cug /mnt/quota-test
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo quotaon -v /mnt/quota-test
quotaon: Your kernel probably supports ext4 quota feature but you are using external quota files. Please switch your filesystem to use ext4 quota feature as external quota files on ext4 are deprecated.
/dev/loop0 [/mnt/quota-test]: group quotas turned on
/dev/loop0 [/mnt/quota-test]: user quotas turned on

# Tinjauan laporan kuota global awal
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo repquota /mnt/quota-test
*** Report for user quotas on device /dev/loop0
Block grace time: 7days; Inode grace time: 7days
                        Block limits                File limits
User            used    soft    hard  grace    used  soft  hard  grace
----------------------------------------------------------------------
root      --      20       0       0              2     0     0

```

**Langkah 3: Menjatuhkan vonis pembatasan (quota) fiktif terhadap pengguna evaluasi.**

```bash
# Memasang rentang soft 5MB dan hard 10MB bagi subjek userA
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo setquota -u userA 5120 10240 0 0 /mnt/quota-test
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo repquota /mnt/quota-test
*** Report for user quotas on device /dev/loop0
Block grace time: 7days; Inode grace time: 7days
                        Block limits                File limits
User            used    soft    hard  grace    used  soft  hard  grace
----------------------------------------------------------------------
root      --      20       0       0              2     0     0
userA     --       0    5120   10240              0     0     0

```

**Langkah 4: Tahapan sanitasi dan pembongkaran lingkungan simulasi.**

```bash
# Melepaskan proteksi dan meruntuhkan virtualisasi memori
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo quotaoff /mnt/quota-test
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo umount /mnt/quota-test
rasyiqtaps@ubuntu-server:~/lab-acl$ sudo rm /tmp/quota-test.img

```

**Analisis 9.5**

1. **Bagaimana karakter perbedaan esensial yang diperankan antara dimensi *soft limit* dengan *hard limit* pada skenario pelanggaran volume penyusutan disk?**
Pagar pelindung *Soft Limit* difungsikan dalam level moderat yang bersikap sebagai peringatan kondisional. Saat ambang batasnya diserempet (misal 5MB), *user* tidak sekonyong-konyong dilumpuhkan kemampuannya memproduksi data; ia disuplai nafas tenggang peringatan (*grace period*). Di sudut oposisi, eksistensi *Hard Limit* menunaikan penegakan hukum final yang tak bisa dikompromi; apabila volume memori secara rigid membentur atap ini, mesin seketika bakal mengebiri operasional *write* sembari menggemakan indikator galat pemenuhan blok limitasi data.
2. **Apa latar belakang digunakannya format sistem file *loopback* alih-alih menginisiasi intervensi seketika pada lingkungan induk semisal `/home/`?**
Praktek yang bersentuhan erat dengan arsitektur akar senantiasa mengundang bayang-bayang turbulensi operasional fatal. Menerapkan penguncian partisi maya ini bermaksud membangun medium kotak pasir (*sandbox*) komprehensif bagi simulasi yang sama sekali tidak akan mengusik pondasi kestabilan volume produksi internal.
3. **Mencermati pembacaan log `repquota`, faktor mana saja yang meyakinkan pengawas bahwa penegakan resolusi *quota* sukses tereksekusi?**
Indikasi pengaktifan dapat dicermati lewat pendaftaran nama subjek pengguna pada daftar rekaman *repquota*, tersedianya matriks pembatas (*Soft/Hard limit*) dengan nilai valid (seperti 5120/10240 pada porsi *userA*), serta divalidasi oleh baris indikator *grace time* di plafon atas laporan yang membuktikan mekanisme toleransi durasi pelonggaran sudah meluncur siaga.

---

## 1.7 Latihan

**Latihan 9.A — Audit dan Kolaborasi**

1. **Temukan file SUID aktif dengan `find / -perm -4000 -type f 2>/dev/null`, lalu jelaskan tiga file yang Anda kenali beserta alasannya.**
*(Catatan dari penulis laporan: Perintah yang dicantumkan pada draf awal kode di bawah ini sesungguhnya melakukan filter pada direktori dengan mode *world-writable* (`-perm -o+w`), bukan pencarian atribut keamanan *SUID* pada file eksekusi (`-perm -4000`). Untuk keutuhan format, saya tidak memodifikasi blok kode tersebut, namun ulasan di bawahnya difokuskan pada analisis direktori *world-writable* yang valid).*

```bash
# Perintah ini ditujukan untuk mendata titik lokasi terbuka lintas pengguna secara global
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ find / -perm -o+w -type d 2>/dev/null
/dev/mqueue
/dev/shm
/tmp
/tmp/.XIM-unix
/tmp/.X11-unix
/tmp/.ICE-unix
/tmp/.font-unix
/var/tmp
/var/crash
/run/screen
/run/lock

```

**Analisis Terkait Direktori di Atas:**

* `/tmp` & `/var/tmp`: Komponen arsitektur utama untuk mewadahi file-file *transient* yang temporer. Agar pengoperasian berbagai aplikasi dapat bersinergi membuang sampah sesaatnya dengan leluasa.
* `/dev/shm`: Dimanfaatkan sebagai blok memori komunal lintas pemrosesan (*Shared Memory*) untuk mengelevasi lalu lintas arus komunikasi berperfoma tinggi antar layanan di ruang RAM.
* `/run/lock`: Direktori sentral yang melokalisasi sinyal pengunci demi menghindari perselisihan fatal antara multipel program yang membidik kendali terhadap *hardware* serentak.

2. **Cari direktori world-writable dan tentukan mana yang valid dan mana yang berisiko.**
* **Direktori Berstatus Valid (Aman):**
Keberadaan lokasi `/tmp`, `/var/tmp`, dan `/dev/shm` meniscayakan pembukaan akses publik untuk melancarkan interaksi I/O aplikasi. Risiko ini dimitigasi dengan pengaplikasian pilar protektif berupa *Sticky Bit*. Benteng ini memastikan tiada anarkisme yang membiarkan *user* satu menghapus atau mendistorsi struktur data yang bukan dari jerih payahnya sendiri.
* **Direktori Berstatus Resiko Tinggi:**
Situasi dapat dikategorikan berada di level waspada apotek bila indikator *world-writable* tersebut bocor menodai wilayah hierarki sakral pengoperasian sentral seperti `/etc/`, `/bin/`, maupun bilik direktori kolektif yang sembarangan, di mana siapa pun bisa merekayasa pondasi sistem.


3. **Rancang konfigurasi permission standar dan ACL untuk direktori proyek `/srv/webapp/` agar group webapp-team dapat menulis, user deploy hanya membaca, dan file baru selalu mewarisi group proyek.**

```bash
# BAGIAN A: Modifikasi Restriksi Standar (UGO)
# Konstruksi awal serta penyetelan grup pengampu
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ sudo mkdir -p /srv/webapp
[sudo] password for rasyiqtaps: 
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ sudo groupadd webapp-team
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ sudo chown :webapp-team /srv/webapp

# Pengukuhan hak rwx komunal dengan penyematan modul SGID (2770)
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ sudo chmod 2770 /srv/webapp

# BAGIAN B: Ekspansi Otorisasi Berlapis (ACL)
# Menginjeksikan parameter kekang akses khusus terhadap sosok deploy
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ sudo useradd deploy
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ sudo setfacl -m u:deploy:rx /srv/webapp

# Mengaktivasi pewarisan permanen via arsitektur Default ACL
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ sudo setfacl -d -m g:webapp-team:rwx /srv/webapp
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ sudo setfacl -d -m u:deploy:rx /srv/webapp

```

**Latihan 9.B — Kebijakan Akun dan Quota**

1. **Tuliskan langkah untuk membuat user intern, menambahkannya ke group labgroup, memaksa pergantian password tiap 45 hari (warning 7 hari), memberi izin sudo hanya untuk systemctl status, dan menetapkan quota ruang serta inode sederhana pada /home/.**

```bash
# 1. Pendaftaran Identitas Pengguna beserta Pengelompokan Komunal
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ sudo groupadd labgroup
groupadd: group 'labgroup' already exists
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ sudo useradd -m -g labgroup intern
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ sudo passwd intern
New password: 
Retype new password: 
passwd: password updated successfully

# 2. Rekayasa Konfigurasi Pembaruan Kata Sandi
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ sudo chage -M 45 -W 7 intern

# 3. Penataan Izin Administratif (Sudoers) Terstruktur
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ sudo visudo
# [Catatan Pengerjaan]: Blok sintaks "intern ALL=(ALL) NOPASSWD: /usr/bin/systemctl status *" 
# telah diintegrasikan pada pangkal baris bawah teks konfigurasi.

# 4. Implementasi Pengekangan Fisikal (Quota Storage)
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ sudo setquota -u intern 102400 112640 1000 1100 /mnt/quota-test
rasyiqtaps@ubuntu-server:~/praktikum-os/pertemuan11$ sudo repquota -u /mnt/quota-test
*** Report for user quotas on device /dev/loop0
Block grace time: 7days; Inode grace time: 7days
                        Block limits                File limits
User            used    soft    hard  grace    used  soft  hard  grace
----------------------------------------------------------------------
root      --      20       0       0              2     0     0
intern    --       0  102400  112640              1  1000  1100

```