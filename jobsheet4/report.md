# Laporan Pertemuan ke-3 Sistem Operasi

**Tanggal:** 9 maret 2026  
**Disusun Oleh:** Rasyiq Satrio m  
**NIM:** 254107020079  
**Kelas/No:** TI-1G/25  

---

## Tugas Pendahuluan

1. **Apa yang dimaksud perintah-perintah direktory : pwd, cd, mkdir, rmdir.**
    * **>> pwd (Print Working Directory):** Menampilkan lokasi folder tempat kamu berada sekarang.
    * **>> cd (Change Directory):** Pindah atau masuk ke folder lain.
    * **>> mkdir (Make Directory):** Membuat folder baru.
    * **>> rmdir (Remove Directory):** Menghapus folder (hanya jika foldernya kosong).

2. **Apa yang dimaksud perintah-perintah manipulasi file : cp, mv dan rm (sertakan format yang digunakan)**
    * **>> cp (Copy):** Digunakan untuk menyalin file atau folder dari satu tempat ke tempat lain tanpa menghapus aslinya.
      * Format: `cp [asal] [tujuan]`
    * **>> mv (Move):** Digunakan untuk memindahkan file ke lokasi baru atau mengubah nama file (rename).
      * Format: `mv [asal] [tujuan]`
    * **>> rm (Remove):** Digunakan untuk menghapus file secara permanen.
      * Format: `rm [nama_file]`

3. **Jelaskan perbedaan Symbolic link menggunakan hard link (direct) dan soft link (indirect).**
    * **>> Hard Link (Direct):** Duplikat nama yang menunjuk langsung ke data yang sama. Jika file asli dihapus, data tetap ada dan bisa diakses lewat link ini.
      * (Format: `ln [sumber] [link]`)
    * **>> soft Link (Indirect):** Penunjuk alamat (shortcut) ke nama file asli. Jika file asli dihapus atau dipindah, link ini akan rusak.
      * (Format: `ln -s [sumber] [link]`)

4. **Tuliskan maksud perintah-perintah : file, find, which, locate dan grep.**
    * **>> file:** Mengidentifikasi tipe format suatu file (apakah teks, gambar, eksekusi, dll).
    * **>> find:** Mencari file/direktori secara mendalam berdasarkan kriteria spesifik (nama, ukuran, waktu modifikasi).
    * **>> which:** Menampilkan lokasi path dari file eksekusi (program) yang sedang dijalankan.
    * **>> locate:** Mencari lokasi file dengan sangat cepat menggunakan database sistem (indeks).
    * **>> grep:** Mencari pola atau kata tertentu di dalam isi sebuah file atau output teks.

---

## Latihan 

### 1. Cobalah urutan perintah berikut :

