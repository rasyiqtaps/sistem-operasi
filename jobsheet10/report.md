# Laporan Praktikum 10

<h4>Nama : Rasyiq Satrio Musthafa<h4>
<h4>Nim : 254107020079<h4>
<h4>Kelas : TI-1G<h4>

## PRAKTIKUM

## Praktikum 10.1

### Langkah 1
Input
```bash
free -h
```

Output
```bash
root@rasyiqtaps:~/praktikum-os/week10-memory# free -h
               total        used        free      shared  buff/cache   available
Mem:           3.3Gi       378Mi       2.8Gi       1.1Mi       355Mi       3.0Gi
Swap:          511Mi          0B       511Mi
```

### Langkah 2
Input
```bash
cat /proc/meminfo | head -n 20
```

Output
```bash
root@rasyiqtaps:~/praktikum-os/week10-memory# cat /proc/meminfo | head -n 20
MemTotal:        3504528 kB
MemFree:         2909672 kB
MemAvailable:    3116948 kB
Buffers:           21472 kB
Cached:           323112 kB
SwapCached:            0 kB
Active:           310416 kB
Inactive:          82940 kB
Active(anon):      58608 kB
Inactive(anon):        0 kB
Active(file):     251808 kB
Inactive(file):    82940 kB
Unevictable:       27316 kB
Mlocked:           27316 kB
SwapTotal:        524284 kB
SwapFree:         524284 kB
Zswap:                 0 kB
Zswapped:              0 kB
Dirty:                 0 kB
Writeback:             0 kB
```

### Analisis
1. Hitung persentase memori tersedia: available / total × 100%. Jika hasilnya di bawah 10%, sistem mulai kekurangan memori.
2. Pada baris Swap, apakah kolom used bernilai 0? Jika lebih dari 0, kernel sudah pernah memindahkan data ke disk karena RAM tidak cukup.
3. Perhatikan field Cached dan Buffers di /proc/meminfo. Nilai ini sesuai dengan kolom buff/cache pada free -h.

**Jawaban:**  
1. Perhitungannya adalah (3116948/3504528) * 100%, yang menghasilkan persentase ketersediaan memori sekitar 88,94%[cite: 6].
2. Benar, hal tersebut mengindikasikan bahwa kernel belum melakukan aktivitas pemindahan data ke area swap sama sekali, dikarenakan kapasitas RAM yang terpasang masih sangat memadai[cite: 6].
3. Berdasarkan hasil pengecekan, nilai Buffers adalah 21472 kB dan Cached sebesar 323112 kB. Angka-angka ini sudah sinkron dengan informasi yang tertera pada kolom buff/cache ketika mengeksekusi perintah `free -h`[cite: 6].


## Praktikum 10.2

### Langkah 1
Input
```bash
vmstat 1 5
```

Output
```bash
root@rasyiqtaps:~/praktikum-os/week10-memory# vmstat 1 5
procs -----------memory---------- ---swap-- -----io---- -system-- -------cpu-------
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st gu
 2  0      0 2910952  21512 342800    0    0   105    16 1114    0  0  7 93  0  0  0
 0  0      0 2910952  21512 342800    0    0     0     0 1244  212  0 11 89  0  0  0
 0  0      0 2910952  21512 342800    0    0     0     0 1278  208  0 11 89  0  0  0
 0  0      0 2910952  21512 342800    0    0     0     0 1211  159  0  9 91  0  0  0
 0  0      0 2910952  21512 342800    0    0     0     0 1193  102  0  7 93  0  0  0
```

### Analisis
1. Amati nilai si dan so pada kelima baris. Pada sistem normal dengan RAM cukup, kedua nilai ini selalu 0.
2. Jika nilai si atau so sesekali muncul lebih dari 0, artinya pernah ada aktivitas swap. Ini masih wajar jika tidak terus-menerus.
3. Jika si dan so terus-menerus lebih dari 0, sistem dalam kondisi memory pressure serius — performa turun drastis karena akses disk jauh lebih lambat dari RAM.
4. Perhatikan juga kolom free (RAM kosong) dan buff (buffer) untuk memahami kondisi keseluruhan RAM saat itu.

