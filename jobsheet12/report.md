# jobsheet 12
**Nama:** Rasyiq Satrio M

**NIM:** 254107020079

**Kelas:** TI-1G

---

## PRAKTIKUM

## Praktikum 10.1 — Observasi Unit dan Layanan (*Services*)

### Langkah 1

**Instruksi:** Melakukan pengecekan terhadap seluruh layanan (*services*) yang saat ini berstatus aktif dan sedang berjalan di latar belakang.

```bash
# [Admin] - Memeriksa daftar layanan yang berstatus 'running'
root@ubuntu-server:~/praktikum-os/week12# systemctl list-units --type=service --state=running
  UNIT                           LOAD   ACTIVE SUB     DESCRIPTION         >
  cron.service                   loaded active running Regular background p>
  dbus.service                   loaded active running D-Bus System Message>
  getty@tty1.service             loaded active running Getty on tty1
  ModemManager.service           loaded active running Modem Manager
  multipathd.service             loaded active running Device-Mapper Multip>
  polkit.service                 loaded active running Authorization Manager
  rsyslog.service                loaded active running System Logging Servi>
  ssh.service                    loaded active running OpenBSD Secure Shell>
  systemd-journald.service       loaded active running Journal Service
  systemd-logind.service         loaded active running User Login Management
  systemd-networkd.service       loaded active running Network Configuration
  systemd-resolved.service       loaded active running Network Name Resolut>
  systemd-udevd.service          loaded active running Rule-based Manager f>
  udisks2.service                loaded active running Disk Manager
  unattended-upgrades.service    loaded active running Unattended Upgrades >
  user@0.service                 loaded active running User Manager for UID>
  user@1000.service              loaded active running User Manager for UID>
  virtualbox-guest-utils.service loaded active running Virtualbox guest uti>

Legend: LOAD   → Reflects whether the unit definition was properly loaded.
        ACTIVE → The high-level unit activation state, i.e. generalization >
        SUB    → The low-level unit activation state, values depend on unit>

18 loaded units listed.

```

### Langkah 2

**Instruksi:** Menampilkan daftar konfigurasi file unit khusus layanan untuk melihat preferensi *startup* (misal: *enabled* atau *disabled*).

```bash
# [Admin] - Melihat 30 entri pertama dari daftar file unit layanan
root@ubuntu-server:~/praktikum-os/week12# systemctl list-unit-files --type=service | head -30
UNIT FILE                                    STATE           PRESET
apparmor.service                             enabled         enabled
apport-autoreport.service                    static          -
apport-coredump-hook@.service                static          -
apport-forward@.service                      static          -
apport.service                               enabled         enabled
apt-daily-upgrade.service                    static          -
apt-daily.service                            static          -
apt-news.service                             static          -
autovt@.service                              alias           -
blk-availability.service                     enabled         enabled
bolt.service                                 static          -
cloud-config.service                         enabled         enabled
cloud-final.service                          enabled         enabled
cloud-init-hotplugd.service                  static          -
cloud-init-local.service                     enabled         enabled
cloud-init.service                           enabled         enabled
console-getty.service                        disabled        disabled
console-setup.service                        enabled         enabled
container-getty@.service                     static          -
cron.service                                 enabled         enabled
cryptdisks-early.service                     masked          enabled
cryptdisks.service                           masked          enabled
dbus-org.freedesktop.hostname1.service       alias           -
dbus-org.freedesktop.locale1.service         alias           -
dbus-org.freedesktop.login1.service          alias           -
dbus-org.freedesktop.ModemManager1.service   alias           -
dbus-org.freedesktop.resolve1.service        alias           -
dbus-org.freedesktop.thermald.service        alias           -
dbus-org.freedesktop.timedate1.service       alias           -

```

### Langkah 3

**Instruksi:** Menganalisis durasi waktu *booting* sistem dan melacak layanan mana yang paling membebani proses inisialisasi.

```bash
# [Admin] - Meninjau statistik waktu booting
root@ubuntu-server:~/praktikum-os/week12# systemd-analyze
Startup finished in 13.124s (kernel) + 12.424s (userspace) = 25.549s
graphical.target reached after 12.299s in userspace.

# [Admin] - Mengidentifikasi 15 layanan dengan durasi muat terlama
root@ubuntu-server:~/praktikum-os/week12# systemd-analyze blame | head -15
6.582s dev-sda2.device
6.381s snapd.seeded.service
6.117s snapd.service
5.268s dpkg-db-backup.service
3.931s systemd-networkd.service
2.582s ModemManager.service
2.426s polkit.service
2.308s udisks2.service
2.244s grub-common.service
2.113s rsyslog.service
2.039s virtualbox-guest-utils.service
1.978s apparmor.service
1.869s systemd-networkd-wait-online.service
1.617s apport.service
1.344s dbus.service

```

### Tantangan 10.1

**Instruksi:** Filter tiga layanan dengan beban *startup* terberat. Identifikasi fungsinya masing-masing menggunakan perintah `systemctl cat`.

```bash
# [Admin] - Memfilter 3 besar penyebab lambatnya proses boot
root@ubuntu-server:~/praktikum-os/week12# systemd-analyze blame | head -3
6.582s dev-sda2.device
6.381s snapd.seeded.service
6.117s snapd.service

# [Admin] - Melakukan inspeksi definisi layanan
root@ubuntu-server:~/praktikum-os/week12# systemctl cat dev-sda2.device 2>/dev/null || echo "Unit device ini tidak memiliki file terpisah"
Unit device ini tidak memiliki file terpisah

root@ubuntu-server:~/praktikum-os/week12# systemctl cat snapd.seeded.service
# /usr/lib/systemd/system/snapd.seeded.service
[Unit]
Description=Wait until snapd is fully seeded
After=snapd.socket
Requires=snapd.socket

[Service]
Type=oneshot
ExecStart=/usr/bin/snap wait system seed.loaded
RemainAfterExit=true

[Install]
WantedBy=multi-user.target cloud-final.service
# This is handled special in snapd
# X-Snapd-Snap: do-not-start

root@ubuntu-server:~/praktikum-os/week12# systemctl cat snapd.service | head -15
# /usr/lib/systemd/system/snapd.service
[Unit]
Description=Snap Daemon
After=snapd.socket
After=time-set.target
After=snapd.mounts.target
Wants=time-set.target
Wants=snapd.mounts.target
Requires=snapd.socket
OnFailure=snapd.failure.service
# This is handled by snapd
# X-Snapd-Snap: do-not-start

[Service]
# Disabled because it breaks lxd

```

**Hasil Analisis:**