* **$ cd** >> null
* **$ pwd** >> home/rasyiqtaps
* **$ ls –al** >> 
  ```text
  total 116
  drwxr-x--- 7 rasyiqtaps rasyiqtaps  4096 Mar  3 09:51 .
  drwxr-xr-x 3 root       root        4096 Feb 23 07:39 ..
  -rw-rw-r-- 1 rasyiqtaps rasyiqtaps   168 Mar  3 09:51 backup-error.log
  -rw-rw-r-- 1 rasyiqtaps rasyiqtaps 10240 Mar  3 09:51 backup_hari_ini.tar
  -rw-rw-r-- 1 rasyiqtaps rasyiqtaps     0 Mar  3 09:51 backup-success.log
  -rw------- 1 rasyiqtaps rasyiqtaps  3090 Mar  1 13:10 .bash_history
  -rw-r--r-- 1 rasyiqtaps rasyiqtaps   220 Mar 31  2024 .bash_logout
  -rw-r--r-- 1 rasyiqtaps rasyiqtaps  3771 Mar 31  2024 .bashrc
  drwx------ 2 rasyiqtaps rasyiqtaps  4096 Feb 23 07:39 .cache
  drwxrwxr-x 5 rasyiqtaps rasyiqtaps  4096 Feb 24 14:30 .config
  -rw-rw-r-- 1 rasyiqtaps rasyiqtaps    38 Feb 24 14:24 config.txt
  -rw-rw-r-- 1 rasyiqtaps rasyiqtaps 16590 Mar  3 09:46 daftar_config.txt
  -rw-rw-r-- 1 rasyiqtaps rasyiqtaps    69 Feb 24 14:00 data.log
  -rw-rw-r-- 1 rasyiqtaps rasyiqtaps     0 Mar  3 09:23 error.log
  -rw-rw-r-- 1 rasyiqtaps rasyiqtaps   681 Mar  3 09:23 large-logs.txt
  -rw-rw-r-- 1 rasyiqtaps rasyiqtaps   105 Feb 24 15:28 latihan
  -rw------- 1 rasyiqtaps rasyiqtaps    20 Feb 24 15:39 .lesshst
  drwxrwxr-x 3 rasyiqtaps rasyiqtaps  4096 Mar  3 09:34 .local
  -rwxrwxr-x 1 rasyiqtaps rasyiqtaps   505 Mar  3 09:39 monitoring.sh
  -rw-rw-r-- 1 rasyiqtaps rasyiqtaps   980 Mar  3 09:40 monitor.log
  drwxrwxr-x 3 rasyiqtaps rasyiqtaps  4096 Feb 24 13:48 praktikum-os
  -rw-r--r-- 1 rasyiqtaps rasyiqtaps   807 Mar 31  2024 .profile
  -rw-rw-r-- 1 rasyiqtaps rasyiqtaps   210 Feb 24 15:24 server.log
  -rw-rw-r-- 1 rasyiqtaps rasyiqtaps   253 Mar  3 09:28 sorted-users.txt
  drwx------ 2 rasyiqtaps rasyiqtaps  4096 Feb 23 07:39 .ssh
  -rw-r--r-- 1 rasyiqtaps rasyiqtaps     0 Feb 23 07:40 .sudo_as_admin_successful
    ```
* **$ cd .** >> null
* **$ pwd** >> /home/rasyiqtaps
* **$ cd ..** >> rasyiqtaps@ubuntu-server:/home
* **$ pwd** >> /home
* **$ ls -al** >> 
    ```total 12 
    drwxr-xr-x  3 root       root       4096 Feb 23 07:39 .
    drwxr-xr-x 23 root       root       4096 Feb 23 07:37 ..
    drwxr-x---  7 rasyiqtaps rasyiqtaps 4096 Mar  3 09:51 rasyiqtaps
    ```
* **$ cd ..** >> "/home" nya hilang
* **$ pwd** >> /
* **$ ls -al** >> 
    ```total 88
    drwxr-xr-x  23 root root  4096 Feb 23 07:37 .
    drwxr-xr-x  23 root root  4096 Feb 23 07:37 ..
    lrwxrwxrwx   1 root root     7 Apr 22  2024 bin -> usr/bin
    drwxr-xr-x   2 root root  4096 Feb 26  2024 bin.usr-is-merged
    drwxr-xr-x   3 root root  4096 Feb 24 14:51 boot
    dr-xr-xr-x   2 root root  4096 Feb 23 07:36 cdrom
    drwxr-xr-x  19 root root  4040 Mar 10 11:34 dev
    drwxr-xr-x 112 root root  4096 Mar  4 04:44 etc
    drwxr-xr-x   3 root root  4096 Feb 23 07:39 home
    lrwxrwxrwx   1 root root     7 Apr 22  2024 lib -> usr/lib
    lrwxrwxrwx   1 root root     9 Apr 22  2024 lib64 -> usr/lib64
    drwxr-xr-x   2 root root  4096 Feb 26  2024 lib.usr-is-merged
    drwx------   2 root root 16384 Feb 23 07:37 lost+found
    drwxr-xr-x   2 root root  4096 Feb 10 00:16 media
    drwxr-xr-x   3 root root  4096 Mar  1 12:32 mnt
    drwxr-xr-x   2 root root  4096 Feb 10 00:16 opt
    dr-xr-xr-x 199 root root     0 Mar 10 11:34 proc
    drwx------   3 root root  4096 Feb 23 07:39 root
    drwxr-xr-x  28 root root   860 Mar 10 11:54 run
    lrwxrwxrwx   1 root root     8 Apr 22  2024 sbin -> usr/sbin
    drwxr-xr-x   2 root root  4096 Oct  3 21:26 sbin.usr-is-merged
    drwxr-xr-x   2 root root  4096 Feb 23 07:39 snap
    drwxr-xr-x   2 root root  4096 Feb 10 00:16 srv
    dr-xr-xr-x  13 root root     0 Mar 10 11:34 sys
    drwxrwxrwt  13 root root  4096 Mar 10 11:59 tmp
    drwxr-xr-x  12 root root  4096 Feb 10 00:16 usr
    drwxr-xr-x  13 root root  4096 Feb 23 07:39 var
    ```

