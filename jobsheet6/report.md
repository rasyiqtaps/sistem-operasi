# Laporan Praktikum Sistem Operasi Pertemuan 4

**Tanggal:** 6 April 2026  
**Disusun Oleh:** Rasyiq Satrio Musthafa
**NIM:** 254107020079  
**Kelas/No:** TI-1G/25

---

## Praktikum 6.1 — Melacak Proses dan Thread

1. Menampilkan daftar keseluruhan proses yang sedang aktif:
```bash
ps aux
```
Output  :
```bash
root@ubuntuser:/home/rasyiqtaps# ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.3  0.3  22112 13208 ?        Ss   13:47   0:00 /sbin/ini
root           2  0.0  0.0      0     0 ?        S    13:47   0:00 [kthreadd
root           3  0.0  0.0      0     0 ?        S    13:47   0:00 [pool_wor
root           4  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root           5  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root           6  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root           7  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root           8  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root           9  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          10  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          11  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          12  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          13  0.0  0.0      0     0 ?        I    13:47   0:00 [rcu_task
root          14  0.0  0.0      0     0 ?        I    13:47   0:00 [rcu_task
root          15  0.0  0.0      0     0 ?        I    13:47   0:00 [rcu_task
root          16  0.0  0.0      0     0 ?        S    13:47   0:00 [ksoftirq
root          17  0.0  0.0      0     0 ?        I    13:47   0:00 [rcu_pree
root          18  0.0  0.0      0     0 ?        S    13:47   0:00 [migratio
root          19  0.0  0.0      0     0 ?        S    13:47   0:00 [idle_inj
root          20  0.0  0.0      0     0 ?        S    13:47   0:00 [cpuhp/0]
root          21  0.0  0.0      0     0 ?        S    13:47   0:00 [cpuhp/1]
root          22  0.0  0.0      0     0 ?        S    13:47   0:00 [idle_inj
root          23  0.0  0.0      0     0 ?        S    13:47   0:00 [migratio
root          24  0.0  0.0      0     0 ?        S    13:47   0:00 [ksoftirq
root          25  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          26  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          27  0.0  0.0      0     0 ?        S    13:47   0:00 [cpuhp/2]
root          28  0.0  0.0      0     0 ?        S    13:47   0:00 [idle_inj
root          29  0.0  0.0      0     0 ?        S    13:47   0:00 [migratio
root          30  0.0  0.0      0     0 ?        S    13:47   0:00 [ksoftirq
root          31  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          32  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          33  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          34  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          35  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          36  0.0  0.0      0     0 ?        S    13:47   0:00 [kdevtmpf
root          37  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          38  0.0  0.0      0     0 ?        S    13:47   0:00 [kauditd]
root          39  0.0  0.0      0     0 ?        S    13:47   0:00 [khungtas
root          40  0.0  0.0      0     0 ?        S    13:47   0:00 [oom_reap
root          41  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          42  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          43  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          44  0.0  0.0      0     0 ?        S    13:47   0:00 [kcompact
root          45  0.0  0.0      0     0 ?        SN   13:47   0:00 [ksmd]
root          46  0.0  0.0      0     0 ?        SN   13:47   0:00 [khugepag
root          47  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          48  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          49  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          50  0.0  0.0      0     0 ?        S    13:47   0:00 [irq/9-ac
root          51  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          52  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          53  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          54  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          55  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          56  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          57  0.0  0.0      0     0 ?        S    13:47   0:00 [watchdog
root          58  0.1  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          59  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          60  0.3  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          61  0.0  0.0      0     0 ?        S    13:47   0:00 [kswapd0]
root          62  0.0  0.0      0     0 ?        S    13:47   0:00 [ecryptfs
root          63  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          64  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          65  0.0  0.0      0     0 ?        S    13:47   0:00 [scsi_eh_
root          66  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          67  0.0  0.0      0     0 ?        S    13:47   0:00 [scsi_eh_
root          68  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          69  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          70  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          71  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          72  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          73  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          74  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          75  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          76  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          78  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          84  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          85  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          87  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          88  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          89  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          90  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          94  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root          96  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root          97  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root         106  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root         131  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root         158  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root         159  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root         166  0.0  0.0      0     0 ?        S    13:47   0:00 [scsi_eh_
root         167  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root         212  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root         256  0.0  0.0      0     0 ?        S    13:47   0:00 [jbd2/sda
root         257  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root         279  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root         323  0.1  0.4  50468 17420 ?        S<s  13:47   0:00 /usr/lib/
root         343  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root         345  0.2  0.0      0     0 ?        I    13:47   0:00 [kworker/
root         347  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root         356  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root         359  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root         363  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root         379  0.0  0.7 288988 27324 ?        SLsl 13:47   0:00 /sbin/mul
root         389  0.0  0.2  28816  7588 ?        Ss   13:47   0:00 /usr/lib/
root         419  0.0  0.0      0     0 ?        S    13:47   0:00 [psimon]
root         437  0.1  0.0      0     0 ?        I    13:47   0:00 [kworker/
systemd+     443  0.0  0.2  19012  9544 ?        Ss   13:47   0:00 /usr/lib/
systemd+     480  0.0  0.3  21588 13052 ?        Ss   13:47   0:00 /usr/lib/
root         588  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root         610  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root         625  0.0  0.0      0     0 ?        S    13:47   0:00 [irq/18-v
root         626  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
message+     628  0.0  0.1   9892  5656 ?        Ss   13:47   0:00 @dbus-dae
polkitd      647  0.0  0.2 308164  8108 ?        Ssl  13:47   0:00 /usr/lib/
root         660  0.0  0.2  18128  9012 ?        Ss   13:47   0:00 /usr/lib/
root         668  0.0  0.3 468972 13608 ?        Ssl  13:47   0:00 /usr/libe
syslog       704  0.0  0.1 222508  6124 ?        Ssl  13:47   0:00 /usr/sbin
root         731  0.0  0.6 109688 23192 ?        Ssl  13:47   0:00 /usr/bin/
root         754  0.0  0.3 392100 12948 ?        Ssl  13:47   0:00 /usr/sbin
root         764  0.0  0.0      0     0 ?        I    13:47   0:00 [kworker/
root         767  0.0  0.1 293152  3764 ?        Sl   13:47   0:00 /usr/sbin
root         854  0.0  0.0   6824  2908 ?        Ss   13:47   0:00 /usr/sbin
root         861  0.0  0.2  12020  8220 ?        Ss   13:47   0:00 sshd: /us
root         866  0.0  0.1   6956  4924 tty1     Ss   13:47   0:00 /bin/logi
root         980  0.0  0.0      0     0 ?        S    13:47   0:00 [psimon]
rasyiqtaps   982  0.0  0.3  20076 11232 ?        Ss   13:47   0:00 /usr/lib/
rasyiqtaps   983  0.0  0.1  21152  3568 ?        S    13:47   0:00 (sd-pam)
rasyiqtaps   991  0.0  0.1   8656  5652 tty1     S    13:47   0:00 -bash
root         994  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root        1038  0.0  0.0      0     0 ?        I<   13:47   0:00 [kworker/
root        1040  0.0  0.2  16768  7408 tty1     S+   13:47   0:00 sudo su
root        1041  0.0  0.0  16768  2556 pts/0    Ss   13:47   0:00 sudo su
root        1042  0.0  0.1   9376  4492 pts/0    S    13:47   0:00 su
root        1043  0.0  0.1   7604  4496 pts/0    S+   13:47   0:00 bash
root        1050  0.0  0.2  12152  8640 ?        Ss   13:49   0:00 sshd: rasyiqtaps
rasyiqtaps  1052  0.2  0.2  13764  7484 ?        S    13:49   0:00 sshd: rasyiqtaps
rasyiqtaps  1053  0.0  0.1   5644  5080 pts/1    Ss   13:49   0:00 -bash
root        1062  0.1  0.1  13872  6976 pts/1    S+   13:49   0:00 sudo su
root        1063  0.0  0.0  13872  2596 pts/2    Ss   13:49   0:00 sudo su
root        1064  0.0  0.1   9376  4492 pts/2    S    13:49   0:00 su
root        1067  0.2  0.3  20092 11292 ?        Ss   13:49   0:00 /usr/lib/
root        1068  0.0  0.0  21160  3492 ?        S    13:49   0:00 (sd-pam)
root        1073  0.0  0.0      0     0 ?        S    13:49   0:00 [psimon]
root        1080  0.0  0.1   7736  4508 pts/2    S    13:49   0:00 bash
fwupd-r+    1090  2.3  0.8 440908 28568 ?        Ssl  13:51   0:00 /usr/bin/
root        1110  2.7  1.1 551776 41680 ?        Ssl  13:51   0:00 /usr/libe
root        1120  0.5  0.2 314136  9216 ?        Ssl  13:51   0:00 /usr/libe
root        1231 50.0  0.1  11012  4620 pts/2    R+   13:51   0:00 ps aux
```