1. **`dev-sda2.device` (6.582s)** Komponen ini bukanlah sebuah program layanan, melainkan entitas unit yang melambangkan perangkat keras partisi memori (`sda2`). Tingginya waktu inisialisasi pada sektor ini secara umum diakibatkan oleh proses pengaitan sistem file (*mounting*), pemindaian integritas disk (*fsck*), atau kapasitas penyimpanan yang memakan waktu lama untuk dikenali oleh kernel.
2. **`snapd.seeded.service` (6.381s)** Layanan ini difungsikan untuk menahan antrean proses hingga *daemon* paket *Snap* selesai melakukan *seeding* (persiapan fundamental). Proses ini membutuhkan durasi yang lumayan karena sistem perlu mengekstraksi, memvalidasi dependensi inti, serta menautkan pustaka dasar aplikasi *Snap*.
3. **`snapd.service` (6.117s)** Ini merupakan *daemon* utama pengelola keseluruhan paket perangkat lunak berformat *Snap*. Keterlambatan *boot* diakibatkan oleh siklus pemeriksaan pembaruan, inisialisasi lingkungan *container* terisolasi untuk tiap-tiap aplikasi, serta pembentukan jalur integrasi dengan pangkalan data *Snap Store*.

## Praktikum 10.2 — Manajemen Layanan dengan `systemctl`

### Langkah 1

**Instruksi:** Memeriksa status, kondisi operasional, dan parameter *auto-start* dari layanan SSH.

```bash
# [Admin] - Pengecekan komprehensif pada layanan SSH
root@ubuntu-server:~/praktikum-os/week12# systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: >
     Active: active (running) since Sun 2026-05-17 15:02:25 UTC; 23min ago
TriggeredBy: ● ssh.socket
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 857 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCC>
   Main PID: 869 (sshd)
      Tasks: 6 (limit: 4008)
     Memory: 8.4M (peak: 23.5M)
        CPU: 8.267s
     CGroup: /system.slice/ssh.service
             ├─ 869 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startup>
             ├─1133 "sshd: rasyiqtaps [priv]"
             ├─1135 "sshd: rasyiqtaps@pts/1"
             ├─1136 -bash
             ├─1145 sudo su
             └─1146 sudo su

May 17 15:02:24 ubuntu-server systemd[1]: Starting ssh.service - OpenBSD Secure>
May 17 15:02:25 ubuntu-server sshd[869]: Server listening on 0.0.0.0 port 22.
May 17 15:02:25 ubuntu-server sshd[869]: Server listening on :: port 22.
May 17 15:02:25 ubuntu-server systemd[1]: Started ssh.service - OpenBSD Secure >
May 17 15:08:00 ubuntu-server sshd[1133]: Accepted password for rasyiqtaps from 10.>
May 17 15:08:20 ubuntu-server sudo[1145]:   rasyiqtaps : TTY=pts/1 ; PWD=/home/rasyiqtaps>
May 17 15:08:20 ubuntu-server sudo[1145]: pam_unix(sudo:session): session opene>
May 17 15:08:20 ubuntu-server su[1147]: (to root) root on pts/2
May 17 15:08:20 ubuntu-server su[1147]: pam_unix(su:session): session opened fo>

root@ubuntu-server:~/praktikum-os/week12# systemctl is-active ssh
active
root@ubuntu-server:~/praktikum-os/week12# systemctl is-enabled ssh
enabled

```

### Langkah 2

**Instruksi:** Melakukan prosedur *restart* untuk menyegarkan konfigurasi layanan SSH.

```bash
# [Admin] - Memulai ulang SSH dan memantau status terbarunya
root@ubuntu-server:~/praktikum-os/week12# sudo systemctl restart ssh
root@ubuntu-server:~/praktikum-os/week12# systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: >
     Active: active (running) since Sun 2026-05-17 15:15:21 UTC; 12s ago
TriggeredBy: ● ssh.socket
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 1350 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUC>
   Main PID: 1352 (sshd)
      Tasks: 6 (limit: 4008)
     Memory: 8.6M (peak: 23.5M)
        CPU: 279ms
     CGroup: /system.slice/ssh.service
             ├─1133 "sshd: rasyiqtaps [priv]"
             ├─1135 "sshd: rasyiqtaps@pts/1"
             ├─1136 -bash
             ├─1145 sudo su
             ├─1146 sudo su
             └─1352 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startup>

May 17 15:15:21 ubuntu-server systemd[1]: ssh.service: This usually indicates u>
May 17 15:15:21 ubuntu-server systemd[1]: ssh.service: Found left-over process >
May 17 15:15:21 ubuntu-server systemd[1]: ssh.service: This usually indicates u>
May 17 15:15:21 ubuntu-server sshd[1352]: Server listening on 0.0.0.0 port 22.
May 17 15:15:21 ubuntu-server sshd[1352]: Server listening on :: port 22.
May 17 15:15:21 ubuntu-server systemd[1]: Started ssh.service - OpenBSD Secure >

```

### Langkah 3

**Instruksi:** Melacak diagram ketergantungan komponen (*dependencies*) yang disyaratkan oleh layanan SSH sebelum dapat beroperasi.

```bash
# [Admin] - Melihat rincian dependensi layanan ssh
root@ubuntu-server:~/praktikum-os/week12# systemctl list-dependencies ssh
ssh.service
● ├─-.mount
● ├─ssh.socket
● ├─system.slice
● └─sysinit.target
●   ├─apparmor.service
●   ├─blk-availability.service
●   ├─dev-hugepages.mount
●   ├─dev-mqueue.mount
●   ├─finalrd.service
●   ├─keyboard-setup.service
●   ├─kmod-static-nodes.service
○   ├─ldconfig.service
●   ├─lvm2-lvmpolld.socket
●   ├─lvm2-monitor.service
●   ├─multipathd.service
○   ├─open-iscsi.service
●   ├─plymouth-read-write.service
●   ├─plymouth-start.service
●   ├─proc-sys-fs-binfmt_misc.automount
●   ├─setvtrgb.service
●   ├─sys-fs-fuse-connections.mount
●   ├─sys-kernel-config.mount
●   ├─sys-kernel-debug.mount
●   ├─sys-kernel-tracing.mount
○   ├─systemd-ask-password-console.path
●   ├─systemd-binfmt.service
○   ├─systemd-firstboot.service
○   ├─systemd-hwdb-update.service
○   ├─systemd-journal-catalog-update.service
●   ├─systemd-journal-flush.service
●   ├─systemd-journald.service
○   ├─systemd-machine-id-commit.service
●   ├─systemd-modules-load.service
○   ├─systemd-pcrmachine.service
○   ├─systemd-pcrphase-sysinit.service
○   ├─systemd-pcrphase.service
○   ├─systemd-pstore.service
●   ├─systemd-random-seed.service
○   ├─systemd-repart.service
●   ├─systemd-resolved.service
●   ├─systemd-sysctl.service

```

### Langkah 4

**Instruksi:** Melakukan audit terhadap unit-unit yang gagal dimuat selama proses operasi sistem.

```bash
# [Admin] - Mengecek layanan yang berstatus gagal (failed)
root@ubuntu-server:~/praktikum-os/week12# systemctl --failed
  UNIT            LOAD   ACTIVE SUB    DESCRIPTION
● quotaon.service loaded failed failed Enable File System Quotas

Legend: LOAD   → Reflects whether the unit definition was properly loaded.
        ACTIVE → The high-level unit activation state, i.e. generalization >
        SUB    → The low-level unit activation state, values depend on unit>

1 loaded units listed.

```