**Jawaban:**   
1. Berdasarkan sampel, kedua nilai (`si` dan `so`) konsisten menunjukkan angka 0[cite: 6].
2. Pada seluruh baris observasi, kedua nilai tersebut tetap berada di angka 0 dan tidak menunjukkan lonjakan[cite: 6].
3. Kondisinya tetap stabil, tidak ditemukan aktivitas di mana kedua nilai tersebut melebihi 0 secara kontinu[cite: 6].
4. Pada output tersebut, kolom `free` memperlihatkan ketersediaan sekitar 2.91 GB memori yang masih kosong, dan kolom `cache` mengalokasikan memori sekitar 3.42 GB sebagai cache file. Hal ini mengonfirmasi bahwa beban penggunaan RAM masih sangat ringan[cite: 6].


## Praktikum 10.3

### Langkah 1
Input
```bash
sudo fallocate -l 512M /swapfile-week10
ls -lh /swapfile-week10
```

Output
```bash
root@rasyiqtaps:~/praktikum-os/week10-memory# sudo fallocate -l 512M /swapfile-week10
root@rasyiqtaps:~/praktikum-os/week10-memory# ls -lh /swapfile-week10
-rw-r--r-- 1 root root 512M May  4 18:55 /swapfile-week10
```

### Langkah 2
Input
```bash
sudo chmod 600 /swapfile-week10
ls -lh /swapfile-week10
```

Output
```bash
root@rasyiqtaps:~/praktikum-os/week10-memory# sudo chmod 600 /swapfile-week10
root@rasyiqtaps:~/praktikum-os/week10-memory# ls -lh /swapfile-week10
-rw------- 1 root root 512M May  4 18:55 /swapfile-week10
```

### Langkah 3
Input
```bash
sudo mkswap /swapfile-week10
sudo swapon /swapfile-week10
```

Output
```bash
root@rasyiqtaps:~/praktikum-os/week10-memory# sudo mkswap /swapfile-week10
Setting up swapspace version 1, size = 512 MiB (536866816 bytes)
no label, UUID=561a9ccd-7b32-45c2-9ba3-ba83a1c3b128
root@rasyiqtaps:~/praktikum-os/week10-memory# sudo swapon /swapfile-week10
```

### Langkah 4
Input
```bash
swapon --show
free -h
```

Output
```bash
root@rasyiqtaps:~/praktikum-os/week10-memory# swapon --show
NAME             TYPE SIZE USED PRIO
/swapfile-week10 file 512M   0B   -2
root@rasyiqtaps:~/praktikum-os/week10-memory# free -h
               total        used        free      shared  buff/cache   available
Mem:           3.3Gi       365Mi       2.8Gi       1.1Mi       361Mi       3.0Gi
Swap:          511Mi          0B       511Mi
```

### Langkah 5
Input
```bash
cat /proc/sys/vm/swappiness
sudo sysctl vm.swappiness=10
cat /proc/sys/vm/swappiness
```

Output
```bash
root@rasyiqtaps:~/praktikum-os/week10-memory# cat /proc/sys/vm/swappiness
60
root@rasyiqtaps:~/praktikum-os/week10-memory# sudo sysctl vm.swappiness=10
vm.swappiness = 10
root@rasyiqtaps:~/praktikum-os/week10-memory# cat /proc/sys/vm/swappiness
10
```

### Analisis
1. Berapa nilai swappiness default? Apa artinya bagi perilaku kernel dalam menggunakan swap?
2. Setelah diubah ke 10, konfirmasi nilai berubah pada output cat kedua. Apa dampak nilai 10 terhadap penggunaan swap dibanding nilai 60?
3. Apakah entri /swapfile-week10 muncul di swapon –show? Jika tidak, pastikan Langkah 2 (chmod 600) sudah dijalankan sebelum Langkah 3.

**Jawaban:**  
1. Nilai default-nya adalah 60. Angka ini menandakan bahwa kernel memiliki kecenderungan cukup agresif dalam menggunakan area swap; kernel akan mulai memindahkan page yang tidak aktif meskipun sisa kapasitas RAM masih tergolong wajar. Setting ini optimal bagi environment desktop sehari-hari, namun kurang disarankan bagi kebutuhan server yang menuntut performa tinggi[cite: 6].
2. Pengubahan parameter menjadi 10 bertujuan membuat performa sistem beroperasi lebih stabil dengan cara meminimalkan terjadinya paging ke disk. Namun di sisi lain, konfigurasi ini meningkatkan potensi kehabisan alokasi RAM apabila terjadi pelonjakan beban kerja yang tiba-tiba[cite: 6].
3. Ya, entri bersangkutan berhasil terdeteksi dan tercetak dengan benar pada output perintah `swapon --show`[cite: 6].


## Praktikum 10.4

