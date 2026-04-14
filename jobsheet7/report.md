# Laporan Praktikum 7

<h4\>Nama : Rasyiq Satrio Musthafa\</h4\>
<h4\>Nim : 254107020073</h4\>
<h4\>Absen : 25</h4\>
<h4\>Kelas : TI-1G</h4\>

-----

## Tugas Praktikum 1 — Toolkit Bash Administrator Pribadi

### Langkah 1: Persiapan Ruang Kerja (*Workspace*)

**Perintah Eksekusi:**

```bash
mkdir -p ~/praktikum-os/week07-bash/{bin,backup,logs,sampel,ruang-nama}

cd ~/praktikum-os/week07-bash

touch sample-app.conf
touch logs/app-{01,02,03}.log
touch sampel/catatan-{a,b}.txt
touch sampel/backup-{01,02}.tar
touch "ruang-nama/laporan server april.txt"
touch "ruang-nama/backup [mingguan] server.conf"

echo "✅ Workspace siap"
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:/home/rasyiqtaps# mkdir -p ~/praktikum-os/week07-bash/{bin,backup,logs,sampel,ruang-nama}
root@ubuntuser:/home/rasyiqtaps# cd ~/praktikum-os/week07-bash
root@ubuntuser:~/praktikum-os/week07-bash# touch sample-app.conf
root@ubuntuser:~/praktikum-os/week07-bash# touch logs/app-{01,02,03}.log
root@ubuntuser:~/praktikum-os/week07-bash# touch sampel/catatan-{a,b}.txt
root@ubuntuser:~/praktikum-os/week07-bash# touch sampel/backup-{01,02}.tar
root@ubuntuser:~/praktikum-os/week07-bash# touch "ruang-nama/laporan server april.txt"
root@ubuntuser:~/praktikum-os/week07-bash# touch "ruang-nama/backup [mingguan] server.conf"
root@ubuntuser:~/praktikum-os/week07-bash# echo "✅ Workspace siap"
✅ Workspace siap
```

### Langkah 2: Pencadangan dan Pembaruan Konfigurasi pada `.bashrc`

**Perintah Eksekusi:**

```bash
cp ~/.bashrc ~/.bashrc.bak-praktikum-$(date +%Y%m%d)

cat <<'EOF' >> ~/.bashrc

# ===== PRAKTIKUM BASH SHELL - TUGAS 1 =====
# 1. Menambahkan direktori bin pribadi ke dalam PATH
export PATH="$HOME/praktikum-os/week07-bash/bin:$PATH"

# 2. Pembuatan alias untuk mempercepat rutinitas
alias ll='ls -lah --color=auto'
alias hist10='history | tail -10'
alias dfh='df -h'
alias freeh='free -h'

# 3. Fungsi pencadangan (backup) otomatis dengan timestamp
backup_file() {
    if [ $# -ne 1 ]; then
        echo "Format: backup_file <nama_file>"
        return 1
    fi
    local src="$1"
    local dst="$HOME/praktikum-os/week07-bash/backup"
    if [ ! -f "$src" ]; then
        echo "Kesalahan: File '$src' tidak ditemukan pada direktori!"
        return 2
    fi
    mkdir -p "$dst"
    local timestamp=$(date +%Y%m%d_%H%M%S)
    cp "$src" "$dst/$(basename "$src").$timestamp.bak"
    echo "✅ Proses pencadangan berhasil: $dst/$(basename "$src").$timestamp.bak"
}
# ===== END PRAKTIKUM =====

EOF

source ~/.bashrc

echo "✅ Konfigurasi baru telah diterapkan ke .bashrc"
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# cp ~/.bashrc ~/.bashrc.bak-praktikum-$(date +%Y%m%d)
root@ubuntuser:~/praktikum-os/week07-bash# cat <<'EOF' >> ~/.bashrc
> # ===== PRAKTIKUM BASH SHELL - TUGAS 1 =====
# 1. Menambahkan direktori bin pribadi ke dalam PATH
export PATH="$HOME/praktikum-os/week07-bash/bin:$PATH"

# 2. Pembuatan alias untuk mempercepat rutinitas
alias ll='ls -lah --color=auto'
alias hist10='history | tail -10'
alias dfh='df -h'
alias freeh='free -h'

# 3. Fungsi pencadangan (backup) otomatis dengan timestamp
backup_file() {
    if [ $# -ne 1 ]; then
        echo "Format: backup_file <nama_file>"
        return 1
    fi
    local src="$1"
    local dst="$HOME/praktikum-os/week07-bash/backup"
    if [ ! -f "$src" ]; then
        echo "Kesalahan: File '$src' tidak ditemukan pada direktori!"
        return 2
    fi
    mkdir -p "$dst"
    local timestamp=$(date +%Y%m%d_%H%M%S)
    cp "$src" "$dst/$(basename "$src").$timestamp.bak"
    echo "✅ Proses pencadangan berhasil: $dst/$(basename "$src").$timestamp.bak"
}
# ===== END PRAKTIKUM =====
> EOF
root@ubuntuser:~/praktikum-os/week07-bash# source ~/.bashrc
root@ubuntuser:~/praktikum-os/week07-bash# echo "✅ Konfigurasi baru telah diterapkan ke .bashrc"
✅ Konfigurasi baru telah diterapkan ke .bashrc
```

### Langkah 3: Pembuatan Skrip Manajemen Sistem pada Direktori bin

**Perintah Eksekusi:**

```bash
cat <<'EOF' > ~/praktikum-os/week07-bash/bin/ringkas-sistem
#!/usr/bin/env bash

echo "=========================================="
echo "        STATISTIK RINGKAS SISTEM"
echo "=========================================="
echo "Hostname    : $(hostname)"
echo "Pengguna    : $(whoami)"
echo "Waktu       : $(date '+%Y-%m-%d %H:%M:%S')"
echo "Waktu Aktif : $(uptime -p)"
echo "Kapasitas RAM: $(free -h | grep Mem | awk '{print $3 "/" $2}')"
echo "Penyimpanan /: $(df -h / | tail -1 | awk '{print $3 "/" $2 " (" $5 ")"}')"
echo "Shell Utama : $SHELL"
echo "=========================================="
EOF

chmod +x ~/praktikum-os/week07-bash/bin/ringkas-sistem

echo "✅ Skrip ringkas-sistem berhasil dikonfigurasi"
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# cat <<'EOF' > ~/praktikum-os/week07-bash/bin/ringkas-sistem
> #!/usr/bin/env bash

echo "=========================================="
echo "        STATISTIK RINGKAS SISTEM"
echo "=========================================="
echo "Hostname    : $(hostname)"
echo "Pengguna    : $(whoami)"
echo "Waktu       : $(date '+%Y-%m-%d %H:%M:%S')"
echo "Waktu Aktif : $(uptime -p)"
echo "Kapasitas RAM: $(free -h | grep Mem | awk '{print $3 "/" $2}')"
echo "Penyimpanan /: $(df -h / | tail -1 | awk '{print $3 "/" $2 " (" $5 ")"}')"
echo "Shell Utama : $SHELL"
echo "=========================================="
> EOF
root@ubuntuser:~/praktikum-os/week07-bash# chmod +x ~/praktikum-os/week07-bash/bin/ringkas-sistem
root@ubuntuser:~/praktikum-os/week07-bash# echo "✅ Skrip ringkas-sistem berhasil dikonfigurasi"
✅ Skrip ringkas-sistem berhasil dikonfigurasi
```

### Langkah 4: Pengujian Keseluruhan Konfigurasi

**Perintah Eksekusi:**

```bash
echo "=== PENGUJIAN 1: Verifikasi PATH ==="
echo "$PATH" | tr ':' '\n' | grep "week07-bash/bin"

echo -e "\n=== PENGUJIAN 2: Verifikasi Perintah Alias ll ==="
ll ~/praktikum-os/week07-bash/ | head -5

echo -e "\n=== PENGUJIAN 3: Verifikasi Fungsi backup_file ==="
backup_file ~/praktikum-os/week07-bash/sample-app.conf

echo -e "\n=== PENGUJIAN 4: Eksekusi Skrip ringkas-sistem ==="
cd /tmp
ringkas-sistem

cd ~/praktikum-os/week07-bash
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# echo "=== PENGUJIAN 1: Verifikasi PATH ==="
=== PENGUJIAN 1: Verifikasi PATH ===
root@ubuntuser:~/praktikum-os/week07-bash# echo "$PATH" | tr ':' '\n' | grep "week07-bash/bin"
/root/praktikum-os/week07-bash/bin
root@ubuntuser:~/praktikum-os/week07-bash# echo -e "\n=== PENGUJIAN 2: Verifikasi Perintah Alias ll ==="

=== PENGUJIAN 2: Verifikasi Perintah Alias ll ===
root@ubuntuser:~/praktikum-os/week07-bash# ll ~/praktikum-os/week07-bash/ | head -5
total 28K
drwxr-xr-x 7 root root 4.0K Apr 13 19:05 .
drwxr-xr-x 7 root root 4.0K Apr 13 19:04 ..
drwxr-xr-x 2 root root 4.0K Apr 13 19:04 backup
drwxr-xr-x 2 root root 4.0K Apr 13 19:20 bin
root@ubuntuser:~/praktikum-os/week07-bash# echo -e "\n=== PENGUJIAN 3: Verifikasi Fungsi backup_file ==="

=== PENGUJIAN 3: Verifikasi Fungsi backup_file ===
root@ubuntuser:~/praktikum-os/week07-bash# backup_file ~/praktikum-os/week07-bash/sample-app.conf
✅ Proses pencadangan berhasil: /root/praktikum-os/week07-bash/backup/sample-app.conf.20260413_190512.bak
root@ubuntuser:~/praktikum-os/week07-bash# echo -e "\n=== PENGUJIAN 4: Eksekusi Skrip ringkas-sistem ==="

=== PENGUJIAN 4: Eksekusi Skrip ringkas-sistem ===
root@ubuntuser:~/praktikum-os/week07-bash# cd /tmp
root@ubuntuser:/tmp# ringkas-sistem
==========================================
        STATISTIK RINGKAS SISTEM
==========================================
Hostname    : ubuntuser
Pengguna    : root
Waktu       : 2026-04-13 19:08:15
Waktu Aktif : up 26 minutes
Kapasitas RAM: 363Mi/3.3Gi
Penyimpanan /: 3.7G/50G (8%)
Shell Utama : /bin/bash
==========================================
root@ubuntuser:/tmp# cd ~/praktikum-os/week07-bash
root@ubuntuser:~/praktikum-os/week07-bash#
```