### Tantangan 10.2

**Instruksi:** Merancang sebuah skrip berbasis Bash untuk mengotomatisasi pengecekan status dari beberapa layanan krusial sekaligus, lalu menyimpan laporannya ke dalam log.

```bash
root@ubuntu-server:~/praktikum-os/week12# cd ~/lab-os/chapter10-services
root@ubuntu-server:~/lab-os/chapter10-services# nano cek-layanan.sh

# --- [Isi Script: cek-layanan.sh] ---
#!/bin/bash

# =========================================================
# Utilitas Pengecekan Status Layanan Terpadu
# Format Eksekusi: ./cek-layanan.sh [nama-file-daftar]
# =========================================================

# Mendefinisikan file input dan output
INPUT_FILE="${1:-daftar-layanan.txt}"
OUTPUT_FILE="laporan-layanan.log"

# Melakukan validasi eksistensi berkas input
if [ ! -f "$INPUT_FILE" ]; then
    echo "Peringatan: Berkas '$INPUT_FILE' tidak dapat ditemukan!"
    echo "Harap buat berkas yang memuat daftar layanan terlebih dahulu."
    exit 1
fi

# Menyusun format kop laporan
echo "=====================================" > "$OUTPUT_FILE"
echo "  REKAPITULASI STATUS LAYANAN AKTIF" >> "$OUTPUT_FILE"
echo "  Waktu: $(date '+%Y-%m-%d %H:%M:%S')" >> "$OUTPUT_FILE"
echo "=====================================" >> "$OUTPUT_FILE"
echo "" >> "$OUTPUT_FILE"

# Iterasi pembacaan setiap entri layanan dari file
while read -r service; do
    if [ -z "$service" ]; then
        continue
    fi
    
    # Mengevaluasi kondisi 'running' pada tiap layanan
    if systemctl is-active --quiet "$service"; then
        status="ACTIVE"
    else
        status="INACTIVE"
    fi
    
    # Melakukan pencatatan log (output ke file & terminal)
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] Layanan $service: $status" | tee -a "$OUTPUT_FILE"
    
done < "$INPUT_FILE"

echo -e "\nProses pencatatan rampung. Log tersimpan di: $OUTPUT_FILE"
# ------------------------------------

# [Admin] - Menyusun hak eksekusi dan file parameter
root@ubuntu-server:~/lab-os/chapter10-services# chmod +x cek-layanan.sh
root@ubuntu-server:~/lab-os/chapter10-services# nano daftar-layanan.txt
# (Mengisi file dengan: ssh, cron, rsyslog, cups, apache2, bash)

# [Admin] - Menjalankan utilitas pengecekan
root@ubuntu-server:~/lab-os/chapter10-services# ./cek-layanan.sh daftar-layanan.txt
[2026-05-17 15:23:46] Layanan ssh: ACTIVE
[2026-05-17 15:23:46] Layanan cron: ACTIVE
[2026-05-17 15:23:46] Layanan rsyslog: ACTIVE
[2026-05-17 15:23:46] Layanan cups: INACTIVE
[2026-05-17 15:23:46] Layanan apache2: INACTIVE

Proses pencatatan rampung. Log tersimpan di: laporan-layanan.log

# [Admin] - Memverifikasi isi berkas log
root@ubuntu-server:~/lab-os/chapter10-services# cat laporan-layanan.log
=====================================
  REKAPITULASI STATUS LAYANAN AKTIF
  Waktu: 2026-05-17 15:23:46
=====================================

[2026-05-17 15:23:46] Layanan ssh: ACTIVE
[2026-05-17 15:23:46] Layanan cron: ACTIVE
[2026-05-17 15:23:46] Layanan rsyslog: ACTIVE
[2026-05-17 15:23:46] Layanan cups: INACTIVE
[2026-05-17 15:23:46] Layanan apache2: INACTIVE

```

## Praktikum 10.3 — Konfigurasi Berkas Unit Kustom

### Langkah 1

**Instruksi:** Menyusun struktur berkas HTML statis yang nantinya akan disajikan lewat server HTTP kustom.

```bash
# [Admin] - Menyiapkan fondasi website sederhana
root@ubuntu-server:~/praktikum-os/week12# cd /root/lab-os/chapter10-services
root@ubuntu-server:~/lab-os/chapter10-services# mkdir -p situs-demo
root@ubuntu-server:~/lab-os/chapter10-services# nano situs-demo/index.html

# --- [Isi File: index.html] ---
# <!DOCTYPE html>
# <html>
# <head>
#     <title>Demo Web Server</title>
#     <meta charset="utf-8">
# </head>
# <body>
#     <h1>Halo dari layanan systemd kustom!</h1>
#     <p>Layanan ini dibuat pada praktek Bab 10.</p>
#     <hr>
#     <p><small>Server berjalan sebagai service systemd</small></p>
# </body>
# </html>
# ------------------------------

```

### Langkah 2

**Instruksi:** Mempersiapkan *script wrapper* untuk memanggil modul HTTP bawaan Python.

```bash
# [Admin] - Merancang skrip aktivasi daemon
root@ubuntu-server:~/lab-os/chapter10-services# nano ~/lab-os/chapter10-services/jalankan-server.sh

# --- [Isi File: jalankan-server.sh] ---
# #!/bin/bash
# DIREKTORI="$HOME/lab-os/chapter10-services/situs-demo"
# PORT=9090
# 
# echo "Memulai inisiasi server di port $PORT..."
# echo "Merutekan akses ke direktori: $DIREKTORI"
# 
# exec python3 -m http.server $PORT --directory "$DIREKTORI"
# --------------------------------------

# [Admin] - Memberikan izin eksekusi pada skrip
root@ubuntu-server:~/lab-os/chapter10-services# chmod +x ~/lab-os/chapter10-services/jalankan-server.sh

```

### Langkah 3

**Instruksi:** Mendeklarasikan berkas konfigurasi *systemd* khusus (`.service`) untuk meregistrasi web server buatan sendiri ke dalam sistem.

```bash
# [Admin] - Menyusun berkas unit layanan
root@ubuntu-server:~/lab-os/chapter10-services# nano ~/lab-os/chapter10-services/demo-web.service

# --- [Isi File: demo-web.service] ---
# [Unit]
# Description=Demo Web Server Praktek Bab 10
# After=network.target
# 
# [Service]
# Type=simple
# User=rasyiqtaps
# ExecStart=/root/lab-os/chapter10-services/jalankan-server.sh
# Restart=on-failure
# RestartSec=3s
# 
# [Install]
# WantedBy=multi-user.target
# ------------------------------------

# [Admin] - Mengintegrasikan layanan ke ruang sistem dan me-reload daemon
root@ubuntu-server:~/lab-os/chapter10-services# sudo cp ~/lab-os/chapter10-services/demo-web.service /etc/systemd/system/demo-web.service
root@ubuntu-server:~/lab-os/chapter10-services# sudo systemctl daemon-reload

```