* **$ cd /etc** >> /etc
* **$ ls –al | more** >> 
    ```total 952
    drwxr-xr-x 112 root root       4096 Mar  4 04:44 .
    drwxr-xr-x  23 root root       4096 Feb 23 07:37 ..
    -rw-r--r--   1 root root       3444 Jul  5  2023 adduser.conf
    drwxr-xr-x   2 root root       4096 Feb 23 07:45 alternatives
    drwxr-xr-x   2 root root       4096 Feb 10 00:26 apparmor
    drwxr-xr-x   9 root root       4096 Feb 10 00:34 apparmor.d
    drwxr-xr-x   3 root root       4096 Feb 10 00:26 apport
    drwxr-xr-x   9 root root       4096 Feb 23 07:37 apt
    -rw-r--r--   1 root root       2319 Mar 31  2024 bash.bashrc
    -rw-r--r--   1 root root         45 Feb 10 00:34 bash_completion
    drwxr-xr-x   2 root root       4096 Feb 10 00:34 bash_completion.d
    -rw-r--r--   1 root root        367 Aug  2  2022 bindresvport.blacklist
    drwxr-xr-x   2 root root       4096 Nov 25 18:16 binfmt.d
    drwxr-xr-x   2 root root       4096 Feb 10 00:34 byobu
    drwxr-xr-x   3 root root       4096 Feb 10 00:26 ca-certificates
    -rw-r--r--   1 root root       6288 Feb 10 00:26 ca-certificates.conf
    drwxr-xr-x   5 root root       4096 Feb 23 07:39 cloud
    drwxr-xr-x   2 root root       4096 Feb 23 07:37 console-setup
    drwx------   2 root root       4096 Nov 25 18:16 credstore
    drwx------   2 root root       4096 Nov 25 18:16 credstore.encrypted
    drwxr-xr-x   2 root root       4096 Feb 10 00:34 cron.d
    drwxr-xr-x   2 root root       4096 Feb 10 00:34 cron.daily
    drwxr-xr-x   2 root root       4096 Feb 10 00:34 cron.hourly
    drwxr-xr-x   2 root root       4096 Feb 10 00:34 cron.monthly
    -rw-r--r--   1 root root       1136 Feb 10 00:34 crontab
    drwxr-xr-x   2 root root       4096 Feb 10 00:34 cron.weekly
    drwxr-xr-x   2 root root       4096 Feb 10 00:34 cron.yearly
    drwxr-xr-x   2 root root       4096 Feb 10 00:26 cryptsetup-initramfs
    -rw-r--r--   1 root root         54 Feb 10 00:26 crypttab
    drwxr-xr-x   4 root root       4096 Feb 10 00:26 dbus-1
    -rw-r--r--   1 root root       2967 Apr 12  2024 debconf.conf
    -rw-r--r--   1 root root         11 Apr 22  2024 debian_version
    drwxr-xr-x   3 root root       4096 Feb 24 15:38 default
    -rw-r--r--   1 root root       1706 Jul  5  2023 deluser.conf
    drwxr-xr-x   2 root root       4096 Feb 10 00:26 depmod.d
    drwxr-xr-x   3 root root       4096 Feb 10 00:26 dhcp
    -rw-r--r--   1 root root       1429 Nov 13 17:47 dhcpcd.conf
    drwxr-xr-x   4 root root       4096 Feb 10 00:25 dpkg
    -rw-r--r--   1 root root        685 Apr  8  2024 e2scrub.conf
    -rw-r--r--   1 root root        106 Feb 10 00:17 environment
    -rw-r--r--   1 root root       1853 Oct 17  2022 ethertypes
    drwxr-xr-x   4 root root       4096 Feb 23 07:38 fonts
    -rw-r--r--   1 root root        446 Feb 23 07:38 fstab
    -rw-r--r--   1 root root        694 Apr  8  2024 fuse.conf
    drwxr-xr-x   4 root root       4096 Feb 10 00:34 fwupd
    -rw-r--r--   1 root root       2584 Jan 31  2024 gai.conf
    drwxr-xr-x   4 root root       4096 Feb 23 07:45 ghostscript
    drwxr-xr-x   2 root root       4096 Feb 23 07:38 gnutls
    drwxr-xr-x   2 root root       4096 Feb 10 00:26 groff
    -rw-r--r--   1 root root        785 Feb 23 07:39 group
    -rw-r--r--   1 root root        775 Feb 23 07:39 group-
    --More--
    ```