### Langkah 5: Penyusunan Laporan Praktikum Tugas 1

**Perintah Eksekusi:**

```bash
cat <<'EOF' > ~/praktikum-os/week07-bash/toolkit-bash-report.txt
==========================================
LAPORAN HASIL PRAKTIKUM 1
Toolkit Bash Administrator Pribadi
==========================================
Tanggal: $(date '+%Y-%m-%d %H:%M:%S')
Pengguna: $(whoami)
Sistem Host: $(hostname)

------------------------------------------
1. RINCIAN KONFIGURASI PADA .bashrc
------------------------------------------
EOF

echo -e "\n--- Detail Kode Konfigurasi ---" >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt
grep -A 30 "PRAKTIKUM BASH SHELL" ~/.bashrc >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt

echo -e "\n\n------------------------------------------
2. VERIFIKASI PATH VARIABEL
------------------------------------------" >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt
echo "$PATH" | tr ':' '\n' >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt

echo -e "\n\n------------------------------------------
3. IDENTIFIKASI TIPE ALIAS, FUNGSI, DAN SKRIP
------------------------------------------" >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt
echo "--- Analisis Perintah ll ---" >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt
type ll >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt 2>&1

echo -e "\n--- Analisis Fungsi backup_file ---" >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt
type backup_file >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt 2>&1

echo -e "\n--- Analisis Skrip ringkas-sistem ---" >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt
type ringkas-sistem >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt 2>&1

echo -e "\n\n------------------------------------------
4. REKAPITULASI HASIL PENGUJIAN
------------------------------------------" >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt
echo "Uji Coba Alias ll:" >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt
ll ~/praktikum-os/week07-bash/ >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt 2>&1

echo -e "\nUji Coba Fungsi backup_file:" >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt
backup_file ~/praktikum-os/week07-bash/sample-app.conf >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt 2>&1

echo -e "\nUji Coba Skrip ringkas-sistem:" >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt
ringkas-sistem >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt 2>&1

echo -e "\n\n==========================================
AKHIR DARI LAPORAN
==========================================" >> ~/praktikum-os/week07-bash/toolkit-bash-report.txt

echo "✅ Dokumen Laporan Tugas 1 selesai di-generate: ~/praktikum-os/week07-bash/toolkit-bash-report.txt"

cat ~/praktikum-os/week07-bash/toolkit-bash-report.txt
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# cat ~/praktikum-os/week07-bash/toolkit-bash-report.txt
==========================================
LAPORAN HASIL PRAKTIKUM 1
Toolkit Bash Administrator Pribadi
==========================================
Tanggal: $(date '+%Y-%m-%d %H:%M:%S')
Pengguna: $(whoami)
Sistem Host: $(hostname)

------------------------------------------
1. RINCIAN KONFIGURASI PADA .bashrc
------------------------------------------

--- Detail Kode Konfigurasi ---
# ===== PRAKTIKUM BASH SHELL - TUGAS 1 =====
# 1. Menambahkan direktori bin pribadi ke dalam PATH
export PATH="$HOME/praktikum-os/week07-bash/bin:$PATH"

# 2. Pembuatan alias untuk mempercepat rutinitas
alias ll='ls -lah --color=auto'
alias hist10='history | tail -10'
alias dfh='df -h'
alias freeh='free -h'

# 3. Fungsi pencadangan (backup) otomatis dengan timestamp
backup_file() {
    if [ $# -ne 1 ]; then
        echo "Format: backup_file <nama_file>"
        return 1
    fi
    local src="$1"
    local dst="$HOME/praktikum-os/week07-bash/backup"
    if [ ! -f "$src" ]; then
        echo "Kesalahan: File '$src' tidak ditemukan pada direktori!";
        return 2
    fi
    mkdir -p "$dst"
    local timestamp=$(date +%Y%m%d_%H%M%S)
    cp "$src" "$dst/$(basename "$src").$timestamp.bak"
    echo "✅ Proses pencadangan berhasil: $dst/$(basename "$src").$timestamp.bak"
}
# ===== END PRAKTIKUM =====



------------------------------------------
2. VERIFIKASI PATH VARIABEL
------------------------------------------
/root/praktikum-os/week07-bash/bin
/usr/local/sbin
/usr/local/bin
/usr/sbin
/usr/bin
/sbin
/bin
/usr/games
/usr/local/games
/snap/bin


------------------------------------------
3. IDENTIFIKASI TIPE ALIAS, FUNGSI, DAN SKRIP
------------------------------------------
--- Analisis Perintah ll ---
ll is aliased to `ls -lah --color=auto'