### Langkah 4

**Instruksi:** Menghidupkan layanan baru dan menguji aksesibilitas server web via utilitas `curl`.

```bash
# [Admin] - Meluncurkan layanan dan memvalidasi akses HTTP
root@ubuntu-server:~/lab-os/chapter10-services# sudo systemctl start demo-web
root@ubuntu-server:~/lab-os/chapter10-services# curl http://localhost:9090
<!DOCTYPE html>
<html>
<head>
    <title>Demo Web Server</title>
    <meta charset="utf-8">
</head>
<body>
    <h1>Halo dari layanan systemd kustom!</h1>
    <p>Layanan ini dibuat pada praktek Bab 10.</p>
    <hr>
    <p><small>Server berjalan sebagai service systemd</small></p>
</body>
</html>

```

### Langkah 5

**Instruksi:** Melakukan simulasi matinya server (*crash*) secara ekstrem untuk memvalidasi fitur otomatisasi *restart* milik *systemd*.

```bash
# [Admin] - Mengisolasi PID layanan untuk dimatikan paksa
root@ubuntu-server:~/lab-os/chapter10-services# systemctl status demo-web | grep "Main PID"
   Main PID: 2394 (python3)
root@ubuntu-server:~/lab-os/chapter10-services# sudo kill -9 $(systemctl show demo-web --property=MainPID --value)

# [Admin] - Menunda pengecekan dan memverifikasi status pemulihan layanan
root@ubuntu-server:~/lab-os/chapter10-services# sleep 5
root@ubuntu-server:~/lab-os/chapter10-services# systemctl status demo-web
● demo-web.service - Demo Web Server Praktek Bab 10
     Loaded: loaded (/etc/systemd/system/demo-web.service; disabled; preset>
     Active: active (running) since Sun 2026-05-17 15:35:41 UTC; 11s ago
   Main PID: 2426 (python3)
      Tasks: 1 (limit: 4008)
     Memory: 9.3M (peak: 9.6M)
        CPU: 327ms
     CGroup: /system.slice/demo-web.service
             └─2426 python3 -m http.server 9090

May 17 15:35:41 ubuntu-server systemd[1]: demo-web.service: Scheduled restart j>
May 17 15:35:41 ubuntu-server systemd[1]: Started demo-web.service - Demo Web S>
May 17 15:35:41 ubuntu-server jalankan-server.sh[2426]: Memulai inisiasi server di port >
May 17 15:35:41 ubuntu-server jalankan-server.sh[2426]: Merutekan akses ke direktori: /ro>

```

### Langkah 6

**Instruksi:** Melakukan disabilitas layanan dan membersihkan sisa pendaftaran file unit dari repositori *systemd*.

```bash
# [Admin] - Membekukan dan menghapus dependensi layanan kustom
root@ubuntu-server:~/lab-os/chapter10-services# sudo systemctl disable --now demo-web
root@ubuntu-server:~/lab-os/chapter10-services# sudo rm /etc/systemd/system/demo-web.service
root@ubuntu-server:~/lab-os/chapter10-services# sudo systemctl daemon-reload

```

### Tantangan 10.3

**Instruksi:** Merevisi parameter konfigurasi dengan mengubah otorisasi eksekusi ke akun `root`, merelokasi parameter *port* ke variabel lingkungan (*environment*), serta menaikkan jeda waktu pemulihan.

```bash
# [Admin] - Membuka editor guna memodifikasi rancangan layanan
root@ubuntu-server:~/lab-os/chapter10-services# sudo nano /etc/systemd/system/demo-web.service

# --- [Modifikasi Isi File: demo-web.service] ---
# [Unit]
# Description=Demo Web Server Praktek Bab 10 (Modified)
# After=network.target
# 
# [Service]
# Type=simple
# User=root
# WorkingDirectory=/root/lab-os/chapter10-services/situs-demo
# Environment="PORT=9091"
# ExecStart=/usr/bin/python3 -m http.server ${PORT}
# Restart=on-failure
# RestartSec=10s
# 
# [Install]
# WantedBy=multi-user.target
# -----------------------------------------------

# [Admin] - Memuat ulang repositori dan mengaktifkan perombakan layanan
root@ubuntu-server:~/lab-os/chapter10-services# sudo systemctl daemon-reload
root@ubuntu-server:~/lab-os/chapter10-services# sudo systemctl enable --now demo-web
Created symlink /etc/systemd/system/multi-user.target.wants/demo-web.service → /etc/systemd/system/demo-web.service.

# [Admin] - Validasi port web baru (9091)
root@ubuntu-server:~/lab-os/chapter10-services# systemctl status demo-web
● demo-web.service - Demo Web Server Praktek Bab 10 (Modified)
     Loaded: loaded (/etc/systemd/system/demo-web.service; enabled; preset: >
     Active: active (running) since Sun 2026-05-17 15:42:34 UTC; 8s ago
   Main PID: 3103 (python3)
      Tasks: 1 (limit: 4008)
     Memory: 9.3M (peak: 9.5M)
        CPU: 200ms
     CGroup: /system.slice/demo-web.service
             └─3103 /usr/bin/python3 -m http.server 9091

root@ubuntu-server:~/lab-os/chapter10-services# curl http://localhost:9091
<!DOCTYPE html>
<html>
<head>
    <title>Demo Web Server</title>
    <meta charset="utf-8">
</head>
<body>
    <h1>Halo dari layanan systemd kustom!</h1>
    <p>Layanan ini dibuat pada praktek Bab 10.</p>
    <hr>
    <p><small>Server berjalan sebagai service systemd</small></p>
</body>
</html>

# [Admin] - Tahap dekonstruksi setelah pengujian selesai
root@ubuntu-server:~/lab-os/chapter10-services# systemctl is-enabled demo-web
enabled
root@ubuntu-server:~/lab-os/chapter10-services# sudo systemctl disable --now demo-web
root@ubuntu-server:~/lab-os/chapter10-services# sudo rm /etc/systemd/system/demo-web.service
root@ubuntu-server:~/lab-os/chapter10-services# sudo systemctl daemon-reload
Removed "/etc/systemd/system/multi-user.target.wants/demo-web.service".

```

## Praktikum 10.4 — Manajemen Log (*Journalctl*)

### Langkah 1

**Instruksi:** Memeriksa log aktivitas yang direkam dari layanan SSH pada kurun waktu satu jam ke belakang.

```bash
# [Admin] - Memfilter log historis layanan SSH
root@ubuntu-server:~/praktikum-os/week12# journalctl -u ssh --since "1 hour ago" --no-pager
-- No entries --

```

### Langkah 2

**Instruksi:** Menyaring pesan galat (*error*) berprioritas tinggi yang direkam sejak *boot* terakhir sistem.