2. Menampilkan informasi proses lengkap dengan thread masing-masing (bisa diperiksa di kolom LWP atau LightWeight Process ID):
```bash
ps aux -L
```
Output :  
```bash
USER         PID     LWP %CPU NLWP %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1       1  0.1    1  0.3  22112 13208 ?        Ss   13:47   0:00 /sbin/init splash noprompt noshell automatic-ubiquity
root           2       2  0.0    1  0.0      0     0 ?        S    13:47   0:00 [kthreadd]
root           3       3  0.0    1  0.0      0     0 ?        S    13:47   0:00 [pool_workqueue_release]
... (terpotong agar ringkas) ...
root        1120    1123  0.0    4  0.2 314136  9364 ?        Ssl  13:51   0:00 /usr/libexec/upowerd
root        1120    1124  0.0    4  0.2 314136  9364 ?        Ssl  13:51   0:00 /usr/libexec/upowerd
root        1265    1265  200    1  0.1  11012  4640 pts/2    R+   13:57   0:00 ps aux -L
```

3. Mengecek Process ID (PID) dari shell yang saat ini sedang saya operasikan berikut dengan detail prosesnya:
```bash
echo $$
ps -p $$ -f
```
Output :  
```bash
root@ubuntuser:/home/rasyiqtaps# echo $$
1080
root@ubuntuser:/home/rasyiqtaps# ps -p $$ -f
UID          PID    PPID  C STIME TTY          TIME CMD
root        1080    1064  0 13:49 pts/2    00:00:00 bash
```

