# Laporan Pertemuan ke-3 Sistem Operasi

**Tanggal:** 2 maret 2026  
**Disusun Oleh:** Rasyiq Satrio m  
**NIM:** 254107020079  
**Kelas/No:** TI-1G/25  

---

## Latihan 3.1
    Buatlah script yang:
    1. Menampilkan daftar 10 file terbesar di direktori /var/log/
    2. Menyimpan hasilnya ke file large-logs.txt
    3. Menampilkan output juga di terminal menggunakan tee
    4. Menangani error dengan redirect ke error.log

* **1. Mengambil Daftar File dan Ukurannya:**

    >> ls -lh /var/log/

* **2. Menangani Error (Redirection):**

    >> ls -lh /var/log/ 2> error.log

* **3. Mengurutkan dan Mengambil 10 Terbesar:**

    >> ls -lh /var/log/ 2> error.log | sort -k5 -rh | head -10

* **4. Menggunakan tee untuk Output Ganda:**

    >> ... | tee large-logs.txt

* **Hasil Output:**
    `ls -lh /var/log/ 2> error.log | sort -k5 -rh | head -10 | tee large-logs.txt
    -rw-r-----  1 syslog    adm             1.1M Mar  3 09:20 syslog
    -rw-r--r--  1 root      root            703K Feb 24 15:38 dpkg.log.1
    -rw-r-----  1 syslog    adm             582K Mar  3 09:15 kern.log
    -rw-r-----  1 syslog    adm             384K Mar  1 12:26 syslog.1
    -rw-rw-r--  1 root      utmp            286K Mar  3 09:11 lastlog
    -rw-r-----  1 syslog    adm             188K Feb 24 15:41 kern.log.1
    -rw-------  1 root      root             84K Mar  3 09:10 boot.log.1
    -rw-r-----  1 syslog    adm              75K Feb 23 07:39 cloud-init.log
    -rw-r--r--  1 root      root             60K Feb 10 00:17 bootstrap.log
    -rw-r-----  1 root      adm              50K Mar  3 09:10 dmesg`

---

## Latihan 3.2
    Buat pipeline yang:
    1. Membaca /etc/passwd
    2. Mengekstrak username (kolom pertama)
    3. Mengurutkan alfabetis
    4. Menyimpan ke file sorted-users.txt
    Hint: Gunakan cut, sort, dan operator redirect.

* **1. Membaca /etc/passwd:**

    >> cat /etc/passwd

* **2. Mengekstrak username (kolom pertama):**

    >> cut -d ':' -f 1 /etc/passwd

* **3. Mengurutkan secara alfabetis:**

    >> cut -d ':' -f 1 /etc/passwd | sort

* **4. Menyimpan ke file sorted-users.txt**

    >> cut -d ':' -f 1 /etc/passwd | sort > sorted-users.txt

* **Hasil Output:**
    `_apt
    backup
    bin
    daemon
    dhcpcd
    fwupd-refresh
    games
    irc
    landscape
    list
    lp
    mail
    man
    messagebus
    news
    nobody
    polkitd
    pollinate
    proxy
    rasyiqtaps
    root
    sshd
    sync
    sys
    syslog
    systemd-network
    systemd-resolve
    systemd-timesync
    tcpdump
    tss
    usbmux
    uucp
    uuidd
    www-data`

---

## Latihan 3.3
    Tulis script monitoring yang:
    1. Mencatat penggunaan CPU dan memory setiap 5 detik
    2. Menyimpan log dengan timestamp
    3. Berjalan selama 1 menit (12 iterasi)
    4. Output ditampilkan di terminal DAN disimpan ke file

* **1. Membuat File Script:**

    >> nano monitoring.sh

* **2. Menulis Isi Script**

    >>`#!/bin/bash
for i in {1..12}
do
    # 1 & 2. Mencatat timestamp dan penggunaan resource
    echo "--- Log pada: $(date) ---"
    
    # Menampilkan penggunaan Memory (baris Mem saja)
    free -h | grep Mem
    
    # Menampilkan load CPU (mengambil bagian load average)
    uptime | awk -F'load average:' '{ print "CPU Load:" $2 }'
    
    echo "-------------------------------------"
    
    # 3. Jeda selama 5 detik
    sleep 5
done `

* **3. Menyimpan dan Menutup Editor:**

    >> (Ctrl + O) + enter lalu (Ctrl + X)

* **4. Memberikan Izin Eksekusi:**

    >> chmod +x monitoring.sh

* **5. Menjalankan Script dengan Output Ganda (tee):**

    >> ./monitoring.sh | tee monitor.log

* **Hasil output**
    `--- Log pada: Tue Mar  3 09:40:01 AM UTC 2026 ---
    Mem:           1.9Gi       338Mi       1.5Gi       1.0Mi       286Mi       1.6Gi
    CPU Load: 0.00, 0.00, 0.00
    -------------------------------------
    --- Log pada: Tue Mar  3 09:40:06 AM UTC 2026 ---
    Mem:           1.9Gi       338Mi       1.5Gi       1.0Mi       286Mi       1.6Gi
    CPU Load: 0.00, 0.00, 0.00
    -------------------------------------
    --- Log pada: Tue Mar  3 09:40:11 AM UTC 2026 ---
    Mem:           1.9Gi       338Mi       1.5Gi       1.0Mi       286Mi       1.6Gi
    CPU Load: 0.00, 0.00, 0.00
    -------------------------------------`

---

## Latihan 3.4
    Buat perintah yang:
    1. Mencari semua file .conf di sistem
    2. Membuang pesan "Permission denied"
    3. Menghitung jumlah file yang ditemukan
    4. Menyimpan daftar path lengkap ke file
    
* **1. Mencari semua file .conf di sistem:**

    >> find / -name "*.conf"

* **2. Membuang pesan "Permission denied":**

    >> find / -name "*.conf" 2>/dev/null

* **3 & 4. Menyimpan daftar path DAN Menghitung jumlahnya:**

    >> find / -name "*.conf" 2>/dev/null | tee daftar_config.txt | wc -l

* **Hasil output**
    `397`

---

## Latihan 3.5
    Implementasikan script backup yang:
    1. Menggunakan tar untuk backup direktori
    2. Menampilkan progress dengan tee
    3. Mencatat stdout ke backup-success.log
    4. Mencatat stderr ke backup-error.log
    5. Menambahkan timestamp di setiap log entry

* **1. Menentukan Target Backup:**

    >> Misalkan kita ingin mem-backup folder `Documents` milik kita menjadi sebuah file bernama `my_backup.tar`.

* **2. Menggunakan tar dengan Progress (Verbose):**

    >> Opsi -v (verbose) pada tar akan mengeluarkan daftar file yang sedang diproses ke Standard Output (stdout).

* **3. Memisahkan Output (Success vs Error):**

    >> tar -cvf backup_hari_ini.tar ~/tugas 2> backup-error.log | tee backup-success.log

* **Hasil Output**
    `-rw-rw-r-- 1 rasyiqtaps rasyiqtaps 168 Mar  3 09:51 backup-error.log
    -rw-rw-r-- 1 rasyiqtaps rasyiqtaps   0 Mar  3 09:51 backup-success.log`