```bash
# [Admin] - Mengekstrak log dengan flag prioritas 'err'
root@ubuntu-server:~/praktikum-os/week12# journalctl -b -p err --no-pager
May 17 15:02:16 ubuntu-server systemd[1]: Failed to start quotaon.service - Enable File System Quotas.
May 17 15:02:24 ubuntu-server kernel: vmwgfx 0000:00:02.0: [drm] *ERROR* vmwgfx seems to be running on an unsupported hypervisor.
May 17 15:02:24 ubuntu-server kernel: vmwgfx 0000:00:02.0: [drm] *ERROR* This configuration is likely broken.
May 17 15:02:24 ubuntu-server kernel: vmwgfx 0000:00:02.0: [drm] *ERROR* Please switch to a supported graphics device to avoid problems.
May 17 15:07:09 ubuntu-server login[907]: PAM unable to dlopen(pam_lastlog.so): /usr/lib/security/pam_lastlog.so: cannot open shared object file: No such file or directory
May 17 15:07:09 ubuntu-server login[907]: PAM adding faulty module: pam_lastlog.so
May 17 15:32:35 ubuntu-server (python3)[1603]: demo-web.service: Changing to the requested working directory failed: No such file or directory
May 17 15:32:39 ubuntu-server (python3)[1605]: demo-web.service: Changing to the requested working directory failed: No such file or directory
... (Log error direktori lainnya dipersingkat untuk kejelasan) ...
May 17 15:34:49 ubuntu-server (python3)[1695]: demo-web.service: Changing to the requested working directory failed: No such file or directory
May 17 15:34:52 ubuntu-server (python3)[1698]: demo-web.service: Changing to the requested working directory failed: No such file or directory

```

### Langkah 3

**Instruksi:** Memantau rekaman log layanan SSH secara aktual (*real-time*).

```bash
# [Admin] - Menyaksikan aliran data journal daemon menggunakan parameter 'follow'
root@ubuntu-server:~/praktikum-os/week12# journalctl -u ssh -f
May 17 15:15:21 ubuntu-server systemd[1]: ssh.service: This usually indicates unclean termination of a previous run, or service implementation deficiencies.
May 17 15:15:21 ubuntu-server systemd[1]: ssh.service: Found left-over process 1136 (bash) in control group while starting unit. Ignoring.
May 17 15:15:21 ubuntu-server systemd[1]: ssh.service: This usually indicates unclean termination of a previous run, or service implementation deficiencies.
May 17 15:15:21 ubuntu-server systemd[1]: ssh.service: Found left-over process 1145 (sudo) in control group while starting unit. Ignoring.
May 17 15:15:21 ubuntu-server systemd[1]: ssh.service: This usually indicates unclean termination of a previous run, or service implementation deficiencies.
May 17 15:15:21 ubuntu-server systemd[1]: ssh.service: Found left-over process 1146 (sudo) in control group while starting unit. Ignoring.
May 17 15:15:21 ubuntu-server systemd[1]: ssh.service: This usually indicates unclean termination of a previous run, or service implementation deficiencies.
May 17 15:15:21 ubuntu-server sshd[1352]: Server listening on 0.0.0.0 port 22.
May 17 15:15:21 ubuntu-server sshd[1352]: Server listening on :: port 22.
May 17 15:15:21 ubuntu-server systemd[1]: Started ssh.service - OpenBSD Secure Shell server.

```

### Langkah 4

**Instruksi:** Mengekstraksi rekaman aktivitas pada suatu kurun waktu tertentu menuju sebuah dokumen log arsip, lalu menghitung baris pesan gagal.

```bash
# [Admin] - Mendesiminasikan data output menuju file teks statis
root@ubuntu-server:~/praktikum-os/week12# cd ~/lab-os/chapter10-services
root@ubuntu-server:~/lab-os/chapter10-services# journalctl -u ssh --since today --no-pager > log-ssh-hari-ini.txt

# [Admin] - Menghitung jumlah entri serta menelusuri kata kunci peringatan
root@ubuntu-server:~/lab-os/chapter10-services# wc -l log-ssh-hari-ini.txt
53 log-ssh-hari-ini.txt
root@ubuntu-server:~/lab-os/chapter10-services# grep -i "error\|failed" log-ssh-hari-ini.txt | head -20
May 17 15:46:34 ubuntu-server sshd[3267]: Failed password for rasyiqtaps from 10.0.2.2 port 62943 ssh2

```

### Tantangan 10.4

**Instruksi:** Merangkum sepuluh insiden *error* paling membanjiri log pada layanan SSH dalam tempo 24 jam terakhir secara agregat.

```bash
# [Admin] - Menyortir error tertinggi berdasarkan volume frekuensi pemunculan
root@ubuntu-server:~/lab-os/chapter10-services# journalctl -u ssh -p err --since "24 hours ago" --no-pager | \
> sort | uniq -c | sort -rn | head -10
      1 -- No entries --

```

## Praktikum 10.5 — Modifikasi Parameter Konfigurasi Jaringan (Layanan SSH)

### Langkah 1

**Instruksi:** Memverifikasi pengaturan port pendengar (*listening port*) yang saat ini dialokasikan pada *daemon* SSH.

```bash
# [Admin] - Melakukan deteksi alokasi port di berkas konfigurasi vs layanan nyata
root@ubuntu-server:~/praktikum-os/week12# sudo grep -n "^Port\|^#Port" /etc/ssh/sshd_config
1:Port 22
root@ubuntu-server:~/praktikum-os/week12# ss -tlnp | grep ssh
LISTEN 0      4096         0.0.0.0:22        0.0.0.0:* users:(("sshd",pid=1352,fd=3),("systemd",pid=1,fd=148))
LISTEN 0      4096            [::]:22           [::]:* users:(("sshd",pid=1352,fd=4),("systemd",pid=1,fd=149))

```

### Langkah 2

**Instruksi:** Memodifikasi identitas pelabuhan jaringan (*port*) menjadi format yang tidak standar, dengan landasan perombakan berbasis skrip `sed`.

```bash
# [Admin] - Menduplikasi file sumber demi antisipasi galat, lalu melakukan manipulasi teks
root@ubuntu-server:~/praktikum-os/week12# sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup.week12
root@ubuntu-server:~/praktikum-os/week12# sudo sed -i 's/^#Port 22/Port 2222/' /etc/ssh/sshd_config
root@ubuntu-server:~/praktikum-os/week12# grep "^Port" /etc/ssh/sshd_config
Port 22

```

### Langkah 3

**Instruksi:** Mengevaluasi validitas sintaksis pada revisi file konfigurasi, kemudian memerintahkan sistem untuk mencerna ulang parameter tersebut.