4. Menampilkan struktur hierarki proses dalam bentuk visual diagram pohon:
```bash
pstree -p
```
Output:  
```bash
systemd(1)─┬─ModemManager(754)─┬─{ModemManager}(782)
           │                   ├─{ModemManager}(784)
           │                   └─{ModemManager}(787)
... (terpotong agar ringkas) ...
           ├─unattended-upgr(731)───{unattended-upgr}(833)
           └─upowerd(1120)─┬─{upowerd}(1122)
                           ├─{upowerd}(1123)
                           └─{upowerd}(1124)
```  

### Latihan 6.1  

Berdasarkan output perintah `ps aux` dan `pstree`, berikut analisis saya:

1. **Berapa total proses yang berjalan? [cite_start]Proses apa yang memiliki PID terkecil?** Untuk mengetahui jumlah keseluruhan proses, saya menghitung baris output dari perintah `ps aux` lalu menguranginya dengan satu baris judul tabel (header)[cite: 492]. Sedangkan untuk PID paling kecil secara umum dipegang oleh angka 1, yang biasanya merujuk pada proses `init` atau `systemd`. [cite_start]Ini masuk akal karena program tersebut adalah proses perdana yang dibangkitkan oleh sistem saat booting[cite: 470].
2. **Jalankan pstree -p dan temukan proses bash Anda. [cite_start]Proses apa yang menjadi induk (PPID) dari bash tersebut?** Dengan mengeksekusi `pstree -p`, saya dapat melacak PID dari program bash saya[cite: 502]. Proses yang terhubung tepat di atas bash tersebut adalah induknya (PPID). Pada kasus saya, jika mengakses sistem melalui protokol jaringan, induknya adalah `sshd`. Jika secara *local*, biasanya induk utamanya berakar dari perintah login atau `systemd`.  
3. **Bandingkan output ps aux dan ps aux -L. [cite_start]Apa perbedaan yang Anda lihat?** Perbedaan paling jelas adalah `ps aux` hanya menampilkan tepat satu baris informasi untuk tiap proses yang berjalan[cite: 492]. [cite_start]Berbeda dengan `ps aux -L` yang mengekspos semua rinciannya hingga tingkat thread (LWP)[cite: 495]. [cite_start]Otomatis, output perintah yang kedua menghasilkan data baris yang jauh lebih padat, karena satu proses yang memiliki thread banyak akan dicetak berulang-ulang[cite: 494].

---

## Praktikum 6.2 — Mengamati Siklus Hidup Proses

1. Menjalankan suatu proses di latar belakang (*background*) lalu menginspeksi statusnya:
```bash
sleep 60 &
ps aux | grep sleep
```
Output :
```bash
root@ubuntuser:/home/rasyiqtaps# sleep 60 &
[1] 1283
root@ubuntuser:/home/rasyiqtaps# ps aux | grep sleep
root        1283  0.0  0.0   5684  2104 pts/2    S    14:07   0:00 sleep 60
root        1285  0.0  0.0   6544  2328 pts/2    S+   14:07   0:00 grep --color=auto sleep
```

2. Memperhatikan perbedaan nilai exit code antara perintah yang berjalan mulus dengan perintah yang sengaja disalahkan:
```bash
ls / tmp
echo " Sukses : $?"
ls / direktori - tidak - ada
echo " Gagal : $?"
```
Output : 
```bash
root@ubuntuser:/home/rasyiqtaps# ls /tmp
snap-private-tmp
systemd-private-e883a04f257d4402b5ca3840da241f74-fwupd.service-NfBXJ8
systemd-private-e883a04f257d4402b5ca3840da241f74-ModemManager.service-uGfyK9
systemd-private-e883a04f257d4402b5ca3840da241f74-polkit.service-EFsXVL
systemd-private-e883a04f257d4402b5ca3840da241f74-systemd-logind.service-WLp0nl
systemd-private-e883a04f257d4402b5ca3840da241f74-systemd-resolved.service-cUPjRP
systemd-private-e883a04f257d4402b5ca3840da241f74-upower.service-iXFjhA
root@ubuntuser:/home/rasyiqtaps# echo "Sukses: $?"
Sukses: 0
[1]+  Done                    sleep 60
root@ubuntuser:/home/rasyiqtaps# ls /direktori-tidak-ada
ls: cannot access '/direktori-tidak-ada': No such file or directory
root@ubuntuser:/home/rasyiqtaps# echo "Gagal: $?"
Gagal: 2
```  

### Latihan 6.2  