* **$ cat passwd** >> 
    ```root:x:0:0:root:/root:/bin/bash
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    sys:x:3:3:sys:/dev:/usr/sbin/nologin
    sync:x:4:65534:sync:/bin:/bin/sync
    games:x:5:60:games:/usr/games:/usr/sbin/nologin
    man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
    lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
    mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
    news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
    uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
    proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
    www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
    backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
    list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
    irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
    _apt:x:42:65534::/nonexistent:/usr/sbin/nologin
    nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
    systemd-network:x:998:998:systemd Network Management:/:/usr/sbin/nologin
    systemd-timesync:x:997:997:systemd Time Synchronization:/:/usr/sbin/nologin
    dhcpcd:x:100:65534:DHCP Client Daemon,,,:/usr/lib/dhcpcd:/bin/false
    messagebus:x:101:102::/nonexistent:/usr/sbin/nologin
    systemd-resolve:x:992:992:systemd Resolver:/:/usr/sbin/nologin
    pollinate:x:102:1::/var/cache/pollinate:/bin/false
    polkitd:x:991:991:User for polkitd:/:/usr/sbin/nologin
    syslog:x:103:104::/nonexistent:/usr/sbin/nologin
    uuidd:x:104:105::/run/uuidd:/usr/sbin/nologin
    tcpdump:x:105:107::/nonexistent:/usr/sbin/nologin
    tss:x:106:108:TPM software stack,,,:/var/lib/tpm:/bin/false
    landscape:x:107:109::/var/lib/landscape:/usr/sbin/nologin
    fwupd-refresh:x:989:989:Firmware update daemon:/var/lib/fwupd:/usr/sbin/nologin
    usbmux:x:108:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
    rasyiqtaps:x:1000:1000:rasyiqtaps:/home/rasyiqtaps:/bin/bash
    sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
    ```
* **$ cd –** >> /
* **$ pwd** >> /

### 2. Lanjutkan penelusuran pohon pada sistem file menggunakan cd, ls, pwd dan cat. Telusuri direktory /bin, /usr/bin, /sbin, /tmp dan /boot.
**$ cd /bin** >> /bin

**$ ls** >> 
```pbmtoicon
411toppm                             pbmtolj
aa-enabled                           pbmtoln03
aa-exec                              pbmtolps
aa-features-abi                      pbmtomacp
acpidbg                              pbmtomatrixorbital
add-apt-repository                   pbmtomda
etc-- 
```
* **Fungsi: Direktori ini berisi utilitas sistem level rendah (binary)**

**$ cd /usr/bin** >> cd: too many argument

**$ ls** >> kurang labih sama seperti diatas
* **Fungsi: Berisi utilitas sistem dan program aplikasi level tinggi**

**cd /sbin** >> /sbin

**$ ls** >> kurang lebih sama seperti diatas
* **Fungsi: Berisi utilitas sistem untuk superuser (administrasi sistem).**

**$ cd /tmp** >> /tmp