### Langkah 1
Input
```bash
ps aux --sort=-%mem | head
```

Output
```bash
root@rasyiqtaps:~/praktikum-os/week10-memory# ps aux --sort=-%mem | head
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root        1094  0.2  1.2 617808 44080 ?        Ssl  18:40   0:09 /usr/libexec/fwupd/fwupd
root         378  0.1  0.7 288988 27324 ?        SLsl 18:39   0:07 /sbin/multipathd -d -s
root         701  0.0  0.6 109688 23132 ?        Ssl  18:39   0:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-for-signal
root         325  0.0  0.5  66840 17888 ?        S<s  18:39   0:00 /usr/lib/systemd/systemd-journald
root         638  0.0  0.3 468972 13544 ?        Ssl  18:39   0:00 /usr/libexec/udisks2/udisksd
root           1  0.0  0.3  22044 13280 ?        Ss   18:39   0:02 /sbin/init splash noprompt noshell automatic-ubiquity
root         703  0.0  0.3 392100 13072 ?        Ssl  18:39   0:00 /usr/sbin/ModemManager
systemd+     458  0.0  0.3  21588 12996 ?        Ss   18:39   0:00 /usr/lib/systemd/systemd-resolved
root        1380  0.0  0.3  20292 11492 ?        Ss   19:00   0:00 /usr/lib/systemd/systemd --user
```

### Langkah 2
Input
```bash
top
```

Output
```bash
root@rasyiqtaps:~/praktikum-os/week10-memory# top
top - 19:04:58 up  1:25,  2 users,  load average: 0.00, 0.00, 0.00
Tasks: 139 total,   1 running, 138 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.2 us,  1.7 sy,  0.0 ni, 91.3 id,  0.0 wa,  0.0 hi,  6.9 si,  0.0
MiB Mem : 10.8/3422.4   [|||||                                             ]
MiB Swap:  0.0/512.0    [                                                  ]

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+
   1365 rasyiqt+  20   0   13764   7512   6008 S   5.6   0.2   0:07.30
     30 root      20   0       0      0      0 S   2.0   0.0   0:15.56
   1507 root      20   0       0      0      0 I   1.5   0.0   0:03.80
    766 root      20   0  293152   3764   3284 S   0.5   0.1   0:05.57
   1304 root      20   0       0      0      0 I   0.5   0.0   0:00.42
   1582 root      20   0   11940   6008   3780 R   0.5   0.2   0:00.03
      1 root      20   0   22044  13292   9532 S   0.0   0.4   0:02.30
...
```

### Analisis
1. Proses apa yang berada di urutan pertama? Catat nilai %MEM dan RSS-nya.
2. Konversikan RSS dari KB ke MB (bagi 1024). Misalnya, RSS=524288 berarti proses menggunakan 512 MB RAM. Apakah wajar untuk jenis program tersebut?
3. Mengapa VSZ selalu lebih besar dari RSS pada proses yang sama?
4. Apakah urutan proses di ps konsisten dengan tampilan top saat diurutkan berdasarkan %MEM?

**Jawaban:**  
1. Proses yang berada di pucuk daftar adalah milik user `rasyiqtaps` (PID 1365). Secara spesifik, proses tersebut mengonsumsi %MEM di angka 0.2 dengan nilai RSS sejumlah 7512[cite: 6].
2. Hasil konversi RSS dari 7512 ke MB menunjukkan bahwa proses ini menghabiskan ruang memori RAM di kisaran 7 MB. Besaran ini dapat dikategorikan amat wajar[cite: 6].
3. VSZ umumnya mencakup totalitas pemetaan memori virtual, seperti shared libraries, file yang dipetakan ke memori, hingga buffer/heap yang telah dialokasikan namun belum terpakai secara fisik. Di sisi lain, pengukuran RSS murni mendata page yang betul-betul sedang menempati ruang RAM secara nyata[cite: 6].
4. Benar, umumnya akan terlihat konsisten mengingat kedua utilitas tersebut mereferensikan sumber data yang identik dari `/proc`. Sedikit perbedaan yang mungkin muncul biasanya diakibatkan oleh selisih temporal (jeda waktu eksekusi) antara perintah `ps` dengan `top`, di mana beberapa proses mungkin saja baru tercipta atau telah dihentikan[cite: 6].


## Praktikum 10.5