1. **Jalankan sleep 120 & dan amati kolom STAT pada ps aux. Kondisi apa yang ditampilkan? [cite_start]Mengapa proses sleep berada di kondisi tersebut?** Terlihat pada kolom STAT ada huruf S yang merepresentasikan kondisi *Sleeping*[cite: 519]. [cite_start]Penjelasannya cukup logis: perintah `sleep` sedang mem-pause dirinya sendiri menunggu batas waktu habis tanpa menyedot komputasi CPU[cite: 519]. [cite_start]Kebanyakan proses yang berjalan santai di *background* memang akan menampilkan status S[cite: 519].
2. **Jalankan beberapa perintah yang berhasil dan yang gagal, lalu catat exit code masing-masing. [cite_start]Pola apa yang Anda temukan?** Berdasarkan tes yang saya lakukan, eksekusi kode yang lancar tanpa *error* akan mendongkrak variabel `$?` menjadi angka `0`[cite: 533]. [cite_start]Sedangkan bila ada kegagalan, `$?` memuntahkan angka non-nol seperti 1, 2, atau angka kembalian sistem lainnya[cite: 533, 537]. [cite_start]Polanya sangat baku: `0` berarti *clear* (sukses), sisa angkanya merepresentasikan indikasi kerusakan atau *error*[cite: 533].

---

## Praktikum 6.3 — Mengatur Prioritas Proses

1. Menjalankan sebuah proses baru sembari menurunkan bobot prioritasnya (memberi nilai nice lebih besar):
```bash
nice -n 10 sleep 300 &
```

Output :  
```bash
[1] 1768
```

2. Mengecek ulang validitas angka nice proses tadi melalui kolom NI:
```bash
ps aux | grep sleep
```

Output :  
```bash
root        1768  0.0  0.0   5684  2104 pts/2    SN   14:19   0:00 sleep 300
root        1774  0.0  0.0   6544  2332 pts/2    S+   14:20   0:00 grep --color=auto sleep
```

3. Mengganti angka nice pada proses yang sudah keburu aktif:
```bash
renice -n 15 -p <PID >
ps -p <PID > -o pid , ni , cmd
```

Output :  
```bash
root@ubuntuser:/home/rasyiqtaps# renice -n 15 -p 1794
1794 (process ID) old priority 10, new priority 15
root@ubuntuser:/home/rasyiqtaps# ps -p 1794 -o pid,ni,cmd
    PID  NI CMD
   1794  15 sleep 300
```

4. Membersihkan atau menyingkirkan proses yang digunakan selama percobaan ini:
```bash
kill %1
```

Output :  
```bash
[1]+  Terminated              nice -n 10 sleep 300
```

### Latihan 6.3
1. **Jalankan nice -n 5 sleep 200 & dan verifikasi nilai NI-nya dengan ps.** Berikut adalah eksekusi perintahnya dan konfirmasi kolom NI dari saya:
```bash
root@ubuntuser:/home/rasyiqtaps# nice -n 5 sleep 200 &
[1] 1808
root@ubuntuser:/home/rasyiqtaps# ps -o pid,ni,cmd
    PID  NI CMD
   1063   0 sudo su
   1064   0 su
   1080   0 bash
   1808   5 sleep 200
   1809   0 ps -o pid,ni,cmd
```
2. **Ubah nilai nice menjadi 10 menggunakan renice, lalu verifikasi kembali.** Memakai *command* `renice` guna mereset skala prioritas menjadi angka 10:
```bash
root@ubuntuser:/home/rasyiqtaps# renice -n 10 -p 1808
1808 (process ID) old priority 5, new priority 10
root@ubuntuser:/home/rasyiqtaps# ps -o pid,ni,cmd
    PID  NI CMD
   1063   0 sudo su
   1064   0 su
   1080   0 bash
   1808  10 sleep 200
   1811   0 ps -o pid,ni,cmd
```
3. **Coba ubah nilai nice menjadi -5 tanpa sudo. Apa yang terjadi? Mengapa Linux membatasi hal ini untuk user biasa?** Mesin akan membalas dengan teguran *permission denied*. Kenapa begitu? Karena angka negatif mencerminkan prioritas yang super tinggi (mendahului antrean normal sistem). [cite_start]Secara logika dasar sekuritas, hak merampas antrean prioritas komputasi (-20 hingga nilai minus lainnya) secara eksklusif hanya diberikan pada sang administrator yaitu akun root[cite: 565]. [cite_start]Pengguna awam memang secara kodrat dikunci agar tak dapat melakukan hal tersebut[cite: 566].

---

## Praktikum 6.4 — Mengirim Sinyal ke Proses

1. Menciptakan sekumpulan proses tidur sebagai subjek eksperimen:
```bash
sleep 500 &
sleep 600 &
sleep 700 &
ps aux | grep -v grep | grep sleep
```

Output :  
```bash
root@ubuntuser:/home/rasyiqtaps# sleep 500 &
[2] 1822
[1]   Done                    nice -n 5 sleep 200
root@ubuntuser:/home/rasyiqtaps# sleep 600 &
[3] 1826
root@ubuntuser:/home/rasyiqtaps# sleep 700 &
[4] 1827
root@ubuntuser:/home/rasyiqtaps# ps aux | grep -v grep | grep sleep
root        1822  0.0  0.0   5684  2104 pts/2    S    14:44   0:00 sleep 500
root        1826  0.0  0.0   5684  2104 pts/2    S    14:45   0:00 sleep 600
root        1827  0.0  0.0   5684  2108 pts/2    S    14:45   0:00 sleep 700
```