--- Analisis Fungsi backup_file ---
backup_file is a function
backup_file ()
{
    if [ $# -ne 1 ]; then
        echo "Format: backup_file <nama_file>";
        return 1;
    fi;
    local src="$1";
    local dst="$HOME/praktikum-os/week07-bash/backup";
    if [ ! -f "$src" ]; then
        echo "Kesalahan: File '$src' tidak ditemukan pada direktori!";
        return 2;
    fi;
    mkdir -p "$dst";
    local timestamp=$(date +%Y%m%d_%H%M%S);
    cp "$src" "$dst/$(basename "$src").$timestamp.bak";
    echo "✅ Proses pencadangan berhasil: $dst/$(basename "$src").$timestamp.bak"
}

--- Analisis Skrip ringkas-sistem ---
ringkas-sistem is hashed (/root/praktikum-os/week07-bash/bin/ringkas-sistem)


------------------------------------------
4. REKAPITULASI HASIL PENGUJIAN
------------------------------------------
Uji Coba Alias ll:
total 32K
drwxr-xr-x 7 root root 4.0K Apr 13 19:15 .
drwxr-xr-x 7 root root 4.0K Apr 13 19:04 ..
drwxr-xr-x 2 root root 4.0K Apr 13 19:05 backup
drwxr-xr-x 2 root root 4.0K Apr 13 19:20 bin
drwxr-xr-x 2 root root 4.0K Apr 13 19:06 logs
drwxr-xr-x 2 root root 4.0K Apr 13 19:08 ruang-nama
drwxr-xr-x 2 root root 4.0K Apr 13 19:07 sampel
-rw-r--r-- 1 root root    0 Apr 13 19:05 sample-app.conf
-rw-r--r-- 1 root root 2.4K Apr 13 19:15 toolkit-bash-report.txt

Uji Coba Fungsi backup_file:
✅ Proses pencadangan berhasil: /root/praktikum-os/week07-bash/backup/sample-app.conf.20260413_191540.bak

Uji Coba Skrip ringkas-sistem:
==========================================
        STATISTIK RINGKAS SISTEM
==========================================
Hostname    : ubuntuser
Pengguna    : root
Waktu       : 2026-04-13 19:15:46
Waktu Aktif : up 36 minutes
Kapasitas RAM: 361Mi/3.3Gi
Penyimpanan /: 3.7G/50G (8%)
Shell Utama : /bin/bash
==========================================


==========================================
AKHIR DARI LAPORAN
==========================================
```

-----

## Tugas Praktikum 2 — Audit File Konfigurasi dan Penanganan Log Secara Aman

### Langkah 1: Tahap Persiapan dan Pembuatan Laporan Audit

**Perintah Eksekusi:**

```bash
cd ~/praktikum-os/week07-bash

TGL=$(date +%F)

echo "=== INISIASI AUDIT KONFIGURASI ==="
echo "Tanggal Pelaksanaan : $TGL"
echo "Berkas Laporan      : audit-konfigurasi-$TGL.txt"
echo "Catatan Error       : audit-error.log"
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# cd ~/praktikum-os/week07-bash
root@ubuntuser:~/praktikum-os/week07-bash# TGL=$(date +%F)
root@ubuntuser:~/praktikum-os/week07-bash# echo "=== INISIASI AUDIT KONFIGURASI ==="
=== INISIASI AUDIT KONFIGURASI ===
root@ubuntuser:~/praktikum-os/week07-bash# echo "Tanggal Pelaksanaan : $TGL"
Tanggal Pelaksanaan : 2026-04-13
root@ubuntuser:~/praktikum-os/week07-bash# echo "Berkas Laporan      : audit-konfigurasi-$TGL.txt"
Berkas Laporan      : audit-konfigurasi-2026-04-13.txt
root@ubuntuser:~/praktikum-os/week07-bash# echo "Catatan Error       : audit-error.log"
Catatan Error       : audit-error.log
```

### Langkah 2: Pemindaian File `*.conf` di Direktori `/etc` Beserta Separasi *Output*

**Perintah Eksekusi:**

```bash
find /etc -name "*.conf" 2> audit-error.log > audit-konfigurasi-2026-04-13.txt

echo "=== HASIL PEMINDAIAN BERKAS .conf ==="
echo "Total file terekam: $(wc -l < audit-konfigurasi-2026-04-13.txt)"
echo "Total insiden error: $(wc -l < audit-error.log)"
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# find /etc -name "*.conf" 2> audit-error.log > audit-konfigurasi-2026-04-13.txt
root@ubuntuser:~/praktikum-os/week07-bash# echo "=== HASIL PEMINDAIAN BERKAS .conf ==="
=== HASIL PEMINDAIAN BERKAS .conf ===
root@ubuntuser:~/praktikum-os/week07-bash# echo "Total file terekam: $(wc -l < audit-konfigurasi-2026-04-13.txt)"
Total file terekam: 179
root@ubuntuser:~/praktikum-os/week07-bash# echo "Total insiden error: $(wc -l < audit-error.log)"
Total insiden error: 0
root@ubuntuser:~/praktikum-os/week07-bash#
```

### Langkah 3: Ekstraksi Laporan Menggunakan Perintah `tee`

**Perintah Eksekusi:**

```bash
echo "=== PREVIEW 10 FILE .conf PERTAMA ===" | tee -a audit-ringkasan-2026-04-13.txt
head -10 audit-konfigurasi-2026-04-13.txt | tee -a audit-ringkasan-2026-04-13.txt

echo -e "\n=== AKUMULASI TOTAL ===" | tee -a audit-ringkasan-2026-04-13.txt
echo "Total berkas konfigurasi ditemukan: $(wc -l < audit-konfigurasi-2026-04-13.txt)" | tee -a audit-ringkasan-2026-04-13.txt
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# echo "=== PREVIEW 10 FILE .conf PERTAMA ===" | tee -a audit-ringkasan-2026-04-13.txt
=== PREVIEW 10 FILE .conf PERTAMA ===
root@ubuntuser:~/praktikum-os/week07-bash# head -10 audit-konfigurasi-2026-04-13.txt | tee -a audit-ringkasan-2026-04-13.txt
/etc/UPower/UPower.conf
/etc/nftables.conf
/etc/ld.so.conf
/etc/sudo_logsrvd.conf
/etc/systemd/resolved.conf
/etc/systemd/logind.conf
/etc/systemd/sleep.conf
/etc/systemd/pstore.conf
/etc/systemd/timesyncd.conf.d/cloud-init.conf
/etc/systemd/user.conf
root@ubuntuser:~/praktikum-os/week07-bash# echo -e "\n=== AKUMULASI TOTAL ===" | tee -a audit-ringkasan-2026-04-13.txt
echo "Total berkas konfigurasi ditemukan: $(wc -l < audit-konfigurasi-2026-04-13.txt)" | tee -a audit-ringkasan-2026-04-13.txt

=== AKUMULASI TOTAL ===
Total berkas konfigurasi ditemukan: 179
```

### Langkah 4: Pencarian File Berbasis *Keyword* Tertentu via *Pipeline*

**Perintah Eksekusi:**

```bash
echo -e "\n=== BERKAS KONFIGURASI TERKAIT NETWORK DAN SSH ===" | tee -a audit-ringkasan-2026-04-13.txt

find /etc -name "*.conf" 2>/dev/null | grep -E "network|ssh" | sort | tee -a audit-ringkasan-2026-04-13.txt

echo -e "\nAkumulasi file terkait network/ssh: $(find /etc -name "*.conf" 2>/dev/null | grep -E "network|ssh" | wc -l)" | tee -a audit-ringkasan-2026-04-13.txt
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# echo -e "\n=== BERKAS KONFIGURASI TERKAIT NETWORK DAN SSH ===" | tee -a audit-ringkasan-2026-04-13.txt

=== BERKAS KONFIGURASI TERKAIT NETWORK DAN SSH ===
root@ubuntuser:~/praktikum-os/week07-bash# find /etc -name "*.conf" 2>/dev/null | grep -E "network|ssh" | sort | tee -a audit-ringkasan-2026-04-13.txt
/etc/modprobe.d/blacklist-rare-network.conf
/etc/sysctl.d/10-network-security.conf
/etc/systemd/networkd.conf
root@ubuntuser:~/praktikum-os/week07-bash# echo -e "\nAkumulasi file terkait network/ssh: $(find /etc -name "*.conf" 2>/dev/null | grep -E "network|ssh" | wc -l)" | tee -a audit-ringkasan-2026-04-13.txt

Akumulasi file terkait network/ssh: 3
```

### Langkah 5: Implementasi *Command Substitution* Untuk Pembuatan Ringkasan

**Perintah Eksekusi:**

```bash
cat <<EOF > audit-final-2026-04-13.txt
==========================================
DOKUMEN AUDIT KONFIGURASI SISTEM
==========================================
Waktu Inspeksi : $(date '+%Y-%m-%d %H:%M:%S')
Akun Pengeksekusi : $(whoami)
Nama Host         : $(hostname)
==========================================

1. REKAPITULASI FILE KONFIGURASI
   Total berkas *.conf  : $(wc -l < audit-konfigurasi-2026-04-13.txt) file
   Total error pemindaian : $(wc -l < audit-error.log) error

2. DIREKTORI DENGAN POPULASI FILE .conf TERTINGGI
$(find /etc -name "*.conf" 2>/dev/null | xargs dirname | sort | uniq -c | sort -rn | head -5)

3. SAMPEL ISI DARI BERKAS KONFIGURASI
   (Meninjau file: /etc/resolv.conf)
------------------------------------------
$(head -5 /etc/resolv.conf 2>/dev/null || echo "Akses pembacaan file ditolak")
------------------------------------------

==========================================
EOF

echo "✅ Ringkasan laporan final telah dieksekusi"
cat audit-final-2026-04-13.txt
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# cat <<EOF > audit-final-2026-04-13.txt
> ==========================================
DOKUMEN AUDIT KONFIGURASI SISTEM
==========================================
Waktu Inspeksi : $(date '+%Y-%m-%d %H:%M:%S')
Akun Pengeksekusi : $(whoami)
Nama Host         : $(hostname)
==========================================

1. REKAPITULASI FILE KONFIGURASI
   Total berkas *.conf  : $(wc -l < audit-konfigurasi-2026-04-13.txt) file
   Total error pemindaian : $(wc -l < audit-error.log) error

2. DIREKTORI DENGAN POPULASI FILE .conf TERTINGGI
$(find /etc -name "*.conf" 2>/dev/null | xargs dirname | sort | uniq -c | sort -rn | head -5)

3. SAMPEL ISI DARI BERKAS KONFIGURASI
   (Meninjau file: /etc/resolv.conf)
------------------------------------------
$(head -5 /etc/resolv.conf 2>/dev/null || echo "Akses pembacaan file ditolak")
------------------------------------------

==========================================
> EOF
root@ubuntuser:~/praktikum-os/week07-bash# echo "✅ Ringkasan laporan final telah dieksekusi"
cat audit-final-2026-04-13.txt
✅ Ringkasan laporan final telah dieksekusi
==========================================
DOKUMEN AUDIT KONFIGURASI SISTEM
==========================================
Waktu Inspeksi : 2026-04-13 19:22:10
Akun Pengeksekusi : root
Nama Host         : ubuntuser
==========================================

1. REKAPITULASI FILE KONFIGURASI
   Total berkas *.conf  : 179 file
   Total error pemindaian : 0 error

2. DIREKTORI DENGAN POPULASI FILE .conf TERTINGGI
     47 /etc/fonts/conf.d
     30 /etc
     14 /etc/fonts/conf.avail
     10 /etc/sysctl.d
     10 /etc/security

3. SAMPEL ISI DARI BERKAS KONFIGURASI
   (Meninjau file: /etc/resolv.conf)
------------------------------------------
# This is /run/systemd/resolve/stub-resolv.conf managed by man:systemd-resolved(8).
# Do not edit.
#
# This file might be symlinked as /etc/resolv.conf. If you're looking at
# /etc/resolv.conf and seeing this text, you have followed the symlink.
------------------------------------------

==========================================
```

### Langkah 6: Analisis Signifikansi Pemisahan Output (`stdout`) dan Error (`stderr`)

**Perintah Eksekusi:**

```bash
cat <<'EOF' >> audit-final-2026-04-13.txt

4. EVALUASI KEGUNAAN PEMISAHAN STDOUT DAN STDERR
==================================================
Pada saat menjalankan prosedur audit sistem, pemisahan antara aliran data reguler (stdout) 
dan log peringatan (stderr) memegang peranan krusial yang ditandai oleh beberapa faktor:

1. KEAKURATAN PROSES DIAGNOSIS
   - Peringatan seperti "Permission denied" mengisyaratkan adanya restriksi berkas.
   - Tanpa adanya separasi, notifikasi error akan berbaur dengan hasil audit, menyulitkan proses pembacaan.

2. PENGAWASAN ASPEK KEAMANAN
   - Berkas-berkas yang tertutup dari akses baca dapat memicu investigasi lanjutan.
   - Bisa saja mengindikasikan adanya celah pada regulasi hak akses keamanan.

3. OPTIMALISASI PELAPORAN
   - Memanfaatkan '2> error.log' memungkinkan pengabaian peringatan yang tidak vital.
   - Ataupun memberikan ruang untuk mengisolasi kendala spesifik demi tujuan perbaikan.

4. MENJAGA INTEGRITAS DATA
   - Data keluaran utama tetap terbebas dari interferensi teks peringatan.
   - Menghindari terjadinya polusi data pada rekaman hasil audit operasional.

Implementasi teknis pada praktikum ini:
- stdout (>) diarahkan ke audit-konfigurasi-*.txt (Perekaman data sah)
- stderr (2>) diarahkan ke audit-error.log (Perekaman aktivitas malfungsi)

==================================================
EOF

echo "✅ Analisis komprehensif berhasil disisipkan pada berkas laporan"
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# cat <<'EOF' >> audit-final-2026-04-13.txt
> 4. EVALUASI KEGUNAAN PEMISAHAN STDOUT DAN STDERR
==================================================
Pada saat menjalankan prosedur audit sistem, pemisahan antara aliran data reguler (stdout) 
dan log peringatan (stderr) memegang peranan krusial yang ditandai oleh beberapa faktor:

1. KEAKURATAN PROSES DIAGNOSIS
   - Peringatan seperti "Permission denied" mengisyaratkan adanya restriksi berkas.
   - Tanpa adanya separasi, notifikasi error akan berbaur dengan hasil audit, menyulitkan proses pembacaan.

2. PENGAWASAN ASPEK KEAMANAN
   - Berkas-berkas yang tertutup dari akses baca dapat memicu investigasi lanjutan.
   - Bisa saja mengindikasikan adanya celah pada regulasi hak akses keamanan.

3. OPTIMALISASI PELAPORAN
   - Memanfaatkan '2> error.log' memungkinkan pengabaian peringatan yang tidak vital.
   - Ataupun memberikan ruang untuk mengisolasi kendala spesifik demi tujuan perbaikan.

4. MENJAGA INTEGRITAS DATA
   - Data keluaran utama tetap terbebas dari interferensi teks peringatan.
   - Menghindari terjadinya polusi data pada rekaman hasil audit operasional.

Implementasi teknis pada praktikum ini:
- stdout (>) diarahkan ke audit-konfigurasi-*.txt (Perekaman data sah)
- stderr (2>) diarahkan ke audit-error.log (Perekaman aktivitas malfungsi)

==================================================
> EOF
root@ubuntuser:~/praktikum-os/week07-bash# echo "✅ Analisis komprehensif berhasil disisipkan pada berkas laporan"
✅ Analisis komprehensif berhasil disisipkan pada berkas laporan
```

### Langkah 7: Verifikasi Akhir Dokumen Laporan

**Perintah Eksekusi:**

```bash
echo "=== PRATINJAU DOKUMEN LAPORAN FINAL ==="
cat audit-final-2026-04-13.txt
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# echo "=== PRATINJAU DOKUMEN LAPORAN FINAL ==="
cat audit-final-2026-04-13.txt
=== PRATINJAU DOKUMEN LAPORAN FINAL ===
==========================================
DOKUMEN AUDIT KONFIGURASI SISTEM
==========================================
Waktu Inspeksi : 2026-04-13 19:22:10
Akun Pengeksekusi : root
Nama Host         : ubuntuser
==========================================

1. REKAPITULASI FILE KONFIGURASI
   Total berkas *.conf  : 179 file
   Total error pemindaian : 0 error

2. DIREKTORI DENGAN POPULASI FILE .conf TERTINGGI
     47 /etc/fonts/conf.d
     30 /etc
     14 /etc/fonts/conf.avail
     10 /etc/sysctl.d
     10 /etc/security

3. SAMPEL ISI DARI BERKAS KONFIGURASI
   (Meninjau file: /etc/resolv.conf)
------------------------------------------
# This is /run/systemd/resolve/stub-resolv.conf managed by man:systemd-resolved(8).
# Do not edit.
#
# This file might be symlinked as /etc/resolv.conf. If you're looking at
# /etc/resolv.conf and seeing this text, you have followed the symlink.
------------------------------------------

4. EVALUASI KEGUNAAN PEMISAHAN STDOUT DAN STDERR
==================================================
Pada saat menjalankan prosedur audit sistem, pemisahan antara aliran data reguler (stdout) 
dan log peringatan (stderr) memegang peranan krusial yang ditandai oleh beberapa faktor:

1. KEAKURATAN PROSES DIAGNOSIS
   - Peringatan seperti "Permission denied" mengisyaratkan adanya restriksi berkas.
   - Tanpa adanya separasi, notifikasi error akan berbaur dengan hasil audit, menyulitkan proses pembacaan.

2. PENGAWASAN ASPEK KEAMANAN
   - Berkas-berkas yang tertutup dari akses baca dapat memicu investigasi lanjutan.
   - Bisa saja mengindikasikan adanya celah pada regulasi hak akses keamanan.

3. OPTIMALISASI PELAPORAN
   - Memanfaatkan '2> error.log' memungkinkan pengabaian peringatan yang tidak vital.
   - Ataupun memberikan ruang untuk mengisolasi kendala spesifik demi tujuan perbaikan.

4. MENJAGA INTEGRITAS DATA
   - Data keluaran utama tetap terbebas dari interferensi teks peringatan.
   - Menghindari terjadinya polusi data pada rekaman hasil audit operasional.

Implementasi teknis pada praktikum ini:
- stdout (>) diarahkan ke audit-konfigurasi-*.txt (Perekaman data sah)
- stderr (2>) diarahkan ke audit-error.log (Perekaman aktivitas malfungsi)

==================================================
```

-----

## Tugas Praktikum 3 — Perancangan Skrip Inspeksi Server (*Daily Health Check*)

### Langkah 1: Penyusunan Skrip Pengecekan Sistem Harian

**Perintah Eksekusi:**

```bash
cd ~/praktikum-os/week07-bash

cat <<'EOF' > bin/daily-healthcheck
#!/usr/bin/env bash

# ============================================
# PROGRAM: daily-healthcheck
# TUJUAN: Pemeriksaan vitalitas server secara periodik
# ============================================

# Inisiasi Variabel Direktori
LOG_DIR="$HOME/praktikum-os/week07-bash/logs"
TGL=$(date +%F)
LOG_FILE="$LOG_DIR/healthcheck-$TGL.log"

# Pembuatan wadah log jika belum tersedia
mkdir -p "$LOG_DIR"

# Modul perenderan judul header
show_header() {
    echo "=========================================="
    echo "     INSPEKSI VITALITAS SERVER HARIAN"
    echo "=========================================="
}

# Blok eksekusi utama
{
    show_header
    echo ""
    
    # 1. Identifikasi Waktu
    echo " [1] DATA WAKTU & TANGGAL"
    echo "    $(date '+%Y-%m-%d %H:%M:%S')"
    echo ""
    
    # 2. Identifikasi Mesin
    echo "  [2] NAMA HOSTNAME"
    echo "    $(hostname)"
    echo ""
    
    # 3. Kredensial Pengguna
    echo " [3] PENGGUNA TEROTENTIKASI"
    echo "    User: $(whoami)"
    echo "    Interpreter: $SHELL"
    echo ""
    
    # 4. Durasi Aktif Server
    echo "  [4] UPTIME SISTEM"
    echo "    $(uptime -p)"
    echo ""
    
    # 5. Utilisasi RAM
    echo " [5] PEMAKAIAN MEMORI"
    free -h | grep -E "Mem|Swap" | while read line; do
        echo "    $line"
    done
    echo ""
    
    # 6. Evaluasi Kapasitas Penyimpanan Utama
    echo " [6] STATISTIK DISK DIREKTORI ROOT (/)" 
    df -h / | tail -1 | awk '{print "    Kapasitas: " $2 " | Terpakai: " $3 " | Tersedia: " $4 " | Persentase: " $5}'
    echo ""
    
    # 7. Rekam Jejak Terminal Terakhir (Edisi Pemantauan)
    echo " [7] REKAMAN 10 INSTRUKSI TERAKHIR"
    echo "    (Terbatas pada sintaks diagnostik)"
    history | tail -20 | grep -E "df|free|uptime|ps|top|htop|systemctl" | tail -10 | while read line; do
        echo "    $line"
    done
    echo ""
    
    # 8. Proses dengan Beban Komputasi Terberat
    echo " [8] TOP 3 APLIKASI PENYEDOT CPU"
    ps aux --sort=-%cpu | head -4 | tail -3 | while read line; do
        echo "    $line"
    done
    echo ""
    
    # 9. Verifikasi Status Layanan Pokok
    echo " [9] KONDISI LAYANAN SISTEM KRUSIAL"
    for service in ssh cron; do
        if systemctl is-active --quiet "$service" 2>/dev/null; then
            echo "     $service: Aktif & Berjalan"
        else
            echo "     $service: Terhenti / Tidak Terdeteksi"
        fi
    done
    echo ""
    
    show_header
    echo " Sesi pemantauan diselesaikan pada: $(date '+%Y-%m-%d %H:%M:%S')"
    
} | tee -a "$LOG_FILE"

# Validasi keberhasilan proses
if [ $? -eq 0 ]; then
    echo ""
    echo " Rekaman log telah diarsipkan menuju: $LOG_FILE"
else
    echo " Ditemukan galat selama inspeksi berlangsung"
    exit 1
fi
EOF

chmod +x bin/daily-healthcheck
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# cat <<'EOF' > bin/daily-healthche
ck
> #!/usr/bin/env bash
>
# ============================================
# PROGRAM: daily-healthcheck
# TUJUAN: Pemeriksaan vitalitas server secara periodik
# ============================================

# Inisiasi Variabel Direktori
LOG_DIR="$HOME/praktikum-os/week07-bash/logs"
TGL=$(date +%F)
LOG_FILE="$LOG_DIR/healthcheck-$TGL.log"

# Pembuatan wadah log jika belum tersedia
mkdir -p "$LOG_DIR"

# Modul perenderan judul header
show_header() {
    echo "=========================================="
    echo "     INSPEKSI VITALITAS SERVER HARIAN"
    echo "=========================================="
}

# Blok eksekusi utama
{
    show_header
    echo ""

>     # 1. Identifikasi Waktu
    echo " [1] DATA WAKTU & TANGGAL"
    echo "    $(date '+%Y-%m-%d %H:%M:%S')"
    echo ""

>   # 2. Identifikasi Mesin
    echo " [2] NAMA HOSTNAME"
    echo "    $(hostname)"
    echo ""

>  # 3. Kredensial Pengguna
    echo " [3] PENGGUNA TEROTENTIKASI"
    echo "    User: $(whoami)"
    echo "    Interpreter: $SHELL"
    echo ""

> # 4. Durasi Aktif Server
    echo " [4] UPTIME SISTEM"
    echo "    $(uptime -p)"
    echo ""

    # 5. Utilisasi RAM
    echo " [5] PEMAKAIAN MEMORI"
    free -h | grep -E "Mem|Swap" | while read line; do
        echo "    $line"
    done
    echo ""

    # 6. Evaluasi Kapasitas Penyimpanan Utama
    echo " [6] STATISTIK DISK DIREKTORI ROOT (/)"
    df -h / | tail -1 | awk '{print "    Kapasitas: " $2 " | Terpakai: " $3 " | Tersedia
: " $4 " | Persentase: " $5}'
    echo ""

    # 7. Rekam Jejak Terminal Terakhir (Edisi Pemantauan)
    echo " [7] REKAMAN 10 INSTRUKSI TERAKHIR"
    echo "    (Terbatas pada sintaks diagnostik)"
    history | tail -20 | grep -E "df|free|uptime|ps|top|htop|systemctl" | ta
il -10 | while read line; do
        echo "    $line"
    done
    echo ""

>  # 8. Proses dengan Beban Komputasi Terberat
    echo " [8] TOP 3 APLIKASI PENYEDOT CPU"
    ps aux --sort=-%cpu | head -4 | tail -3 | while read line; do
        echo "    $line"
    done
    echo ""

    # 9. Verifikasi Status Layanan Pokok
    echo "🔧 [9] KONDISI LAYANAN SISTEM KRUSIAL"
    for service in ssh cron; do
        if systemctl is-active --quiet "$service" 2>/dev/null; then
            echo "     $service: Aktif & Berjalan"
        else
            echo "     $service: Terhenti / Tidak Terdeteksi"
        fi
    done
    echo ""

    show_header
    echo " Sesi pemantauan diselesaikan pada: $(date '+%Y-%m-%d %H:%M:%S')"

} | tee -a "$LOG_FILE"

# Validasi keberhasilan proses
if [ $? -eq 0 ]; then
    echo ""
    echo " Rekaman log telah diarsipkan menuju: $LOG_FILE"
else
    echo " Ditemukan galat selama inspeksi berlangsung"
    exit 1
fi
> EOF
root@ubuntuser:~/praktikum-os/week07-bash# chmod +x bin/daily-healthcheck
```

### Langkah 2: Simulasi Pengeksekusian Skrip

**Perintah Eksekusi:**

```bash
daily-healthcheck
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# daily-healthcheck
==========================================
     INSPEKSI VITALITAS SERVER HARIAN
==========================================

 [1] DATA WAKTU & TANGGAL
    2026-04-13 19:30:15

 [2] NAMA HOSTNAME
    ubuntuser

 [3] PENGGUNA TEROTENTIKASI
    User: root
    Interpreter: /bin/bash

 [4] UPTIME SISTEM
    up 17 minutes

 [5] PEMAKAIAN MEMORI
    Mem:           3.3Gi       352Mi       2.9Gi       1.0Mi       245Mi       3.0Gi
    Swap:             0B          0B          0B

 [6] STATISTIK DISK DIREKTORI ROOT (/)
    Kapasitas: 50G | Terpakai: 3.5G | Tersedia: 44G | Persentase: 8%

 [7] REKAMAN 10 INSTRUKSI TERAKHIR
    (Terbatas pada sintaks diagnostik)

 [8] TOP 3 APLIKASI PENYEDOT CPU
    root        1193  166  0.1  11016  4632 pts/2    R+   19:30   0:00 ps aux --sort=-%cpu
    root        1176  5.8  0.0   7340  2160 pts/2    S+   19:30   0:00 bash /root/praktikum-os/week07-bash/bin/daily-healthcheck
    root        1173  4.5  0.1   7340  3708 pts/2    S+   19:30   0:00 bash /root/praktikum-os/week07-bash/bin/daily-healthcheck

 [9] KONDISI LAYANAN SISTEM KRUSIAL
     ssh: Aktif & Berjalan
     cron: Aktif & Berjalan

==========================================
     INSPEKSI VITALITAS SERVER HARIAN
==========================================
 Sesi pemantauan diselesaikan pada: 2026-04-13 19:30:15

 Rekaman log telah diarsipkan menuju: /root/praktikum-os/week07-bash/logs/healthcheck-2026-04-13.log
```

### Langkah 3: Tes Pemanggilan Skrip Secara Universal

**Perintah Eksekusi:**

```bash
cd /tmp

daily-healthcheck

cd ~/praktikum-os/week07-bash
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# cd /tmp

daily-healthcheck

cd ~/praktikum-os/week07-bash
==========================================
     INSPEKSI VITALITAS SERVER HARIAN
==========================================

 [1] DATA WAKTU & TANGGAL
    2026-04-13 19:32:45

[2] NAMA HOSTNAME
    ubuntuser

 [3] PENGGUNA TEROTENTIKASI
    User: root
    Interpreter: /bin/bash

[4] UPTIME SISTEM
    up 21 minutes

 [5] PEMAKAIAN MEMORI
    Mem:           3.3Gi       352Mi       2.9Gi       1.0Mi       246Mi       3.0Gi
    Swap:             0B          0B          0B

 [6] STATISTIK DISK DIREKTORI ROOT (/)
    Kapasitas: 50G | Terpakai: 3.5G | Tersedia: 44G | Persentase: 8%

 [7] REKAMAN 10 INSTRUKSI TERAKHIR
    (Terbatas pada sintaks diagnostik)

 [8] TOP 3 APLIKASI PENYEDOT CPU
    root        1233  200  0.1  11016  4624 pts/2    R+   19:32   0:00 ps aux --sort=-%cpu
    root        1216  9.0  0.0   7340  2120 pts/2    S+   19:32   0:00 bash /root/praktikum-os/week07-bash/bin/daily-healthcheck
    root        1217  9.0  0.0   5692  1988 pts/2    S+   19:32   0:00 tee -a /root/praktikum-os/week07-bash/logs/healthcheck-2026-04-13.log

🔧 [9] KONDISI LAYANAN SISTEM KRUSIAL
     ssh: Aktif & Berjalan
     cron: Aktif & Berjalan

==========================================
     INSPEKSI VITALITAS SERVER HARIAN
==========================================
 Sesi pemantauan diselesaikan pada: 2026-04-13 19:32:45

 Rekaman log telah diarsipkan menuju: /root/praktikum-os/week07-bash/logs/healthcheck-2026-04-13.log
```

### Langkah 4: Registrasi Alias Untuk Efisiensi Operasional

**Perintah Eksekusi:**

```bash
cat <<'EOF' >> ~/.bashrc

# Pintasan untuk inisiasi skrip inspeksi
alias hc='daily-healthcheck'
alias hclog='tail -20 ~/praktikum-os/week07-bash/logs/healthcheck-*.log | tail -20'
EOF

source ~/.bashrc

echo "=== PENGUJIAN PERINTAH hc ==="
hc 2>&1 | head -10
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# cat <<'EOF' >> ~/.bashrc
> # Pintasan untuk inisiasi skrip inspeksi
alias hc='daily-healthcheck'
alias hclog='tail -20 ~/praktikum-os/week07-bash/logs/healthcheck-*.log | tail -20'
> EOF
root@ubuntuser:~/praktikum-os/week07-bash# source ~/.bashrc
root@ubuntuser:~/praktikum-os/week07-bash# echo "=== PENGUJIAN PERINTAH hc ==="
hc 2>&1 | head -10
=== PENGUJIAN PERINTAH hc ===
==========================================
     INSPEKSI VITALITAS SERVER HARIAN
==========================================

 [1] DATA WAKTU & TANGGAL
    2026-04-13 19:35:10

[2] NAMA HOSTNAME
    ubuntuser

```

### Langkah 5: Penciptaan Dokumen *Readme* Skrip

**Perintah Eksekusi:**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# cat <<'EOF' > healthcheck-documentation.txt
> ==========================================
 MANUAL PENGGUNAAN SKRIP DAILY-HEALTHCHECK
==========================================

DIREKTORI PENEMPATAN
   ~/praktikum-os/week07-bash/bin/daily-healthcheck

PEMBEDAHAN MODUL FUNGSI
------------------------------------------
1. DATA WAKTU & TANGGAL
   - Menyediakan stempel waktu kapan sistem diaudit.
   - Esensial guna menelusuri garis waktu pengecekan infrastruktur.

2. NAMA HOSTNAME
   - Menegaskan identitas server yang tengah dioperasikan.
   - Sebagai mitigasi atas kelalaian penargetan server.

3. PENGGUNA TEROTENTIKASI
   - Mengklarifikasi otorisasi pengguna yang tengah log in.
   - Memanfaatkan pemanggilan variabel $USER dan $SHELL.

> 4. UPTIME SISTEM
   - Menghitung rentang waktu pasca re-boot terakhir.
   - Sebagai acuan stabilitas atau ada tidaknya interupsi server.

5. PEMAKAIAN MEMORI
   - Memberikan laporan pemakaian sirkulasi memori dan swap.
   - Berguna untuk merespons gejala over-capacity pada RAM.

6. STATISTIK DISK DIREKTORI ROOT
   - Memonitor saturasi ruang penyimpanan pada level teratas sistem (/).
   - Sangat membantu untuk mencegah crash akibat memori penuh.

7. REKAMAN 10 INSTRUKSI TERAKHIR
   - Memperlihatkan manuver administrasi terbaru pada shell.
   - Menfasilitasi analisis log aktivitas jika terjadi kerusakan konfigurasi.

8. TOP 3 APLIKASI PENYEDOT CPU
   - Melakukan pelacakan dini pada proses-proses boros sumber daya.
   - Memudahkan troubleshooting performa yang mulai tersendat.

9. KONDISI LAYANAN SISTEM KRUSIAL
   - Penjamin fungsi dasar berjalan lancar.
   - Layanan seperti koneksi Remote (ssh) dan task scheduler (cron).

> LANDASAN PENGGUNAAN BASH
------------------------------------------
Environment Variable: Pemanfaatan $HOME, $SHELL, $USER
Konfigurasi PATH: Agar eksekusi berlaku di sembarang folder.
Alias: Shortcut khusus dengan perintah 'hc'
Riwayat: Teknik pemanggilan jejak sejarah shell
Operator Tee: Teknik merekam log bersamaan dengan mencetaknya ke monitor.
Error Handling: Membaca status eksekusi lewat nilai balik $?
> HASIL AKHIR
------------------------------------------
Mode Tampilan : Output dicitrakan langsung via terminal.
Mode Rekaman  : Tersimpan rapi pada ~/praktikum-os/week07-bash/logs/healthcheck-YYYY-MM-DD.log

==========================================
> EOF
```

**Hasil Keluaran (*Output*):**

```bash
root@ubuntuser:~/praktikum-os/week07-bash# cat healthcheck-documentation.txt
==========================================
 MANUAL PENGGUNAAN SKRIP DAILY-HEALTHCHECK
==========================================

DIREKTORI PENEMPATAN
   ~/praktikum-os/week07-bash/bin/daily-healthcheck

PEMBEDAHAN MODUL FUNGSI
------------------------------------------
1. DATA WAKTU & TANGGAL
   - Menyediakan stempel waktu kapan sistem diaudit.
   - Esensial guna menelusuri garis waktu pengecekan infrastruktur.

2. NAMA HOSTNAME
   - Menegaskan identitas server yang tengah dioperasikan.
   - Sebagai mitigasi atas kelalaian penargetan server.

3. PENGGUNA TEROTENTIKASI
   - Mengklarifikasi otorisasi pengguna yang tengah log in.
   - Memanfaatkan pemanggilan variabel $USER dan $SHELL.


4. UPTIME SISTEM
   - Menghitung rentang waktu pasca re-boot terakhir.
   - Sebagai acuan stabilitas atau ada tidaknya interupsi server.

5. PEMAKAIAN MEMORI
   - Memberikan laporan pemakaian sirkulasi memori dan swap.
   - Berguna untuk merespons gejala over-capacity pada RAM.

6. STATISTIK DISK DIREKTORI ROOT
   - Memonitor saturasi ruang penyimpanan pada level teratas sistem (/).
   - Sangat membantu untuk mencegah crash akibat memori penuh.

7. REKAMAN 10 INSTRUKSI TERAKHIR
   - Memperlihatkan manuver administrasi terbaru pada shell.
   - Menfasilitasi analisis log aktivitas jika terjadi kerusakan konfigurasi.

8. TOP 3 APLIKASI PENYEDOT CPU
   - Melakukan pelacakan dini pada proses-proses boros sumber daya.
   - Memudahkan troubleshooting performa yang mulai tersendat.

9. KONDISI LAYANAN SISTEM KRUSIAL
   - Penjamin fungsi dasar berjalan lancar.
   - Layanan seperti koneksi Remote (ssh) dan task scheduler (cron).


LANDASAN PENGGUNAAN BASH
------------------------------------------
Environment Variable: Pemanfaatan $HOME, $SHELL, $USER
Konfigurasi PATH: Agar eksekusi berlaku di sembarang folder.
Alias: Shortcut khusus dengan perintah 'hc'
Riwayat: Teknik pemanggilan jejak sejarah shell
Operator Tee: Teknik merekam log bersamaan dengan mencetaknya ke monitor.
Error Handling: Membaca status eksekusi lewat nilai balik $?
HASIL AKHIR
------------------------------------------
Mode Tampilan : Output dicitrakan langsung via terminal.
Mode Rekaman  : Tersimpan rapi pada ~/praktikum-os/week07-bash/logs/healthcheck-YYYY-MM-DD.log

==========================================
```

-----

## Tugas Praktikum 4 — Prosedur Penanganan Nama File Unik dan Manajemen Pengarsipan

### Langkah 1: Eksperimen Pembuatan File dengan Variasi Nama Sulit

**Perintah Eksekusi:**

```bash
cd ~/praktikum-os/week07-bash

mkdir -p tugas4-sample tugas4-backup

cd tugas4-sample

echo "=== MEMBENTUK FILE EKSPERIMENTAL DENGAN NAMA KOMPLEKS ==="

touch "laporan keuangan april.csv"
touch "backup server 2026.tar"

touch "config[production].ini"
touch "data[2026-04-10].json"
touch "log_backup_(server).txt"

touch access-log-01.txt
touch access-log-02.txt
touch access-log-03.txt
touch error-log-01.txt
touch error-log-02.txt
touch error-log-03.txt

touch "this_is_a_very_long_filename_for_testing_wildcard_pattern_matching.log"

touch user_data_2024.csv
touch user_data_2025.csv
touch user_data_2026.csv

echo ""
echo "✅ File simulasi telah berhasil digenerate"
echo ""

echo "=== LISTING DIREKTORI tugas4-sample ==="
ls -la
```

**Hasil Keluaran (*Output*):**

```bash
=== MEMBENTUK FILE EKSPERIMENTAL DENGAN NAMA KOMPLEKS ===

✅ File simulasi telah berhasil digenerate

=== LISTING DIREKTORI tugas4-sample ===
total 8
drwxr-xr-x 2 root root 4096 Apr 13 19:40  .
drwxr-xr-x 9 root root 4096 Apr 13 19:40  ..
-rw-r--r-- 1 root root    0 Apr 13 19:40  access-log-01.txt
-rw-r--r-- 1 root root    0 Apr 13 19:40  access-log-02.txt
-rw-r--r-- 1 root root    0 Apr 13 19:40  access-log-03.txt
-rw-r--r-- 1 root root    0 Apr 13 19:40 'backup server 2026.tar'
-rw-r--r-- 1 root root    0 Apr 13 19:40 'config[production].ini'
-rw-r--r-- 1 root root    0 Apr 13 19:40 'data[2026-04-10].json'
-rw-r--r-- 1 root root    0 Apr 13 19:40  error-log-01.txt
-rw-r--r-- 1 root root    0 Apr 13 19:40  error-log-02.txt
-rw-r--r-- 1 root root    0 Apr 13 19:40  error-log-03.txt
-rw-r--r-- 1 root root    0 Apr 13 19:40 'laporan keuangan april.csv'
-rw-r--r-- 1 root root    0 Apr 13 19:40 'log_backup_(server).txt'
-rw-r--r-- 1 root root    0 Apr 13 19:40  this_is_a_very_long_filename_for_testing_wildcard_pattern_matching.log
-rw-r--r-- 1 root root    0 Apr 13 19:40  user_data_2024.csv
-rw-r--r-- 1 root root    0 Apr 13 19:40  user_data_2025.csv
-rw-r--r-- 1 root root    0 Apr 13 19:40  user_data_2026.csv
```

### Langkah 2: Eksplorasi Implementasi Metode Pembacaan Literal (*Quoting*)

**Perintah Eksekusi:**

```bash
echo "=========================================="
echo "SIMULASI KETAHANAN SYNTAX QUOTING"
echo "=========================================="
echo ""

FILE_SPASI="laporan keuangan april.csv"

echo "1️. TANPA TANDA KUTIP (FATAL)"
echo "   Sintaks: ls -la $FILE_SPASI"
echo "   Keluaran:"
ls -la $FILE_SPASI 2>&1
echo "Evaluasi: Bash salah sangka dan menganggap string itu adalah 4 file yang terpisah!"
echo ""

echo "2️. DILENGKAPI KUTIP GANDA (DIREKOMENDASIKAN)"
echo "   Sintaks: ls -la \"$FILE_SPASI\""
echo "   Keluaran:"
ls -la "$FILE_SPASI"
echo "Evaluasi: Berhasil, spasi dibaca satu kesatuan karakter!"
echo ""

FILE_KURUNG="config[production].ini"

echo "3️. PENANGANAN TANPA KUTIP PADA KARAKTER KURUNG"
echo "   Sintaks: ls $FILE_KURUNG"
ls $FILE_KURUNG 2>&1
echo "Evaluasi: Simbol [ ] memicu mekanisme wildcard yang tidak relevan!"
echo ""

echo "4️. MELIBATKAN PENUTUP KUTIP GANDA ATAU ESCAPING"
echo "   Sintaks: ls \"$FILE_KURUNG\""
ls "$FILE_KURUNG"
echo "Evaluasi: Terbaca sempurna karena diselimuti double quote."
echo ""

echo "5️. TEKNIK INJEKSI ESCAPE (BACKSLASH)"
echo "   Sintaks: ls config\[production\].ini"
ls config\[production\].ini
echo "Evaluasi: Sempurna diidentifikasi murni lewat injeksi escape character."
echo ""
```

**Hasil Keluaran (*Output*):**

```bash
==========================================
SIMULASI KETAHANAN SYNTAX QUOTING
==========================================

1️. TANPA TANDA KUTIP (FATAL)
   Sintaks: ls -la laporan keuangan april.csv
   Keluaran:
ls: cannot access 'laporan': No such file or directory
ls: cannot access 'keuangan': No such file or directory
ls: cannot access 'april.csv': No such file or directory
Evaluasi: Bash salah sangka dan menganggap string itu adalah 4 file yang terpisah!

2️. DILENGKAPI KUTIP GANDA (DIREKOMENDASIKAN)
   Sintaks: ls -la "laporan keuangan april.csv"
   Keluaran:
-rw-r--r-- 1 root root 0 Apr 13 19:40 'laporan keuangan april.csv'
Evaluasi: Berhasil, spasi dibaca satu kesatuan karakter!

3️. PENANGANAN TANPA KUTIP PADA KARAKTER KURUNG
   Sintaks: ls config[production].ini
'config[production].ini'
Evaluasi: Simbol [ ] memicu mekanisme wildcard yang tidak relevan!

4️. MELIBATKAN PENUTUP KUTIP GANDA ATAU ESCAPING
   Sintaks: ls "config[production].ini"
'config[production].ini'
Evaluasi: Terbaca sempurna karena diselimuti double quote.

5️. TEKNIK INJEKSI ESCAPE (BACKSLASH)
   Sintaks: ls config\[production\].ini
'config[production].ini'
Evaluasi: Sempurna diidentifikasi murni lewat injeksi escape character.

```

### Langkah 3: Pratinjau Dampak Wildcard via Echo (Praktek Keamanan Dasar)

**Perintah Eksekusi:**

```bash
echo ""
echo "=========================================="
echo "PRATINJAU WILDCARD DENGAN ECHO (SAFE MODE)"
echo "=========================================="
echo ""

echo "Pola 1: *.txt"
echo "   Gambaran:"
echo *.txt
echo "   Akumulasi: $(echo *.txt | wc -w) dokumen"
echo ""

echo "Pola 2: access-log-*.txt"
echo "   Gambaran:"
echo access-log-*.txt
echo "   Akumulasi: $(echo access-log-*.txt | wc -w) dokumen"
echo ""

echo "Pola 3: *-log-*.txt"
echo "   Gambaran:"
echo *-log-*.txt
echo "   Akumulasi: $(echo *-log-*.txt | wc -w) dokumen"
echo ""

echo "Pola 4: user_data_202?.csv"
echo "   Gambaran:"
echo user_data_202?.csv
echo "   Akumulasi: $(echo user_data_202?.csv | wc -w) dokumen"
echo ""

echo "Pola 5: *[2026]*"
echo "   Gambaran:"
echo *[2026]* 2>&1
echo "Ingat: pola [ ] bisa menyebabkan error fatal apabila tidak memiliki padanan file yang pas!"
echo ""
```

**Hasil Keluaran (*Output*):**

```bash
==========================================
PRATINJAU WILDCARD DENGAN ECHO (SAFE MODE)
==========================================

Pola 1: *.txt
   Gambaran:
access-log-01.txt access-log-02.txt access-log-03.txt error-log-01.txt error-log-02.txt error-log-03.txt log_backup_(server).txt
   Akumulasi: 7 dokumen

Pola 2: access-log-*.txt
   Gambaran:
access-log-01.txt access-log-02.txt access-log-03.txt
   Akumulasi: 3 dokumen

Pola 3: *-log-*.txt
   Gambaran:
access-log-01.txt access-log-02.txt access-log-03.txt error-log-01.txt error-log-02.txt error-log-03.txt
   Akumulasi: 6 dokumen

Pola 4: user_data_202?.csv
   Gambaran:
user_data_2024.csv user_data_2025.csv user_data_2026.csv
   Akumulasi: 3 dokumen

Pola 5: *[2026]*
   Gambaran:
access-log-01.txt access-log-02.txt access-log-03.txt backup server 2026.tar data[2026-04-10].json error-log-01.txt error-log-02.txt error-log-03.txt user_data_2024.csv user_data_2025.csv user_data_2026.csv
Ingat: pola [ ] bisa menyebabkan error fatal apabila tidak memiliki padanan file yang pas!

```

### Langkah 4: Operasi Duplikasi File Menggunakan Tata Cara Aman

**Perintah Eksekusi:**

```bash
echo ""
echo "=========================================="
echo "PROSES MIGRASI DATA MENUJU DIREKTORI BACKUP"
echo "=========================================="
echo ""

BACKUP_DIR="$HOME/praktikum-os/week07-bash/tugas4-backup"
SOURCE_DIR="$HOME/praktikum-os/week07-bash/tugas4-sample"

TIMESTAMP=$(date +%Y%m%d_%H%M%S)
BACKUP_SUBDIR="$BACKUP_DIR/backup_$TIMESTAMP"
mkdir -p "$BACKUP_SUBDIR"

echo "Tujuan direktori preservasi: $BACKUP_SUBDIR"
echo ""

echo "1️. Prosedur duplikasi pada file berspasi (dibantu fungsi quoting):"
cp -v "laporan keuangan april.csv" "$BACKUP_SUBDIR/"
echo ""

echo "2️. Menyiasati penyalinan pada file dengan blok kurung (memakai instrumen escape):"
cp -v config\[production\].ini "$BACKUP_SUBDIR/"
echo ""

echo "3️. Memanfaatkan string wildcard pasca peninjauan aman:"
cp -v access-log-*.txt "$BACKUP_SUBDIR/"
echo ""

echo "4️. Menyalin identitas penamaan super panjang:"
cp -v "this_is_a_very_long_filename_for_testing_wildcard_pattern_matching.log" "$BACKUP_SUBDIR/"
echo ""

echo "5️. Borongan untuk seluruh ekstensi .csv:"
cp -v *.csv "$BACKUP_SUBDIR/"
echo ""

echo "6️. Melewati rintangan nama berkas rumit memanfaatkan variabel substitusi:"
FILE_SPECIAL="log_backup_(server).txt"
cp -v "$FILE_SPECIAL" "$BACKUP_SUBDIR/"
echo ""

echo "Integrasi penyalinan data sukses dilakukan!"
echo ""

echo "=== INSIDE LOOK DIREKTORI BACKUP ==="
ls -la "$BACKUP_SUBDIR/"
```

**Hasil Keluaran (*Output*):**

```bash
==========================================
PROSES MIGRASI DATA MENUJU DIREKTORI BACKUP
==========================================

Tujuan direktori preservasi: /root/praktikum-os/week07-bash/tugas4-backup/backup_20260413_194511

1️. Prosedur duplikasi pada file berspasi (dibantu fungsi quoting):
'laporan keuangan april.csv' -> '/root/praktikum-os/week07-bash/tugas4-backup/backup_20260413_194511/laporan keuangan april.csv'

2️. Menyiasati penyalinan pada file dengan blok kurung (memakai instrumen escape):
'config[production].ini' -> '/root/praktikum-os/week07-bash/tugas4-backup/backup_20260413_194511/config[production].ini'

3️. Memanfaatkan string wildcard pasca peninjauan aman:
'access-log-01.txt' -> '/root/praktikum-os/week07-bash/tugas4-backup/backup_20260413_194511/access-log-01.txt'
'access-log-02.txt' -> '/root/praktikum-os/week07-bash/tugas4-backup/backup_20260413_194511/access-log-02.txt'
'access-log-03.txt' -> '/root/praktikum-os/week07-bash/tugas4-backup/backup_20260413_194511/access-log-03.txt'

4️. Menyalin identitas penamaan super panjang:
'this_is_a_very_long_filename_for_testing_wildcard_pattern_matching.log' -> '/root/praktikum-os/week07-bash/tugas4-backup/backup_20260413_194511/this_is_a_very_long_filename_for_testing_wildcard_pattern_matching.log'

5️. Borongan untuk seluruh ekstensi .csv:
'laporan keuangan april.csv' -> '/root/praktikum-os/week07-bash/tugas4-backup/backup_20260413_194511/laporan keuangan april.csv'
'user_data_2024.csv' -> '/root/praktikum-os/week07-bash/tugas4-backup/backup_20260413_194511/user_data_2024.csv'
'user_data_2025.csv' -> '/root/praktikum-os/week07-bash/tugas4-backup/backup_20260413_194511/user_data_2025.csv'
'user_data_2026.csv' -> '/root/praktikum-os/week07-bash/tugas4-backup/backup_20260413_194511/user_data_2026.csv'

6️. Melewati rintangan nama berkas rumit memanfaatkan variabel substitusi:
'log_backup_(server).txt' -> '/root/praktikum-os/week07-bash/tugas4-backup/backup_20260413_194511/log_backup_(server).txt'

Integrasi penyalinan data sukses dilakukan!

=== INSIDE LOOK DIREKTORI BACKUP ===
total 8
drwxr-xr-x 2 root root 4096 Apr 13 19:45  .
drwxr-xr-x 3 root root 4096 Apr 13 19:45  ..
-rw-r--r-- 1 root root    0 Apr 13 19:45  access-log-01.txt
-rw-r--r-- 1 root root    0 Apr 13 19:45  access-log-02.txt
-rw-r--r-- 1 root root    0 Apr 13 19:45  access-log-03.txt
-rw-r--r-- 1 root root    0 Apr 13 19:45 'config[production].ini'
-rw-r--r-- 1 root root    0 Apr 13 19:45 'laporan keuangan april.csv'
-rw-r--r-- 1 root root    0 Apr 13 19:45 'log_backup_(server).txt'
-rw-r--r-- 1 root root    0 Apr 13 19:45  this_is_a_very_long_filename_for_testing_wildcard_pattern_matching.log
-rw-r--r-- 1 root root    0 Apr 13 19:45  user_data_2024.csv
-rw-r--r-- 1 root root    0 Apr 13 19:45  user_data_2025.csv
-rw-r--r-- 1 root root    0 Apr 13 19:45  user_data_2026.csv
```

### Langkah 5: Pembungkusan Data ke Format Kompresi tar.gz

**Perintah Eksekusi:**

```bash
echo ""
echo "=========================================="
echo "PEMBUATAN PAKET ARSIP TERKOMPRESI (TAR.GZ)"
echo "=========================================="
echo ""

cd "$BACKUP_DIR"

ARCHIVE_NAME="backup_archive_$TIMESTAMP.tar.gz"
ARCHIVE_PATH="$BACKUP_DIR/$ARCHIVE_NAME"

echo "Mengkalkulasi arsip: $ARCHIVE_NAME"
tar -czf "$ARCHIVE_PATH" "backup_$TIMESTAMP/"

if [ $? -eq 0 ]; then
    echo "Operasi pengarsipan telah sukses digenerasikan!"
    echo ""
    echo "=== METADATA ARSIP ==="
    ls -lh "$ARCHIVE_PATH"
    echo ""
    echo "=== STRUKTUR KONTEN ARSIP ==="
    tar -tzf "$ARCHIVE_PATH" | head -15
else
    echo "Error: Proses pembungkusan arsip gagal."
fi

cd ~/praktikum-os/week07-bash
```

**Hasil Keluaran (*Output*):**

```bash
==========================================
PEMBUATAN PAKET ARSIP TERKOMPRESI (TAR.GZ)
==========================================

Mengkalkulasi arsip: backup_archive_20260413_194511.tar.gz
Operasi pengarsipan telah sukses digenerasikan!

=== METADATA ARSIP ===
-rw-r--r-- 1 root root 376 Apr 13 19:47 /root/praktikum-os/week07-bash/tugas4-backup/backup_archive_20260413_194511.tar.gz

=== STRUKTUR KONTEN ARSIP ===
backup_20260413_194511/
backup_20260413_194511/user_data_2025.csv
backup_20260413_194511/this_is_a_very_long_filename_for_testing_wildcard_pattern_matching.log
backup_20260413_194511/user_data_2024.csv
backup_20260413_194511/access-log-01.txt
backup_20260413_194511/access-log-03.txt
backup_20260413_194511/laporan keuangan april.csv
backup_20260413_194511/user_data_2026.csv
backup_20260413_194511/config[production].ini
backup_20260413_194511/access-log-02.txt
backup_20260413_194511/log_backup_(server).txt
```

### Langkah 6: Pendokumentasian Skrip Perintah Ke File Riwayat

**Perintah Eksekusi:**

```bash
echo ""
echo "=========================================="
echo "DUMPING HISTORY PERINTAH TERMINAL"
echo "=========================================="
echo ""

cat <<EOF > riwayat-arsip.txt
==========================================
LOG EKSEKUSI TUGAS PRAKTIKUM 4
==========================================
Dicatat pada: $(date '+%Y-%m-%d %H:%M:%S')
Oleh: $(whoami)
Mesin Host: $(hostname)
Area Kerja: $(pwd)

------------------------------------------
KOMPILASI PERINTAH PRAKTIKAL
------------------------------------------

1. PENYAJIAN DUMMY FILES:
   touch "laporan keuangan april.csv"
   touch "backup server 2026.tar"
   touch "config[production].ini"
   touch "data[2026-04-10].json"

2. EDUKASI MEKANISME QUOTING:
   # Simulasi salah kaprah (Tanpa quote)
   ls -la laporan keuangan april.csv
   
   # Simulasi direkomendasikan (Dengan quote)
   ls -la "laporan keuangan april.csv"
   
   # Simulasi intervensi escape (\)
   ls config\[production\].ini

3. UJI COBA WILDCARD YANG AMAN:
   echo *.txt
   echo access-log-*.txt
   echo user_data_202?.csv

4. REPLIKASI FILE BERBANTUAN QUOTING:
   cp -v "laporan keuangan april.csv" "\$BACKUP_DIR/"
   cp -v config\[production\].ini "\$BACKUP_DIR/"

5. REPLIKASI BERBANTUAN STRING WILDCARD:
   cp -v access-log-*.txt "\$BACKUP_DIR/"

6. KOMPILASI KE FORMAT TAR.GZ:
   tar -czf "backup_archive_\$TIMESTAMP.tar.gz" "backup_\$TIMESTAMP/"

7. INVESTIGASI KONTEN PADA ARSIP:
   tar -tzf backup_archive_*.tar.gz

------------------------------------------
PENGINGAT TEKNIS: JANGAN LUPAKAN QUOTING
BILA MENANGANI VARIABEL YANG MEMUAT NAMA PATH/FILE!
------------------------------------------
Format Valid : cp "\$file_asli" "\$file_tujuan"
Format Rentan: cp \$file_asli \$file_tujuan

==========================================
EOF

cat riwayat-arsip.txt
```

**Hasil Keluaran (*Output*):**

```bash
==========================================
DUMPING HISTORY PERINTAH TERMINAL
==========================================

==========================================
LOG EKSEKUSI TUGAS PRAKTIKUM 4
==========================================
Dicatat pada: 2026-04-13 19:50:18
Oleh: root
Mesin Host: ubuntuser
Area Kerja: /root/praktikum-os/week07-bash

------------------------------------------
KOMPILASI PERINTAH PRAKTIKAL
------------------------------------------

1. PENYAJIAN DUMMY FILES:
   touch "laporan keuangan april.csv"
   touch "backup server 2026.tar"
   touch "config[production].ini"
   touch "data[2026-04-10].json"

2. EDUKASI MEKANISME QUOTING:
   # Simulasi salah kaprah (Tanpa quote)
   ls -la laporan keuangan april.csv

   # Simulasi direkomendasikan (Dengan quote)
   ls -la "laporan keuangan april.csv"

   # Simulasi intervensi escape (\)
   ls config\[production\].ini

3. UJI COBA WILDCARD YANG AMAN:
   echo *.txt
   echo access-log-*.txt
   echo user_data_202?.csv

4. REPLIKASI FILE BERBANTUAN QUOTING:
   cp -v "laporan keuangan april.csv" "$BACKUP_DIR/"
   cp -v config\[production\].ini "$BACKUP_DIR/"

5. REPLIKASI BERBANTUAN STRING WILDCARD:
   cp -v access-log-*.txt "$BACKUP_DIR/"

6. KOMPILASI KE FORMAT TAR.GZ:
   tar -czf "backup_archive_$TIMESTAMP.tar.gz" "backup_$TIMESTAMP/"

7. INVESTIGASI KONTEN PADA ARSIP:
   tar -tzf backup_archive_*.tar.gz

------------------------------------------
PENGINGAT TEKNIS: JANGAN LUPAKAN QUOTING
BILA MENANGANI VARIABEL YANG MEMUAT NAMA PATH/FILE!
------------------------------------------
Format Valid : cp "$file_asli" "$file_tujuan"
Format Rentan: cp $file_asli $file_tujuan

==========================================
```

### Langkah 7: Refleksi Deskriptif Mengenai Standar Quoting

**Perintah Eksekusi:**

```bash
echo ""
echo "=========================================="
echo "MANUAL PANDUAN URGENSI QUOTING"
echo "=========================================="
echo ""

cat <<'EOF' > refleksi-quoting.txt
==========================================
TINJAUAN TEKNIS: KENAPA QUOTING WAJIB DIAPLIKASIKAN?
==========================================

1. MENGATASI SPASI PADA NAMA FILE
   Jika terlewat: Bash akan membedah dan memisahkan identitas file.
   Ilustrasi: "laporan keuangan.txt" dibaca sebagai dua barang: 'laporan' dan 'keuangan.txt'
   Bila disertai tanda kutip: Kalimat dianggap sebagai satu objek solid.

2. MENGATASI KARAKTER PENGGANGGU ( *, ?, [, ], (, ), &, ;, $, !, `, | )
   Jika terlewat: Karakter-karakter ini akan dimanipulasi oleh logika shell.
   Ilustrasi: config[prod].ini → simbol [ ] memprovokasi pencarian wildcard system.
   Bila disertai tanda kutip: Bash melihatnya hanya sebagai teks polos (literal).