### Langkah 1
Input
```bash
nano monitor-memori.sh
#!/bin/bash
set -euo pipefail

THRESHOLD=20

echo "=== Monitor Memori ==="
date
echo

free -h

echo

AVAIL=$(free | awk '/Mem/ {printf "%d", $7/$2*100}')
if [ "$AVAIL" -lt "$THRESHOLD" ]; then
    echo "PERINGATAN: Memori tersedia hanya ${AVAIL}%"
else
    echo "Status: Memori tersedia ${AVAIL}% (normal)"
fi

echo
echo "--- 5 Proses Memori Tertinggi ---"
ps aux --sort=-%mem | head -n 6 | tail -n 5
```

### Langkah 2
Input
```bash
chmod +x monitor-memori.sh
bash monitor-memori.sh
```

Output
```bash
root@rasyiqtaps:~/praktikum-os/week10-memory# chmod +x monitor-memori.sh
root@rasyiqtaps:~/praktikum-os/week10-memory# bash monitor-memori.sh
=== Monitor Memori ===
Mon May  4 19:22:07 WIB 2026

               total        used        free      shared  buff/cache   available
Mem:           3.3Gi       369Mi       2.8Gi       1.1Mi       362Mi       3.0Gi
Swap:          511Mi          0B       511Mi

Status: Memori tersedia 89% (normal)

--- 5 Proses Memori Tertinggi ---
root        1094  0.1  1.2 617808 44080 ?        Ssl  18:40   0:10 /usr/libexec/fwupd/fwupd
root         378  0.1  0.7 288988 27324 ?        SLsl 18:39   0:09 /sbin/multipathd -d -s
root         701  0.0  0.6 109688 23132 ?        Ssl  18:39   0:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-for-signal
root         325  0.0  0.5  66840 17900 ?        S<s  18:39   0:00 /usr/lib/systemd/systemd-journald
root         638  0.0  0.3 468972 13544 ?        Ssl  18:39   0:00 /usr/libexec/udisks2/udisksd
```

### Analisis
1. Variabel THRESHOLD=20 menetapkan batas persentase. Perintah free | awk ’/Mem/ {printf "%d", $7/$2*100}’ mengambil kolom ke-7 (available) dibagi kolom ke-2 (total) dari baris Mem, lalu dikalikan 100 untuk menghasilkan persentase bilangan bulat.
2. Kondisi if [ "$AVAIL" -lt "$THRESHOLD" ] bernilai benar jika persentase memori tersedia di bawah 20.
3. Ubah THRESHOLD menjadi 90 dan jalankan ulang. Apa yang berubah pada output? Mengapa demikian?

**Jawaban:**  
3. Pada pengaturan awal di mana THRESHOLD bernilai 20, sistem mengkalkulasi AVAIL di angka 66%. Karena 66 lebih besar dari 20, pesan yang tampil adalah status "normal". Apabila ambang batas THRESHOLD direvisi menjadi 90%, sistem secara otomatis akan mencetak pesan peringatan. Hal tersebut terjadi akibat angka avaibility 66% kini jatuh di bawah standar aman yang baru saja ditetapkan (90%), walau dari sisi teknis kondisi performa memori sedang baik-baik saja[cite: 6].


## Praktikum 10.6

### Langkah 1
Input
```bash
strace ls 2>&1 | head -n 30
```

*(Catatan: Output sebagian besar merupakan log sistem asli sehingga tidak ada perubahan).*

### Langkah 2
Input
```bash
strace -c ls
strace -c ls /etc 2>&1 | tail -5
```

### Analisis
1. Dari output Langkah 1, identifikasi minimal 4 system call berbeda. Jelaskan fungsi singkat masing-masing berdasarkan argumen yang terlihat.
2. Dari ringkasan strace -c, system call mana yang paling sering dipanggil? Mengapa?
3. Apakah ada system call dengan errors lebih dari 0? Apakah itu berarti program bermasalah, ataukah bagian normal dari logika program?
4. Apakah jumlah system call berbeda antara ls dan ls /etc? Faktor apa yang menyebabkan perbedaan tersebut?

**Jawaban:**  
1. Beberapa ragam system call yang terpantau[cite: 6]:
   * `openat()` : Berperan membuka jalur akses ke dalam direktori maupun file yang diidentifikasi[cite: 6].
   * `read()` : Menerima dan membaca muatan data melewati referensi file descriptor[cite: 6].
   * `write()` : Mengeksekusi penulisan atau output data, misalnya mencetak karakter ke layar[cite: 6].
   * `close()` : Berfungsi memutuskan sesi dan mengakhiri penggunaan dari file descriptor[cite: 6].