2. Memutus satu nyawa proses dengan metode SIGTERM dan melihat hasilnya:
```bash
kill <PID - sleep -500 >
ps aux | grep -v grep | grep sleep
```

Output :  
```bash
root@ubuntuser:/home/rasyiqtaps# kill 1822
root@ubuntuser:/home/rasyiqtaps# ps aux | grep -v grep | grep sleep
root        1826  0.0  0.0   5684  2104 pts/2    S    14:45   0:00 sleep 600
root        1827  0.0  0.0   5684  2108 pts/2    S    14:45   0:00 sleep 700
[2]   Terminated              sleep 500
```

3. Menerapkan skenario pause-play menggunakan sinyal SIGSTOP dan SIGCONT:
```bash
kill - SIGSTOP <PID - sleep -600 >
ps aux | grep sleep # amati kolom STAT : berubah menjadi T
kill - SIGCONT <PID - sleep -600 >
ps aux | grep sleep # STAT kembali ke S
```

Output :  
```bash
root@ubuntuser:/home/rasyiqtaps# kill -SIGSTOP 1826
root@ubuntuser:/home/rasyiqtaps# ps aux | grep sleep
root        1826  0.0  0.0   5684  2104 pts/2    T    14:45   0:00 sleep 600
root        1827  0.0  0.0   5684  2108 pts/2    S    14:45   0:00 sleep 700
root        1838  0.0  0.0   6544  2328 pts/2    S+   14:48   0:00 grep --color=auto sleep

[3]+  Stopped                 sleep 600
root@ubuntuser:/home/rasyiqtaps# kill -SIGCONT 1826
root@ubuntuser:/home/rasyiqtaps# ps aux | grep sleep
root        1826  0.0  0.0   5684  2104 pts/2    S    14:45   0:00 sleep 600
root        1827  0.0  0.0   5684  2108 pts/2    S    14:45   0:00 sleep 700
root        1840  0.0  0.0   6544  2328 pts/2    S+   14:48   0:00 grep --color=auto sleep
```

4. Menyapu bersih seluruh proses dummy ini sekaligus:
```bash
pkill sleep
```

Output :  
```bash
root@ubuntuser:/home/rasyiqtaps# pkill sleep
root@ubuntuser:/home/rasyiqtaps# ps aux | grep sleep
root        1844  0.0  0.0   6544  2332 pts/2    S+   14:49   0:00 grep --color=auto sleep
[3]-  Terminated              sleep 600
[4]+  Terminated              sleep 700
```

### Latihan 6.4
1. **Jalankan sleep 400 &, kirim SIGSTOP, dan amati perubahan kolom STAT. [cite_start]Kondisi apa yang muncul?** Pada kolom STAT, huruf yang tercetak berubah menjadi T (kependekan dari *Stopped*)[cite: 519]. [cite_start]Pada status ini, siklus proses dipaksa berhenti beroperasi sementara waktu, otomatis CPU tidak akan mengalokasikan tenaga kerjanya untuk urusan tersebut[cite: 654].  
```bash
root@ubuntuser:/home/rasyiqtaps# sleep 400 &
[1] 1857
root@ubuntuser:/home/rasyiqtaps# kill -SIGSTOP 1857
root@ubuntuser:/home/rasyiqtaps# ps aux | grep sleep
root        1857  0.0  0.0   5684  2104 pts/2    T    14:54   0:00 sleep 400
root        1859  0.0  0.0   6544  2332 pts/2    S+   14:54   0:00 grep --color=auto sleep

[1]+  Stopped                 sleep 400
```
2. [cite_start]**Kirim SIGCONT dan verifikasi proses kembali berjalan.** Begitu saya ketuk dengan SIGCONT, inisial STAT kembali terisi huruf S (*Sleeping*), yang mana menegaskan bahwa rutinitas background sudah berdetak normal layaknya semula[cite: 655].
```bash
root@ubuntuser:/home/rasyiqtaps# kill -SIGCONT 1857
root@ubuntuser:/home/rasyiqtaps# ps aux | grep sleep
root        1857  0.0  0.0   5684  2104 pts/2    S    14:54   0:00 sleep 400
root        1861  0.0  0.0   6544  2328 pts/2    S+   14:54   0:00 grep --color=auto sleep
```
3. **Hentikan proses dengan SIGTERM lalu verifikasi sudah tidak ada. [cite_start]Kapan Anda memilih SIGKILL daripada SIGTERM?** Secara *best practice*, saya selalu melemparkan `SIGTERM` dahulu sebagai bentuk interupsi sopan agar program sanggup mengamankan memori datanya dan tertutup secara rapi tanpa cacat[cite: 625]. [cite_start]Pendekatan brutal ala `SIGKILL` semata-mata saya jadikan opsi final manakala ada program *bandel* yang membeku dan tidak gubris saat diberikan `SIGTERM`[cite: 626].
```bash
root@ubuntuser:/home/rasyiqtaps# kill 1857
root@ubuntuser:/home/rasyiqtaps# ps aux | grep -v grep | grep sleep
[1]+  Terminated              sleep 400
```