**$ ls** >>
```snap-private-tmp
systemd-private-2ee7862c33cf46f18476cf9e03842f74-ModemManager
service-s4tPYm
systemd-private-2ee7862c33cf46f18476cf9e03842f74-polkit.service-sczhBW
systemd-private-2ee7862c33cf46f18476cf9e03842f74-systemd-logind
service-GIBgFW
systemd-private-2ee7862c33cf46f18476cf9e03842f74-systemd-resolved
service-dQsQ1m
systemd-private-2ee7862c33cf46f18476cf9e03842f74-systemd-timesyncd
service-16XWkp
systemd-private-2ee7862c33cf46f18476cf9e03842f74-upower.service-sYhRM9
```
* **Fungsi: Berisi file sementara yang akan dihapus saat sistem reboot.**

**$ cd /boot** >> /boot

**$ ls** >>
```config-6.8.0-100-generic  initrd.img-6.8.0-100-generic  System.map-6.8.0-101-generic  vmlinuz.old
config-6.8.0-101-generic  initrd.img-6.8.0-101-generic  vmlinuz
grub                      initrd.img.old                vmlinuz-6.8.0-100-generic
initrd.img                System.map-6.8.0-100-generic  vmlinuz-6.8.0-101-generic
```
* **Fungsi: Berisi file penting untuk proses bootstrap dan Kernel Linux (vmlinuz)**

### 3. Telusuri direktory /dev. Identifikasi perangkat yang tersedia. Identifikasi tty (termninal) Anda (ketik who am i); siapa pemilih tty Anda (gunakan ls –l). 
**$ cd /dev** >> /dev

**$ ls** >> ```code```

* **semua peralatan hardware (disk, mouse, printer) direpresentasikan sebagai file di dalam direktori ini**

**ketik :** who am i >>
```rasyiqtaps pts/0        2026-03-10 11:36 (10.0.2.2)```

**$ ls -l /dev/pts/0** >>
```crw--w---- 1 rasyiqtaps tty 136, 0 Mar 10 13:09 /dev/pts/0```

* **Perintah ini akan menunjukkan siapa pemilik (user) dan grup dari terminal tersebut.**

### 4. Telusuri derectory /proc. Tampilkan isi file interrupts, devices, cpuinfo, meminfo dan uptime menggunakan perintah cat. Dapatkah Anda melihat mengapa directory /proc disebut pseudo -filesystem yang memungkinkan akses ke struktur data kernel ? 

**Untuk Masuk ke direktori proc**
Ketik: cd /proc 
Ketik: ls

**akan ada banyak folder dengan angka PID (Process ID) atau nomor identitas proses yang sedang berjalan di sistem.**

**Melihat informasi Gangguan (Interrupts):**
Ketik: cat interrupts 

**Melihat daftar Perangkat (Devices):**
Ketik: cat devices 

**Menampilkan driver perangkat yang sedang aktif di sistem.**

**Melihat informasi CPU:**
Ketik: cat cpuinfo 

**melihat jenis prosesor yang digunakan di VM.**

**Melihat informasi Memori (RAM):**
Ketik: cat meminfo 

**Melihat waktu jalan sistem (Uptime):**
Ketik: cat uptime 

### 5. Ubahlah direktory home ke user lain secara langsung menggunakan cd ~username. 
ketik: cd ~root >> "Permission denied"

**Simbol ~ (tilde) diikuti nama user adalah cara singkat untuk menuju home directory spesifik.**

**Jika memasukan ke user yang tidak ada, terminal akan memberikan pesan error.**

**/root adalah home directory khusus untuk superuser(administrator).**

### 6. Ubah kembali ke direktory home Anda.
cukup mngetekik cd lalu pwd saja

### 7. Buat subdirektory work dan play.
ketik: **mkdir** work play
lalu ls -l >>
```drwxrwxr-x 2 rasyiqtaps rasyiqtaps  4096 Mar 10 13:29 play
drwxrwxr-x 3 rasyiqtaps rasyiqtaps  4096 Feb 24 13:48 praktikum-os
-rw-rw-r-- 1 rasyiqtaps rasyiqtaps   210 Feb 24 15:24 server.log
-rw-rw-r-- 1 rasyiqtaps rasyiqtaps   253 Mar  3 09:28 sorted-users.txt
drwxrwxr-x 2 rasyiqtaps rasyiqtaps  4096 Mar 10 13:29 work
```