```bash
# [Admin] - Menganalisis file sshd_config secara sintaksis sebelum me-restart layanan
root@ubuntu-server:~/praktikum-os/week12# sudo sshd -t
root@ubuntu-server:~/praktikum-os/week12# echo "Status kode evaluasi sshd -t: $?"
Status kode evaluasi sshd -t: 0

# [Admin] - Menginisialisasi ulang sistem dan memeriksa rincian PID
root@ubuntu-server:~/praktikum-os/week12# sudo systemctl restart ssh
root@ubuntu-server:~/praktikum-os/week12# systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: e>
     Active: active (running) since Sun 2026-05-17 15:49:18 UTC; 6s ago
TriggeredBy: ● ssh.socket
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 3591 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCC>
   Main PID: 3593 (sshd)
      Tasks: 6 (limit: 4008)
     Memory: 8.6M (peak: 23.7M)
        CPU: 541ms
     CGroup: /system.slice/ssh.service
             ├─3267 "sshd: rasyiqtaps [priv]"
             ├─3278 "sshd: rasyiqtaps@pts/1"
             ├─3279 -bash
             ├─3288 sudo su
             ├─3290 sudo su
             └─3593 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

May 17 15:49:18 ubuntu-server systemd[1]: ssh.service: This usually indicates un>
May 17 15:49:18 ubuntu-server systemd[1]: ssh.service: Found left-over process 3>
May 17 15:49:18 ubuntu-server sshd[3593]: Server listening on 0.0.0.0 port 22.
May 17 15:49:18 ubuntu-server systemd[1]: Started ssh.service - OpenBSD Secure S>

```

### Langkah 4

**Instruksi:** Pengecekan silang terhadap status port *listening* layanan di *socket* TCP jaringan menggunakan `ss`.

```bash
# [Admin] - Melakukan perekaman output monitoring port
root@ubuntu-server:~/praktikum-os/week12# ss -tlnp | grep ssh
LISTEN 0      4096         0.0.0.0:22        0.0.0.0:* users:(("sshd",pid=3593,fd=3),("systemd",pid=1,fd=148))
LISTEN 0      4096            [::]:22           [::]:* users:(("sshd",pid=3593,fd=4),("systemd",pid=1,fd=149))
root@ubuntu-server:~/praktikum-os/week12# ss -tlnp | grep ssh > ~/lab-os/chapter10-services/bukti-port-ssh.txt
root@ubuntu-server:~/praktikum-os/week12# cat ~/lab-os/chapter10-services/bukti-port-ssh.txt
LISTEN 0      4096         0.0.0.0:22        0.0.0.0:* users:(("sshd",pid=3593,fd=3),("systemd",pid=1,fd=148))
LISTEN 0      4096            [::]:22           [::]:* users:(("sshd",pid=3593,fd=4),("systemd",pid=1,fd=149))

```

### Langkah 5

**Instruksi:** Melakukan restorasi data menggunakan berkas cadangan (*backup*) untuk mengembalikan parameter sistem ke kondisi bawaan awal.

```bash
# [Admin] - Memuat ulang versi cadangan SSH config
root@ubuntu-server:~/praktikum-os/week12# sudo cp /etc/ssh/sshd_config.backup.week12 /etc/ssh/sshd_config
root@ubuntu-server:~/praktikum-os/week12# sudo sshd -t
root@ubuntu-server:~/praktikum-os/week12# sudo systemctl restart ssh
root@ubuntu-server:~/praktikum-os/week12# ss -tlnp | grep ssh
LISTEN 0      4096         0.0.0.0:22        0.0.0.0:* users:(("sshd",pid=3617,fd=3),("systemd",pid=1,fd=148))
LISTEN 0      4096            [::]:22           [::]:* users:(("sshd",pid=3617,fd=4),("systemd",pid=1,fd=149))

```

### Tantangan 10.5

**Instruksi:** Menambahkan berbagai perisai pertahanan *remote access* secara manual dan memverifikasinya. Modifikasi memuat parameter keamanan krusial yang mengatur toleransi login *root* serta jangka waktu autentikasi.

```bash
# [Admin] - Menggandakan file sebelum manipulasi blok akhir dari sshd_config
root@ubuntu-server:~/praktikum-os/week12# sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup.tantangan
root@ubuntu-server:~/praktikum-os/week12# echo "" | sudo tee -a /etc/ssh/sshd_config
root@ubuntu-server:~/praktikum-os/week12# echo "# Custom security settings - Week12 Lab" | sudo tee -a /etc/ssh/sshd_config
root@ubuntu-server:~/praktikum-os/week12# echo "PermitRootLogin no" | sudo tee -a /etc/ssh/sshd_config
root@ubuntu-server:~/praktikum-os/week12# echo "MaxAuthTries 3" | sudo tee -a /etc/ssh/sshd_config
root@ubuntu-server:~/praktikum-os/week12# echo "LoginGraceTime 30" | sudo tee -a /etc/ssh/sshd_config

# [Admin] - Meninjau konfirmasi penyematan teks di file sumber
root@ubuntu-server:~/praktikum-os/week12# sudo grep -E "PermitRoot|MaxAuth|GraceTime" /etc/ssh/sshd_config
PermitRootLogin yes
PermitRootLogin no
MaxAuthTries 3
LoginGraceTime 30

# [Admin] - Memastikan integritas formasi sintaks yang ditulis
root@ubuntu-server:~/praktikum-os/week12# sudo sshd -t
root@ubuntu-server:~/praktikum-os/week12# echo "Status kelulusan evaluasi (0 = aman): $?"
Status kelulusan evaluasi (0 = aman): 0

# [Admin] - Menerapkan ulang konfigurasi dan mencetak laporannya
root@ubuntu-server:~/praktikum-os/week12# sudo systemctl reload ssh
root@ubuntu-server:~/praktikum-os/week12# systemctl status ssh --no-pager | head -5
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: active (running) since Sun 2026-05-17 15:53:18 UTC; 1h 15min ago
TriggeredBy: ● ssh.socket
       Docs: man:sshd(8)

root@ubuntu-server:~/praktikum-os/week12# sudo grep -E "PermitRoot|MaxAuth|GraceTime" /etc/ssh/sshd_config > bukti-keamanan-ssh.txt
root@ubuntu-server:~/praktikum-os/week12# cat bukti-keamanan-ssh.txt
PermitRootLogin yes
PermitRootLogin no
MaxAuthTries 3
LoginGraceTime 30

```

---

## LATIHAN

### Latihan 10.1 — Audit Layanan dan Analisis Penetrasi Proses *Boot*

Melakukan diagnostik investigasi komprehensif terkait ekosistem layanan yang mengokupasi sistem operasi.

**1. Ekstraksi daftar layanan via `systemctl list-units`. Pilih tiga subjek spesifik, jabarkan detail kondisinya, dan terangkan fungsi yang mereka sandang.**