---

## Praktikum 6.5 — Manajemen Job Foreground dan Background

1. Menyuntikkan tiga perintah *job* untuk bekerja sunyi di latar belakang:
```bash
sleep 200 &
sleep 300 &
sleep 400 &
jobs
```

Output : 
```bash
root@ubuntuser:/home/rasyiqtaps# sleep 200 &
[1] 1874
root@ubuntuser:/home/rasyiqtaps# sleep 300 &
[2] 1875
root@ubuntuser:/home/rasyiqtaps# sleep 400 &
[3] 1876
root@ubuntuser:/home/rasyiqtaps# jobs
[1]   Running                 sleep 200 &
[2]-  Running                 sleep 300 &
[3]+  Running                 sleep 400 &
```

2. Menarik salah satu job dari baris belakang ke layar pandang (foreground), mengejarnya, lantas melemparnya lagi ke asalnya (background):
```bash
fg %1
# Tekan Ctrl +Z untuk menjeda
bg %1
jobs
```

Output : 
```bash
root@ubuntuser:/home/rasyiqtaps# fg %1
sleep 200
^Z
[1]+  Stopped                 sleep 200
root@ubuntuser:/home/rasyiqtaps# bg %1
[1]+ sleep 200 &
root@ubuntuser:/home/rasyiqtaps# jobs
[1]   Running                 sleep 200 &
[2]-  Running                 sleep 300 &
[3]+  Running                 sleep 400 &
```

3. Mengakhiri perjalanan semua list *job* yang ada:
```bash
kill %1 %2 %3
jobs
```

Output : 
```bash
root@ubuntuser:/home/rasyiqtaps# kill %1 %2 %3
[1]   Terminated              sleep 200
[2]-  Terminated              sleep 300
[3]+  Terminated              sleep 400
root@ubuntuser:/home/rasyiqtaps# jobs
```

### Latihan 6.5
1. **Jalankan top di foreground. [cite_start]Apa yang terjadi di terminal?** Saat itu juga, terminal saya dimonopoli oleh dashboard `top`[cite: 660, 680]. [cite_start]Akibatnya sangat jelas: saya disandera dan tidak dimungkinkan mengetik satu baris command apapun hingga sudi mematikan *interface* `top` tersebut[cite: 660].
```bash
top - 15:07:20 up  1:20,  2 users,  load average: 0.00, 0.01, 0.00
Tasks: 134 total,   1 running, 133 sleeping,   0 stopped,   0 zombie
... (terpotong agar ringkas) ...
```
2. **Tekan Ctrl+Z dan cek statusnya dengan jobs. Kondisi apa yang ditampilkan?** Log informasi menunjukkan status barunya adalah `Stopped`. [cite_start]Hal ini memvalidasi bahwa aplikasi penampil statistik itu tengah tersendat di tengah jalan akibat injeksi tombol berbau `SIGSTOP`[cite: 665].
```bash
[1]+  Stopped                 top
root@ubuntuser:/home/rasyiqtaps# jobs
[1]+  Stopped                 top
```
3. **Pindahkan ke background dengan bg. Apakah top dapat berjalan dengan baik di background? Mengapa?** Meskipun mesin memproses komputasi `top` di layar bayangan, aplikasi ini ngotot mengirimkan rilis visual statistiknya bertubi-tubi ke antarmuka terminal. Pada akhirnya ia hanya mengotori ruang terminal saya yang sedang aktif. Intinya, software yang bertipe interaktif (seperti layar monitor real-time) sama sekali tak cocok dikondisikan di area background.
```bash
root@ubuntuser:/home/rasyiqtaps# bg %1
[1]+ top &

[1]+  Stopped                 top
```
4. [cite_start]**Kembalikan ke foreground dengan fg, lalu keluar dengan q.** Saya menormalisasikannya kembali ke layar *foreground* lantas dengan lancar menyingkirkannya memakai tuts huruf `q`[cite: 683].
```bash
top - 15:10:01 up  1:23,  2 users,  load average: 0.00, 0.00, 0.00
Tasks: 134 total,   1 running, 133 sleeping,   0 stopped,   0 zombie
... (terpotong agar ringkas) ...
```

---

## Praktikum 6.6 — Pemantauan Proses

1. Menjaring aplikasi dengan pemakaian daya cerna prosesor serta RAM yang paling memberatkan:
```bash
ps aux -- sort = -% cpu | head -10
ps aux -- sort = -% mem | head -10
```

Output :  
```bash
root@ubuntuser:/home/rasyiqtaps# ps aux --sort=-%cpu | head -10
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root        1887  300  0.1  10884  4624 pts/2    R+   15:13   0:00 ps aux --sort=-%cpu
... (terpotong agar ringkas) ...
root@ubuntuser:/home/rasyiqtaps# ps aux --sort=-%mem | head -10
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root        1110  0.0  1.2 685756 43676 ?        Ssl  13:51   0:03 /usr/libexec/fwupd/fwupd
... (terpotong agar ringkas) ...
```