### 8. Hapus subdirektory work. 
ketik **rmidir** work
cek di ls -l sudah tidak ada

### 9. Copy file /etc/passwd ke direktory home Anda.
ketik: **cp /etc/passwd .** 
dan cek di ls -l >> 
```
-rw-r--r-- 1 rasyiqtaps rasyiqtaps  1795 Mar 10 13:33 passwd
```

### 10. Pindahkan ke subirectory play. 
ketik: **mv passwd play/**
cek di ls -l play/ >> 
```
total 4
-rw-r--r-- 1 rasyiqtaps rasyiqtaps 1795 Mar 10 13:33 passwd
```
* ini berati file passwd sudah tidak di home, sudah dipindah kesalam folder play


### 11. Ubahlah ke subdirektory play dan buat symbolic link dengan nama terminal yang menunjuk ke perangkat tty. Apa yang terjadi jika melakukan hard link ke perangkat tty ? 
ketik: cd play 
lalu ketik : **ln -s /dev/rasyiqtaps terminal**
lalu cek ls -l >>
```
total 4
-rw-r--r-- 1 rasyiqtaps rasyiqtaps 1795 Mar 10 13:33 passwd
lrwxrwxrwx 1 rasyiqtaps rasyiqtaps   15 Mar 10 13:41 terminal -> /dev/rasyiqtaps
```
* terminal dengan tanda l di awal baris dan panah -> /dev/tty1. Ini berarti terminal adalah penunjuk ke perangkat

* lalu jika Apa yang terjadi jika melakukan hard link ke perangkat? 
: Jika mencoba ln /dev/tty1 terminal_hard, sistem biasanya akan memberikan pesan error "Invalid cross-device link" atau "Operation not permitted".dikarenakan hard link terbatas pada partisi disk yang sama dan tidak dimungkinkan untuk dibuat pada file perangkat (special file) atau direktori.

### 12. Buatlah file bernama hello.txt yang berisi kata ”hello word”. Dapatkah Anda gunakan ”cp” menggunakan ”terminal” sebagai file asal untuk menghasilkan efek yang sama ? 
cd terlebih dahulu lalu ketik: **echo "hello world" > hello.txt**
lalu panggil menggunakan cat hello.txt akan keluar
```
hello world
```
* lalu jika Dapatkah Anda gunakan cp menggunakan terminal sebagai file asal untuk menghasilkan efek yang sama?
jawabannya : Tidak bisa secara langsung karena Jika mencoba cp terminal tes.txt, perintah tersebut akan mencoba menyalin isi dari perangkat terminal (yang sedang menunggu input), bukan menyalin teks "hello world".

### 13. Copy hello.txt ke terminal. Apa yang terjadi ? 
ketik: **cp hello.txt terminal**
lalu cek cat terminal >>
```
hello world
```
* Tulisan "hello world" akan muncul lagi di layar terminalmu (seperti hasil perintah cat). karena terminal adalah symbolic link yang mengarah ke /dev/tty.

### 14. Masih direktory home, copy keseluruhan direktory play ke direktory bernama work menggunakan symbolic link. 
kembali ke awal (cd ~)
ketik: **ln -s /home/rasyiqtaps/play /home/rasyiqtaps/work**
cek hasilnya di ls -l >>
```
lrwxrwxrwx 1 rasyiqtaps rasyiqtaps    21 Mar 10 13:53 work -> /home/rasyiqtaps/play
```
* folder work sekarang adalah "bayangan" atau shortcut dari folder play.

### 15. Hapus direktory work dan isinya dengan satu perintah
ketik: **rm -rf work**
cek di ls -l 

* Opsi -r (recursive) digunakan untuk menghapus direktori beserta isinya.

* Opsi -f (force) digunakan agar sistem tidak meminta konfirmasi untuk setiap file yang dihapus.

* Setelah perintah ini dijalankan, folder work akan hilang dari daftar.*