3. PENYELESAIAN VARIABEL DENGAN PATH ALAMAT
   Jika terlewat: Path direktori yang mengandung rongga spasi pasti gagal terbaca.
   Ilustrasi: $FILE="My Documents/file.txt"
   cp $FILE backup/ → operasi kandas karena terpotong di bagian "My".
   Bila disertai tanda kutip: cp "$FILE" backup/ → instruksi tersalurkan mulus.

4. BENTENG KEAMANAN SISTEM
   Jika terlewat: Mudah kebobolan oleh serangan command injection.
   Ilustrasi: nama_file="file.txt; rm -rf /"
   cat $nama_file → terminal berisiko menghapus paksa seluruh pondasi root!
   Bila disertai tanda kutip: cat "$nama_file" → hanya akan mencetak eror no such file, jauh lebih protektif.

5. INTEGRITAS COMMAND SUBSTITUTION
   Jika terlewat: Luaran command bisa berantakan akibat struktur spasi dari instruksi sebelumnya.
   Bila disertai tanda kutip: Susunan $(command) tetap utuh diikat dalam satu tali.

==========================================
PEDOMAN DASAR PEMROGRAMAN SHELL
==========================================

WAJIB membubuhkan quote ganda (" ") pada pemanggilan variabel:
   cp "$sumber" "$destinasi"
   echo "User Login: $USER"