2. Mengeksekusi *command* `top` sekalian berkenalan dengan kapabilitas tombol pintasannya:
```bash
top
# Tekan M, P, 1 , u secara bergantian
# Tekan q untuk keluar
```

Output :  
```bash
root@ubuntuser:/home/rasyiqtaps# top
top - 15:15:22 up  1:28,  2 users,  load average: 0.00, 0.00, 0.00
Tasks: 136 total,   2 running, 134 sleeping,   0 stopped,   0 zombie
%Cpu0  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.
... (terpotong agar ringkas) ...
```

3. Mengambil file binari pemantau bernama `htop` lalu menampilkannya:
```bash
sudo apt install -y htop
htop
# Tekan F6 untuk pilih kolom pengurutan
# Tekan F10 atau q untuk keluar
```

Output :  
```bash
root@ubuntuser:/home/rasyiqtaps# sudo apt install -y htop
Reading package lists... Done
... (terpotong agar ringkas) ...
root@ubuntuser:/home/rasyiqtaps# htop
```

### Latihan 6.6  

1. **Gunakan ps aux –sort=%mem untuk menemukan proses yang menggunakan memori paling banyak di VM Anda. [cite_start]Proses apa itu?** Dari rekapitulasi data menggunakan *command* `ps aux --sort=-%mem`, bisa dipertanggungjawabkan bahwa aplikasi penguras *memory space* nomor satu di lingkungan virtual saya jatuh kepada `fwupd`[cite: 690, 691].  
```bash
root@ubuntuser:/home/rasyiqtaps# ps aux --sort=-%mem | head -5
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root        1110  0.0  1.2 685756 43676 ?        Ssl  13:51   0:03 /usr/libexec/fwupd/fwupd
... (terpotong agar ringkas) ...
```
2. **Di dalam top, tekan 1 . Apa yang berubah pada tampilan? [cite_start]Mengapa informasi ini berguna?** Layar pemantau kini menyebar statistik bebannya dengan menunjukkan rasio konsumsi untuk masing-masing *core processor* (misal %Cpu0, %Cpu1) yang ada[cite: 683]. [cite_start]Info sedetail ini krusial saat kita menganalisis sehat atau tidaknya *load-balancing* mesin; kita bisa menilai apakah sistem menderita gara-gara satu inti terlalu berat (*bottleneck*) atau pekerjaan sudah dipikul dengan adil[cite: 683].
3. **Di dalam htop, navigasikan ke proses sshd menggunakan tombol panah. [cite_start]Tekan F9 dan amati opsi sinyal yang tersedia.** Pasca penekanan tombol, sebuah *interface* khusus bakal pop-up memajang segala tipe sinyal interupsi yang di-support mesin (semisal `SIGTERM`, `SIGKILL`, hingga `SIGHUP`)[cite: 686]. [cite_start]Fitur visual ini sungguh membantu pengguna semacam saya agar dapat membidik perintah eksekusi yang pas buat si target, tanpa kewajiban menghafal angka kode teknis sinyalnya[cite: 686].

---

## 1.8 Latihan

### Latihan 6.A: Eksplorasi Proses Sistem

1. [cite_start]**Jalankan ps aux –forest dan temukan proses dengan PID 1. Apa nama dan fungsi proses tersebut dalam sistem Linux modern?** Sistem Linux merujuk proses ini dengan nama resmi `init` (sementara distro anyar lebih suka menyebutnya `systemd`)[cite: 704]. Fungsinya jelas, ia adalah pionir atau program yang pertama kali dikerjakan kernel saat menyala. Otomatis, secara teknis `systemd` / `init` menahbiskan dirinya menjadi bapak pendiri atau induk dari semua rangkaian proses yang berkembang pada ekosistem operasi kita.  
```bash
root           1  0.4  0.3  21992 13284 ?        Ss   05:20   0:03 /sbin/ini
```
2. **Hitung berapa proses yang dimiliki oleh user root dan berapa yang dimiliki oleh user Anda. Mengapa root memiliki lebih banyak proses?** Dari kalkulasi visual di daftar, proses di bawah bendera *root* bertengger mendominasi di angka sekitar 92 biji (termasuk modul mesin seperti `[kthreadd]`). Berbanding jauh, user lokal saya (`rasyiqtaps`) terpantau cuma menjepit kurang lebih 4 identitas proses. Kenapa disparitas ini terjadi? [cite_start]Ini logis karena user *root*-lah sang mandor yang dipaksa memanggul berbagai *services* krusial, *daemon*, dan driver kernel supaya keseluruhan OS bisa terus hidup dan diakses *user* biasa[cite: 706].  
3. [cite_start]**Temukan semua proses yang berada dalam kondisi S. Mengapa sebagian besar proses di sistem berada dalam kondisi ini?** Cukup amati bagian STAT yang melabelkan status awal huruf S (dapat berbentuk `Ss`, `S+`, dsb)[cite: 519]. Contoh kasusnya adalah `PID 1 (/sbin/init)` atau `PID 2 ([kthreadd])`. [cite_start]Populasi mereka begitu meledak lantaran status S melambangkan *sleeping* di mana aplikasi-aplikasi ini pada dasarnya sedang rehat sembari menunggu ada instruksi, kedipan *timer*, *interrupt I/O*, atau sebuah kondisi spesifik sebelum akhirnya bangun merampas jatah prosesor[cite: 515].