```bash
# [Admin] - Merangkum katalog layanan aktif ke teks
root@ubuntu-server:~/praktikum-os/week12# systemctl list-units --type=service --state=running > layanan-aktif.txt
root@ubuntu-server:~/praktikum-os/week12# cat layanan-aktif.txt
  UNIT                           LOAD   ACTIVE SUB     DESCRIPTION
  cron.service                   loaded active running Regular background program processing daemon
  dbus.service                   loaded active running D-Bus System Message Bus
  fwupd.service                  loaded active running Firmware update daemon
  getty@tty1.service             loaded active running Getty on tty1
  ModemManager.service           loaded active running Modem Manager
  multipathd.service             loaded active running Device-Mapper Multipath Device Controller
  polkit.service                 loaded active running Authorization Manager
  rsyslog.service                loaded active running System Logging Service
  ssh.service                    loaded active running OpenBSD Secure Shell server
  systemd-journald.service       loaded active running Journal Service
... (dipersingkat untuk kejelasan) ...
20 loaded units listed.

# [Admin] - Tinjauan spesifik status SSH
root@ubuntu-server:~/praktikum-os/week12# systemctl status ssh --no-pager | head -20
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: active (running) since Sun 2026-05-17 15:53:18 UTC; 1h 22min ago
TriggeredBy: ● ssh.socket
... (dipersingkat) ...

# [Admin] - Tinjauan spesifik status Cron
root@ubuntu-server:~/praktikum-os/week12# systemctl status cron --no-pager | head -20
● cron.service - Regular background program processing daemon
     Loaded: loaded (/usr/lib/systemd/system/cron.service; enabled; preset: enabled)
     Active: active (running) since Sun 2026-05-17 15:42:24 UTC; 3h 34min ago
... (dipersingkat) ...

# [Admin] - Tinjauan spesifik status Rsyslog
root@ubuntu-server:~/praktikum-os/week12# systemctl status rsyslog --no-pager | head -20
● rsyslog.service - System Logging Service
     Loaded: loaded (/usr/lib/systemd/system/rsyslog.service; enabled; preset: enabled)
     Active: active (running) since Sun 2026-05-17 15:42:22 UTC; 3h 35min ago
... (dipersingkat) ...

```

**Analisis Fungsi Layanan:** 1. **`ssh.service`** Entitas fundamental ini mewakili pengoperasian peladen *OpenSSH*, yang membuka koridor interaksi sistem dari jarak jauh bagi sang pengelola jaringan dengan mengandalkan kriptografi kokoh.
2. **`cron.service`** Fungsinya adalah mengakomodasi tugas repetitif atau automasi skrip berbasis kalender (skedul). Program *daemon* ini bertugas secara periodik sesuai konfigurasi yang dipatok administrator (harian, mingguan, dll).
3. **`rsyslog.service`** Layanan krusial yang berfungsi menghimpun segala keluh kesah atau notifikasi status (log) yang dihembuskan oleh rupa-ragam subsistem lain, lalu membukukannya secara rapi guna memudahkan *troubleshooting*.

**2. Kerahkan `systemd-analyze blame` untuk mengungkap lima komponen pemakan durasi pemuatan terbanyak sewaktu sistem mengalami *boot*.**

```bash
# [Admin] - Pengurutan 5 proses penghambat booting terlama
root@ubuntu-server:~/praktikum-os/week12# systemd-analyze blame | head -5
6min 44.904s apt-daily.service
      6.582s dev-sda2.device
      6.381s snapd.seeded.service
      6.117s snapd.service
      5.268s dpkg-db-backup.service

```

**3. Lacak elemen bermasalah memakai `systemctl --failed` lalu interogasi kronologinya via `journalctl`.**

```bash
# [Admin] - Melakukan deteksi kerusakan parsial layanan
root@ubuntu-server:~/praktikum-os/week12# systemctl --failed
  UNIT            LOAD   ACTIVE SUB    DESCRIPTION
● quotaon.service loaded failed failed Enable File System Quotas

# [Admin] - Membuka jurnal pembukuan pada layanan yang mogok beroperasi
root@ubuntu-server:~/praktikum-os/week12# journalctl -u quotaon.service -n 30
-- No entries --

```

### Latihan 10.2 — Implementasi Automasi Pemulihan pada Layanan Kustom (*Auto-Restart*)

Rancang layanan berbasis *systemd* independen yang memperagakan kecakapan respons pemulihan otomatis apabila program mengalami penghentian paksa.

**1. Penulisan *script bash* `monitor-disk.sh` untuk melakukan pencatatan statistik penggunaan wadah penyimpanan disk secara teratur setiap 30 detik.**

```bash
root@ubuntu-server:~/praktikum-os/week12# nano monitor-disk.sh

# --- [Kode Skrip: monitor-disk.sh] ---
# #!/bin/bash
# LOG_FILE="/var/log/monitor-disk.log"
# touch $LOG_FILE
# 
# while true; do
#     echo "=====================================" >> $LOG_FILE
#     echo "Rekaman Detik: $(date '+%Y-%m-%d %H:%M:%S')" >> $LOG_FILE
#     echo "Volume Alokasi Penyimpanan:" >> $LOG_FILE
#     df -h >> $LOG_FILE
#     echo "=====================================" >> $LOG_FILE
#     echo "" >> $LOG_FILE
#     
#     logger -t monitor-disk "Disk usage statistics updated at $(date '+%H:%M:%S')"
#     sleep 30
# done
# -------------------------------------

# [Admin] - Penetapan akses eksekusi dan memindahkannya ke sistem utama
root@ubuntu-server:~/praktikum-os/week12# chmod +x monitor-disk.sh
root@ubuntu-server:~/praktikum-os/week12# sudo cp monitor-disk.sh /usr/local/bin/
root@ubuntu-server:~/praktikum-os/week12# ls -la /usr/local/bin/monitor-disk.sh
-rwxr-xr-x 1 root root 718 May 17 16:05 /usr/local/bin/monitor-disk.sh

```

**2. Formulasi integrasi layanan di ranah *systemd* berbekal paramater komando pemulihan berkelanjutan.**

```bash
root@ubuntu-server:~/praktikum-os/week12# sudo nano /etc/systemd/system/monitor-disk.service

# --- [Konfigurasi: monitor-disk.service] ---
# [Unit]
# Description=Monitor Disk Usage Service - Latihan 10.2
# Documentation=man:df(1)
# After=local-fs.target
# 
# [Service]
# Type=simple
# User=root
# ExecStart=/usr/local/bin/monitor-disk.sh
# Restart=always
# RestartSec=5s
# StandardOutput=journal
# StandardError=journal
# 
# [Install]
# WantedBy=multi-user.target
# -------------------------------------------

# [Admin] - Memvalidasi registrasi dengan daemon-reload
root@ubuntu-server:~/praktikum-os/week12# sudo systemctl daemon-reload
root@ubuntu-server:~/praktikum-os/week12# systemctl list-unit-files --type=service | grep monitor-disk
monitor-disk.service                         disabled        enabled

```

**3. Inisiasi awal serta pengawasan eksistensi *output* di jurnal log.**