Manfaatkan quote tunggal (' ') untuk mengikat elemen literal murni:
   echo 'Kode ini mencegah sistem berekspansi secara acak'

Manfaatkan escape (backslash) untuk anomali pada satu karakter saja:
   cp nama\ berjarak\ spasi.txt folder/

SELALU ujicoba wildcard via echo sebelum memerintahkan eksekusi riskan:
   echo rm *.log  (menilik siapa saja korbannya)
   rm *.log      (lakukan penghapusan)

==========================================
EOF

cat refleksi-quoting.txt
```

**Hasil Keluaran (*Output*):**

```bash
==========================================
MANUAL PANDUAN URGENSI QUOTING
==========================================

==========================================
TINJAUAN TEKNIS: KENAPA QUOTING WAJIB DIAPLIKASIKAN?
==========================================

1. MENGATASI SPASI PADA NAMA FILE
   Jika terlewat: Bash akan membedah dan memisahkan identitas file.
   Ilustrasi: "laporan keuangan.txt" dibaca sebagai dua barang: 'laporan' dan 'keuangan.txt'
   Bila disertai tanda kutip: Kalimat dianggap sebagai satu objek solid.

2. MENGATASI KARAKTER PENGGANGGU ( *, ?, [, ], (, ), &, ;, $, !, `, | )
   Jika terlewat: Karakter-karakter ini akan dimanipulasi oleh logika shell.
   Ilustrasi: config[prod].ini → simbol [ ] memprovokasi pencarian wildcard system.
   Bila disertai tanda kutip: Bash melihatnya hanya sebagai teks polos (literal).

3. PENYELESAIAN VARIABEL DENGAN PATH ALAMAT
   Jika terlewat: Path direktori yang mengandung rongga spasi pasti gagal terbaca.
   Ilustrasi: $FILE="My Documents/file.txt"
   cp $FILE backup/ → operasi kandas karena terpotong di bagian "My".
   Bila disertai tanda kutip: cp "$FILE" backup/ → instruksi tersalurkan mulus.

4. BENTENG KEAMANAN SISTEM
   Jika terlewat: Mudah kebobolan oleh serangan command injection.
   Ilustrasi: nama_file="file.txt; rm -rf /"
   cat $nama_file → terminal berisiko menghapus paksa seluruh pondasi root!
   Bila disertai tanda kutip: cat "$nama_file" → hanya akan mencetak eror no such file, jauh lebih protektif.

5. INTEGRITAS COMMAND SUBSTITUTION
   Jika terlewat: Luaran command bisa berantakan akibat struktur spasi dari instruksi sebelumnya.
   Bila disertai tanda kutip: Susunan $(command) tetap utuh diikat dalam satu tali.

==========================================
PEDOMAN DASAR PEMROGRAMAN SHELL
==========================================

WAJIB membubuhkan quote ganda (" ") pada pemanggilan variabel:
   cp "$sumber" "$destinasi"
   echo "User Login: $USER"

Manfaatkan quote tunggal (' ') untuk mengikat elemen literal murni:
   echo 'Kode ini mencegah sistem berekspansi secara acak'

Manfaatkan escape (backslash) untuk anomali pada satu karakter saja:
   cp nama\ berjarak\ spasi.txt folder/

SELALU ujicoba wildcard via echo sebelum memerintahkan eksekusi riskan:
   echo rm *.log  (menilik siapa saja korbannya)
   rm *.log      (lakukan penghapusan)

==========================================
```

### Langkah 8: Mengakumulasi Seluruh Pencapaian Tugas 4

**Perintah Eksekusi:**

```bash
echo ""
echo "=========================================="
echo "DASHBOARD PRESTASI TUGAS PRAKTIKUM 4"
echo "=========================================="
echo ""

echo "INDEKS FILE PERDANA (berlokasi di tugas4-sample):"
ls -1 tugas4-sample/ | head -10
echo "   ... dan sebagainya"
echo ""

echo "DIREKTORI PASCA-BACKUP:"
ls -1 "$BACKUP_SUBDIR/"
echo ""

echo "INSPEKSI KONTEN TAR.GZ:"
ls -lh "$BACKUP_DIR"/*.tar.gz
echo ""

echo "FILE REKAM JEJAK PERINTAH:"
ls -lh riwayat-arsip.txt
echo ""

echo "RANGKUMAN REFLEKTIF QUOTING:"
ls -lh refleksi-quoting.txt
echo ""

echo "=========================================="
echo "MISI TUGAS 4 DINYATAKAN RAMPUNG!"
echo "=========================================="
```

**Hasil Keluaran (*Output*):**

```bash
==========================================
DASHBOARD PRESTASI TUGAS PRAKTIKUM 4
==========================================

INDEKS FILE PERDANA (berlokasi di tugas4-sample):
access-log-01.txt
access-log-02.txt
access-log-03.txt
backup server 2026.tar
config[production].ini
data[2026-04-10].json
error-log-01.txt
error-log-02.txt
error-log-03.txt
laporan keuangan april.csv
   ... dan sebagainya

DIREKTORI PASCA-BACKUP:
access-log-01.txt
access-log-02.txt
access-log-03.txt
'config[production].ini'
'laporan keuangan april.csv'
'log_backup_(server).txt'
this_is_a_very_long_filename_for_testing_wildcard_pattern_matching.log
user_data_2024.csv
user_data_2025.csv
user_data_2026.csv

INSPEKSI KONTEN TAR.GZ:
-rw-r--r-- 1 root root 376 Apr 13 19:47 /root/praktikum-os/week07-bash/tugas4-backup/backup_archive_20260413_194511.tar.gz

FILE REKAM JEJAK PERINTAH:
-rw-r--r-- 1 root root 1.5K Apr 13 19:50 riwayat-arsip.txt

RANGKUMAN REFLEKTIF QUOTING:
-rw-r--r-- 1 root root 1.8K Apr 13 19:53 refleksi-quoting.txt

==========================================
MISI TUGAS 4 DINYATAKAN RAMPUNG!
==========================================
```