2. Rekor pemanggilan tertinggi dicetak oleh system call `mmap` (18 panggilan), diikuti dengan `close` (9), lalu `openat` (7)[cite: 6].
3. Benar, ditemukan jumlah error yang cukup berarti pada perintah `access` (2 error) serta `openat` (5 error). Meskipun demikian, peristiwa ini sama sekali bukan menandakan program mengalami crash. Proses perlakuan seperti saat memanggil fungsi `access("/etc/ld.so.nohwcap", F_OK)` sekadar melakukan pengecekan rutinitas, dan mengalami "gagal" dikarenakan file sasarannya memang nihil. Karena skenario ini telah diantisipasi di level program, eksekusi logikanya akan berjalan mulus tanpa kendala[cite: 6].
4. Dari penelusuran, intensitas kuantitas pemanggilan system call dari keduanya justru seragam di angka 74 instruksi[cite: 6].


## STUDI KASUS

## Studi Kasus 10.1

### Langkah 1
Input
```bash
free -h
```

Output
```bash
root@rasyiqtaps:~/praktikum-os/week10-memory# free -h
               total        used        free      shared  buff/cache   available
Mem:           3.3Gi       378Mi       2.8Gi       1.1Mi       355Mi       3.0Gi
Swap:          511Mi          0B       511Mi
```

### Langkah 2
Input
```bash
top
```

### Analisis
1. Apakah nilai available sangat kecil (misalnya di bawah 200 MB pada server dengan RAM 2 GB)? Jika ya, server kemungkinan kekurangan memori.
2. Apakah kolom used pada baris Swap lebih dari 0? Jika ya, kernel sedang menggunakan swap, yang berarti performa menurun.
3. Di tampilan top, proses apa yang memiliki %MEM terbesar? Proses tersebut menjadi kandidat utama penyebab lambatnya server.

**Jawaban:**  
1. Tidak ada tanda bahaya. Statistik memperlihatkan sisa yang `available` bertengger mantap di angka 3.0 GiB dibanding kapasitas total 3.3 GiB, membuktikan betapa lega ruang kerjanya. Sistem baru divonis krisis memori seandainya sisa ruang `available` terperosok ke zona di bawah 200 MB (dengan asumsi server ber-RAM 2 GB)[cite: 6].
2. Tidak benar. Buktinya, indikator pemakaian mendata nilai 0B di bagian swap. Hal ini menerangkan bahwa proses kernel sepenuhnya bertumpu pada RAM asli tanpa intervensi akses disk swap yang dapat menghambat durasi pemrosesan performa server[cite: 6].
3. Proses paling haus memori dipuncaki oleh akun root dengan detail PID 378 (untuk perintah multipathd)[cite: 6].


## Studi Kasus 10.2

### Langkah 1
Input
```bash
mkdir -p ~/praktikum-os/week10-memory/syscall-case
cd ~/praktikum-os/week10-memory/syscall-case
echo "PORT=8080" > app.conf
ls -l app.conf
cat app.conf
```

Output
```bash
root@rasyiqtaps:~/praktikum-os/week10-memory# mkdir -p ~/praktikum-os/week10-memory/syscall-case
root@rasyiqtaps:~/praktikum-os/week10-memory# cd ~/praktikum-os/week10-memory/syscall-case
root@rasyiqtaps:~/praktikum-os/week10-memory/syscall-case# echo "PORT=8080" > app.conf
root@rasyiqtaps:~/praktikum-os/week10-memory/syscall-case# ls -l app.conf
-rw-r--r-- 1 root root 10 May  4 19:15 app.conf
root@rasyiqtaps:~/praktikum-os/week10-memory/syscall-case# cat app.conf
PORT=8080
```

### Langkah 2
Input
```bash
chmod 000 app.conf
ls -l app.conf
sudo -u nobody cat app.conf
```

Output
```bash
root@rasyiqtaps:~/praktikum-os/week10-memory/syscall-case# chmod 000 app.conf
root@rasyiqtaps:~/praktikum-os/week10-memory/syscall-case# ls -l app.conf
---------- 1 root root 10 May  4 19:15 app.conf
root@rasyiqtaps:~/praktikum-os/week10-memory/syscall-case# sudo -u nobody cat app.conf
cat: app.conf: Permission denied
```

### Langkah 3
Input
```bash
chmod 644 app.conf
ls -l app.conf
cat app.conf
```