### Latihan 6.B: Simulasi Manajemen Job

1. **Jalankan tiga perintah sleep dengan durasi 100, 200, dan 300 detik di background. Verifikasi ketiganya dengan jobs.** Saya menempatkan ketiga timer di belakang terminal untuk verifikasi riwayat kerjanya:
```bash
root@ubuntuser:/home/rasyiqtaps# sleep 100 &
[1] 1165
root@ubuntuser:/home/rasyiqtaps# sleep 200 &
[2] 1166
root@ubuntuser:/home/rasyiqtaps# sleep 300 &
[3] 1168
root@ubuntuser:/home/rasyiqtaps# jobs
[1]   Done                    sleep 100
[2]-  Running                 sleep 200 &
[3]+  Running                 sleep 300 &
```
2. **Bawa job kedua ke foreground, jeda dengan Ctrl+Z , lalu kembalikan ke background dengan bg.** Saya memerintahkan satu job untuk keluar dari persembunyian ke latar depan, mem-pausenya sekilas, lalu melemparnya balik:
```bash
root@ubuntuser:/home/rasyiqtaps# fg %2
sleep 200
^Z
[2]+  Stopped                 sleep 200
root@ubuntuser:/home/rasyiqtaps# bg %2
[2]+ sleep 200 &
```
3. **Hentikan job pertama dengan kill %1. Tampilkan kembali daftar job. Berapa job yang tersisa?** Melalui terminasi pada indeks nomor satu, kini hanya tertinggal sedikit sisa antrean job:
```bash
root@ubuntuser:/home/rasyiqtaps# kill %1
bash: kill: %1: no such job
[2]-  Done                    sleep 200
root@ubuntuser:/home/rasyiqtaps# jobs
[3]+  Running                 sleep 300 &
```

### Latihan 6.C: Prioritas dan Sinyal

1. **Jalankan dua proses sleep: satu dengan nice +5 dan satu dengan nice +15. Verifikasi nilai NI keduanya dengan ps.** Dengan melempar dua skrip uji coba yang ber-nice beda bobot, saya bisa membuktikannya lewat pengecekan manual:
```bash
root@ubuntuser:/home/rasyiqtaps# nice -n 5 sleep 200 &
[1] 1253
root@ubuntuser:/home/rasyiqtaps# nice -n 15 sleep 200 &
[2] 1254
root@ubuntuser:/home/rasyiqtaps# ps -o pid,ni,cmd | grep sleep
   1253   5 sleep 200
   1254  15 sleep 200
   1256   0 grep --color=auto sleep
```
2. **Gunakan renice untuk mengubah nice proses pertama menjadi +10. [cite_start]Proses mana yang kini lebih diprioritaskan scheduler?** Aturan dasarnya, penempatan CPU berfokus menyanjung siapa yang poin *nice*-nya lebih langsing[cite: 562]. Otomatis sehabis pengubahan, program nomor urut pertama yang kini terpatok poin `NI = 10` bakalan memanen atensi operasi paling mumpuni ketimbang partnernya si pemegang `NI 15`.
```bash
root@ubuntuser:/home/rasyiqtaps# renice -n 10 -p 1253
1253 (process ID) old priority 5, new priority 10
root@ubuntuser:/home/rasyiqtaps# ps -o pid,ni,cmd | grep sleep
   1253  10 sleep 200
   1254  15 sleep 200
   1259   0 grep --color=auto sleep
``` 
3. **Kirim SIGSTOP ke salah satu proses, verifikasi kondisi T-nya, lalu kirim SIGCONT. Akhiri semua proses percobaan dengan pkill sleep.** Saya mengirim status freeze pada aplikasi dengan `SIGSTOP` lalu meremotenya supaya lanjut berjalan via `SIGCONT`. Pada *ending*-nya, saya ratakan semuanya memakai instruksi pembunuh massal bernama `pkill`:
```bash
root@ubuntuser:/home/rasyiqtaps# kill -SIGSTOP 1253
root@ubuntuser:/home/rasyiqtaps# ps -o pid,stat,cmd | grep sleep
   1253 TN   sleep 200
   1254 SN   sleep 200
   1266 S+   grep --color=auto sleep

[1]+  Stopped                 nice -n 5 sleep 200
root@ubuntuser:/home/rasyiqtaps# kill -SIGCONT 1253
root@ubuntuser:/home/rasyiqtaps# ps -o pid,stat,cmd | grep sleep
   1253 SN   sleep 200
   1254 SN   sleep 200
   1268 S+   grep --color=auto sleep
root@ubuntuser:/home/rasyiqtaps# pkill sleep
[1]-  Terminated              nice -n 5 sleep 200
[2]+  Terminated              nice -n 15 sleep 200
```