```bash
# [Admin] - Menghidupkan siklus servis permanen
root@ubuntu-server:~/praktikum-os/week12# sudo systemctl enable --now monitor-disk
Created symlink /etc/systemd/system/multi-user.target.wants/monitor-disk.service → /etc/systemd/system/monitor-disk.service.

# [Admin] - Mengulik aktivitas log untuk menjamin operasional
root@ubuntu-server:~/praktikum-os/week12# systemctl status monitor-disk --no-pager
● monitor-disk.service - Monitor Disk Usage Service - Latihan 10.2
     Loaded: loaded (/etc/systemd/system/monitor-disk.service; enabled; preset: enabled)
     Active: active (running) since Sun 2026-05-17 16:45:10 UTC; 4s ago
... (dipersingkat) ...

root@ubuntu-server:~/praktikum-os/week12# tail -20 /var/log/monitor-disk.log
...
=====================================
Waktu: 2026-05-17 16:48:11
Penggunaan Disk:
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        50G  4.6G   43G  10% /
...
=====================================

root@ubuntu-server:~/praktikum-os/week12# journalctl -u monitor-disk -n 10 --no-pager
May 17 16:45:10 ubuntu-server systemd[1]: Started monitor-disk.service - Monitor Disk Usage Service - Latihan 10.2.
May 17 16:46:10 ubuntu-server monitor-disk[4105]: Disk usage logged at 16:46:10

```

**4. Mengorkestrasikan kecelakaan (kematian sub-proses via `kill`) guna membuktikan keampuhan instruksi `Restart=always`.**

```bash
# [Admin] - Meraih identitas PID aktif dan melancarkan penikaman program secara brutal (Signal 9)
root@ubuntu-server:~/praktikum-os/week12# PID=$(systemctl show monitor-disk --property=MainPID --value)
root@ubuntu-server:~/praktikum-os/week12# echo "Pelacakan PID aktif: $PID"
Pelacakan PID aktif: 4414

# [Admin] - Melakukan eliminasi dan menanti manuver otomatis dari Systemd
root@ubuntu-server:~/praktikum-os/week12# sudo kill -9 $PID
root@ubuntu-server:~/praktikum-os/week12# echo "Eksekusi pemaksaan berhenti pada PID $PID tuntas."
Eksekusi pemaksaan berhenti pada PID 4414 tuntas.
root@ubuntu-server:~/praktikum-os/week12# sleep 10

# [Admin] - Evaluasi status pasca-insiden (PID baru terbit)
root@ubuntu-server:~/praktikum-os/week12# systemctl status monitor-disk --no-pager
● monitor-disk.service - Monitor Disk Usage Service - Latihan 10.2
     Loaded: loaded (/etc/systemd/system/monitor-disk.service; enabled; preset: enabled)
     Active: active (running) since Sun 2026-05-17 17:14:25 UTC; 1min 11s ago
       Docs: man:df(1)
   Main PID: 4451 (monitor-disk.sh)
...
May 17 17:14:20 ubuntu-server systemd[1]: monitor-disk.service: Failed with …al'.
May 17 17:14:25 ubuntu-server systemd[1]: monitor-disk.service: Scheduled re…t 2.
May 17 17:14:25 ubuntu-server systemd[1]: Started monitor-disk.service - Mon…0.2.

```

**5. Restorasi ruang kerja dengan melenyapkan keberadaan layanan buatan tersebut.**

```bash
# [Admin] - Mencabut legalitas layanan sepenuhnya
root@ubuntu-server:~/praktikum-os/week12# sudo systemctl disable --now monitor-disk
Removed "/etc/systemd/system/multi-user.target.wants/monitor-disk.service".
root@ubuntu-server:~/praktikum-os/week12# sudo rm /etc/systemd/system/monitor-disk.service
root@ubuntu-server:~/praktikum-os/week12# sudo systemctl daemon-reload
root@ubuntu-server:~/praktikum-os/week12# sudo rm /usr/local/bin/monitor-disk.sh
root@ubuntu-server:~/praktikum-os/week12# sudo rm /var/log/monitor-disk.log

```

### Latihan 10.3 — Pembedahan Log Forensik & Eskalasi Keamanan SSH

Menganalisis *dump* informasi log historis serta merevitalisasi standar konfigurasi peladen SSH menjadi lebih tangguh.

**1. Operasikan utilitas penyaringan jurnal (berfokus pada insiden *error* semenjak *startup*) lalu kompilasi rekamannya.**

```bash
# [Admin] - Merekap keluhan error mesin dan menjumlah total barisnya
root@ubuntu-server:~/praktikum-os/week12# journalctl -b -p err --no-pager > error-boot.txt
root@ubuntu-server:~/praktikum-os/week12# wc -l error-boot.txt
281 error-boot.txt

```

**2. Melengkapi kerangka keamanan protokol `sshd_config` lewat pengetatan autentikasi lalu menstandarisasi validasinya.**

```bash
# [Admin] - Menyelaraskan ulang benteng keamanan tanpa campur tangan editor visual
root@ubuntu-server:~/praktikum-os/week12# sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
root@ubuntu-server:~/praktikum-os/week12# echo -e "\nPermitRootLogin no\nMaxAuthTries 3\nLoginGraceTime 30" | sudo tee -a /etc/ssh/sshd_config

PermitRootLogin no
MaxAuthTries 3
LoginGraceTime 30

# [Admin] - Melakukan kroscek kesehatan sintaks sebelum merealisasikan perubahan
root@ubuntu-server:~/praktikum-os/week12# sudo sshd -t && sudo systemctl reload ssh

```

**3. Pembuktian integritas peladen (*service status*, inspeksi *socket listening*, serta ketersediaan parameter yang baru dimasukkan).**

```bash
# [Admin] - Validasi 3 lapis untuk layanan jarak jauh SSH
root@ubuntu-server:~/praktikum-os/week12# systemctl status ssh --no-pager | head -3
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: active (running) since Sun 2026-05-17 15:53:18 UTC; 2h 2min ago

root@ubuntu-server:~/praktikum-os/week12# ss -tlnp | grep ssh
LISTEN 0      4096         0.0.0.0:22        0.0.0.0:* users:(("sshd",pid=3617,fd=3),("systemd",pid=1,fd=134))
LISTEN 0      4096            [::]:22           [::]:* users:(("sshd",pid=3617,fd=4),("systemd",pid=1,fd=141))

root@ubuntu-server:~/praktikum-os/week12# sudo grep -E "PermitRoot|MaxAuth|GraceTime" /etc/ssh/sshd_config
PermitRootLogin yes
PermitRootLogin no
MaxAuthTries 3
LoginGraceTime 30

```

**4. Pembatalan modifikasi via *rollback* ke dokumen *backup* awal.**

```bash
# [Admin] - Merestorasi dan me-refresh ulang ekosistem SSH
root@ubuntu-server:~/praktikum-os/week12# sudo cp /etc/ssh/sshd_config.bak /etc/ssh/sshd_config
root@ubuntu-server:~/praktikum-os/week12# sudo sshd -t && sudo systemctl reload ssh

```