### Analisis
1. Mengapa cat menghasilkan Permission denied setelah chmod 000? System call apa yang gagal?
2. Apa perbedaan pesan error Permission denied vs No such file or directory? Coba rm app.conf lalu cat app.conf untuk melihat perbedaannya.
3. Permission 644 berarti apa untuk owner, group, dan others?

**Jawaban:**  
1. Instruksi perizinan `chmod 000` telah mencabut hak akses menyeluruh (khususnya untuk baca/r) bagi siapa pun entitas penggunanya. Imbasnya, di kala perintah `cat` memulai permohonan ke tingkat kernel guna menampilkan isi file tersebut, otorisasi sistem langsung melayangkan penolakan keras[cite: 6].
2. Pemberitahuan "Permission denied" (status EACCES) memastikan bahwa keberadaan file tersebut dapat dikonfirmasi, namun hak prerogatif pengaksesan belum tercukupi. Berbeda dari "No such file or directory" (status ENOENT), yang menggarisbawahi bahwa obyek direktori beserta file-nya fiktif alias lenyap dari sistem[cite: 6].
3. Rincian perizinan bermetode 644 (yakni rw-r--r--) mencerminkan[cite: 6]:
   * Kelompok Owner (dipegang root) : Mendapat kebebasan merevisi dan membaca (rw- disetarakan kode 6)[cite: 6].
   * Entitas Group : Dibatasi untuk memonitor pembacaan semata (r-- direpresentasikan kode 4)[cite: 6].
   * Entitas Others : Juga senasib, hanya difasilitasi wewenang membaca isi (r-- direpresentasikan kode 4)[cite: 6].


## TUGAS

## Tugas 10.1

### Langkah 1 & 2
Input :
```bash
nano memory-audit.sh
```

### Langkah 3
Input :
```bash
chmod +x ~/praktikum-os/week10-memory/memory-audit.sh
cd ~/praktikum-os/week10-memory
bash memory-audit.sh
```

Output : 
```bash
root@rasyiqtaps:~/praktikum-os/week10-memory# chmod +x ~/praktikum-os/week10-memory/memory-audit.sh
root@rasyiqtaps:~/praktikum-os/week10-memory# cd ~/praktikum-os/week10-memory
root@rasyiqtaps:~/praktikum-os/week10-memory# bash memory-audit.sh
Laporan disimpan ke: memory-report.txt
=== LAPORAN MEMORI SISTEM ===
Mon May  4 19:39:37 WIB 2026

--- Ringkasan free -h ---
               total        used        free      shared  buff/cache   available
Mem:           3.3Gi       412Mi       1.3Gi       1.1Mi       1.9Gi       2.9Gi
Swap:             0B          0B          0B
...
```

### Analisis
1. Hitung persentase memori tersedia (available / total × 100%). Apakah sistem dalam kondisi normal?
2. Mengapa buff/cache tidak dihitung sebagai memori yang terpakai dari sudut pandang ketersediaan untuk aplikasi?
3. Dari /proc/meminfo, apakah SwapTotal lebih besar dari 0? Berapa nilai SwapFree?

**Jawaban:**  
1. Sesuai dengan hasil eksekusi (3082388/3504528) * 100%, maka tercatatlah kisaran persentase yang merujuk di level 87.95%. Kondisi semacam ini merepresentasikan performa vital yang sungguh normal[cite: 6].
2. Kapasitas pada blok pengalokasian `buff/cache` tak digolongkan pada baris area terpakai (used memory). Arsitektur Linux mengelola hal tersebut agar saat proses di sisi aplikasi mendelegasikan ruang memori yang mendesak, serpihan sisa memori ini dapat secara instan dilepas di dalam latar eksekusi secara diam-diam tanpa memunculkan interupsi serius[cite: 6].
3. Pada catatan tersebut, tidak pernah termuat nilai pengakumulasian yang menginjak lebih dari 0 pada SwapTotal, yang dibarengi pula oleh perolehan nilai parameter SwapFree yang tetap bertahan murni di angka 0[cite: 6].


## Tugas 10.2

### Langkah 1
Input :
```bash
ps aux --sort=-%mem | head -n 10 > top-memory-process.txt
cat top-memory-process.txt
```

### Analisis
1. Proses apa di urutan pertama? Catat nilai %MEM dan RSS.
2. Konversikan RSS ke MB (bagi 1024). Apakah wajar?
3. Jumlahkan %MEM dari 5 proses teratas. Berapa persen RAM yang mereka gunakan bersama?

