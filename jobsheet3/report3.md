# Laporan Pertemuan ke-3 Sistem Operasi

**Tanggal:** 2 maret 2026  
**Disusun Oleh:** Rasyiq Satrio m  
**NIM:** 254107020079  
**Kelas/No:** TI-1G/25  

---

## Latihan 3.1: Log Management
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

### Latihan 3.2: Pipeline User Extraction
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