**Jawaban:**  
1. Aplikasi dengan tingkat pemakaian resource terekstrem adalah entitas `multipathd` pada urutan paling awal, membawakan perolehan sumbangan %MEM yang mencapai 0.7 persen, dipadu dengan volume RSS membengkak menjadi 27308 KB[cite: 6].
2. Pengkalkulasian matematis mempertemukan perolehan (27308/1024) = 26.66 MB RAM. Konversi ukuran program daemon ini tergolong sangat ideal lantaran spesifikasi beban kerjanya diharuskan mengontrol manajemen rumit seputar lintasan media storage server[cite: 6].
3. Akumulasi persentasenya berlabuh pada persentase 2.2 persen[cite: 6]. Konklusi yang dapat ditarik yakni meskipun menjadi lima proses tersubur dalam menggerogoti suplai RAM, secara komunal pemanfaatan mereka atas resource sentral hanyalah sebatas 2.2 persen saja[cite: 6].


## Tugas 10.3

### Langkah 1 & 2
Input :
```bash
sudo fallocate -l 256M /swapfile-tugas-week10
ls -lh /swapfile-tugas-week10
sudo chmod 600 /swapfile-tugas-week10
sudo mkswap /swapfile-tugas-week10
sudo swapon /swapfile-tugas-week10
swapon --show
```

### Analisis
1. Identifikasi kolom NAME, TYPE, SIZE, dan USED pada output swapon –show.
2. Apakah nilai total pada baris Swap di free -h bertambah 256 MB?
3. Mengapa permission 600 penting? Apa risiko jika diatur ke 644?

**Jawaban:**  
1. Elaborasi terkait indikator kolom tersebut dirinci menjadi[cite: 6]:
   * NAME: `/swapfile-tugas-week10`[cite: 6].
   * TYPE: Memiliki format tipe data sebuah `file`[cite: 6].
   * SIZE: Menampung volume 256M[cite: 6].
   * USED: Status pemakaian saat ini yaitu 0B[cite: 6].
2. Terjadi pergantian secara valid di mana rentetan angka kosong (0) sebelumnya berevolusi membengkak mencapai kisaran kapasitas 255/256 MB[cite: 6].
3. Pada level fundamental, sebuah berkas swap merupakan muara wadah penyimpanan data bagi kepingan riwayat lalu lintas per memori yang dimutasi dari sebuah instruksi yang sedang beroperasi[cite: 6]. Kernel menggunakan file ini sebagai ruang pelarian kompensasi saat performa utama terbebani penuh[cite: 6]. Manakala aturan dibiarkan terkonfigurasi pada hak tingkat 644 (secara harfiah mewakili pola rw-r--r--), hal tersebut akan mengekspos keamanan sistem sehingga oknum sembarang atau semua lapisan pengguna mampu melihat kepingan informasi perihal hal ihwal privasi dari sistem[cite: 6].


## Tugas 10.4

### Langkah 1
Input :
```bash
strace -c ls 2> strace-summary.txt
strace ls /etc 2> strace-ls-etc.txt
cat strace-summary.txt
```

### Analisis
1. Sebutkan minimal 5 system call dari strace-summary.txt beserta fungsi singkatnya.
2. System call mana yang paling sering dipanggil? Mengapa?
3. Apakah ada errors lebih dari 0? Apakah program tetap berjalan normal meskipun ada kegagalan tersebut?

**Jawaban:**   
1. Detail dari kelima tipe call mendasar dari dokumentasinya adalah[cite: 6]:
   * `mmap` = Menautkan file bersangkutan dari luar menuju ke ranah pemetaan internal kepingan memori[cite: 6].
   * `close` = Mensyaratkan instruksi formal pada tahapan pengakhiran fungsi pemanggilan pada satu deskripsi file descriptor[cite: 6].
   * `execve` = Memicu tahapan eksekusi instruksi baru per file program yang dijalankan[cite: 6].
   * `access` = Menuntaskan mandat berupa memverifikasi validitas perizinan berkas target[cite: 6].
   * `openat` = Menggariskan prosedur akses awal guna memeriksa dan melihat data-data dalam sub direktori[cite: 6].
2. Kategori panggilan yang membukukan rekor porsi pengaksesan terhebat dikuasai peranan `mmap` sebanyak total 18 pengulangan[cite: 6]. Tuntutan operasi macam ini mewajibkan prosesor buat berulang kali mendistribusikan perbendaharaan modul library eksternalnya memasuki dimensi memori secara presisi guna memanfaatkan standar instruksi bahasa pemrograman C dengan semestinya[cite: 6].
3. Catatan kompilasinya membuahkan kegagalan hingga sejumlah total 4 indikasi eror (yaitu 2 pada akses `access` dan 2 melanda eksekusi pada `statfs`)[cite: 6]. Performa keseluruhan alur pemrograman sama sekali tak tersendat memgingat mekanisme pengalamatan semacam ini sedari awal senantiasa terduga; contohnya indikasi gagalnya access murni terbukti akibat rujukan dari `/etc/ld.so.nohwcap` betul-betul dikonfirmasi tidak dipasang di perangkat mesin[cite: 6].


## Tugas 10.5

### Langkah 1 & 2
Input :
```bash
nano diagnosa-server.sh
```

### Langkah 3
Input :
```bash
chmod +x diagnosa-server.sh
cd ~/praktikum-os/week10-memory
bash diagnosa-server.sh
```

### Analisis
1. Jelaskan peran masing-masing fungsi: cek_memori, cek_swap, cek_proses, cek_paging, dan ringkasan. Mengapa diagnosa dipecah menjadi fungsi terpisah?
2. Berdasarkan bagian RINGKASAN, apakah kondisi sistem normal atau kritis? Jelaskan berdasarkan nilai threshold yang digunakan script.
3. Mengapa script menggunakan tee "$LAPORAN" bukan redirection biasa > "$LAPORAN"? Apa keuntungannya?
4. Dari output cek_paging, apakah ada aktivitas si atau so? Jika ada, apa implikasinya terhadap performa server?

**Jawaban:**  
1. Pendekatan perancangan kode program terstruktur secara terpisahnya fungsionalitas memiliki filosofi kuat semata buat menyajikan struktur yang tertata, lebih interaktif tatkala dinavigasikan, serta mereduksi kesulitan dalam pelacakan titik kesalahan[cite: 6]. Analogi fungsinya diterangkan di bawah:
   * `cek_memori` = Mendesimulasikan kesehatan RAM serta mempresentasikan hitungan estimasi metrik ketersediaannya[cite: 6].
   * `cek_swap` = Menyelidiki lalu mencatat alokasi cadangan virtual (swap space)[cite: 6].
   * `cek_proses` = Menata deretan rincian informasi urutan aplikasi dengan volume tanggungan terbesar[cite: 6].
   * `cek_paging` = Menginisiasi tahapan rekaman swap lintas ranah (metode in dan metode out) menggunakan perlakuan `vmstat`[cite: 6].
   * `ringkasan` = Menyetorkan ulasan simpulan final demi merangkum diagnosis paripurna per metrik kesehatan mesin tersebut[cite: 6].
2. Ekosistem terpantau senantiasa berjalan di koridor kesehatan yang normal[cite: 6]. Pasalnya proporsi persentase sisa kuota yang diistilahkan `available` bersemayam secara dominan sejauh rasio volume 2.9 GB (mencapai sekitar 88% di kalkulasi dari angka komunalitasnya yakni kapasitas utuh 3.3 GB)[cite: 6]. Ini diperkokoh pula per swap sama sekali tiada terganggu penggunaannya, hal mana menjauhi kriteria titik krisis performa yang mengkhawatirkan[cite: 6].
3. Alasan metodis eksistensi tata cara pembubuhan sintaks tipe `tee "$LAPORAN"` sungguh tak lain disebabkan nilai gunanya dalam memberdayakan visualisasi output ganda; selain dicetak dengan jernih di bilik layar promp, di sepersekian detik secara beruntun arsip hasil dokumentasinya bakal dialihkan ke suatu arsip dengan kokoh ketimbang mengadopsi redirection satu sisi (`>`)[cite: 6].
4. Dari deretan metrik lalu lintas indikasi parameter `si` berjejer berdampingan bersama pasangannya si nilai parameter `so`, semuanya stagnan merujuk rekapitulasi persentase kemutlakan poin angka 0 mutlak[cite: 6]. Skenario menguntungkan semacam ini mengamankan pergerakan mulus dan gempur maksimal prosesor dari bahaya membuang-buang laju pertukaran sinkronisasi (paging method) menuju sisi ruang pergerakan yang tersimpan di disk lambat tersebut[cite: 6].

*** 

Jika ada penyesuaian lain yang dibutuhkan, langsung kasih tahu aja ya. Semoga lancar!