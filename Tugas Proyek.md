# Nama   : rasyiq satrio musthafa
# NIM    : 254107020079
# Kelas  : TI-1G
# Absen  : 25

Tugas Proyek 

1) Persiapan Sistem Dasar 

* a) Siapkan file ISO Ubuntu versi LTS (Long Term Support) terbaru 



| Name 

 | Date modified 

 | Type 

 | Size 

 |
| --- | --- | --- | --- |
|  | ubuntu-26.04-desktop-amd64.iso 

 | 03/06/2026 19:59 

 | Disc Image File 

 |

* b) Gunakan tool remastering pilihan Anda (sangat disarankan menggunakan Cubic). 



> **Tampilan Lingkungan Virtual Cubic:**
> Customize the Linux file system using the virtual environment terminal. You have entered the virtual environment. Failed to create stream fd: No such file or directory Failed to create stream fd: No such file or directory Failed to create stream fd: No such file or directory `root@cubic:~#` 
> 
> 

---

2) Kustomisasi dan Instalasi Aplikasi 

Pasang beberapa aplikasi berikut ke dalam sistem yang sedang di-remaster. 

* a) VLC Media Player (Pemutar multimedia) 



```bash
[cite_start]root@cubic:~# apt policy vlc [cite: 43]
[cite_start]vlc: [cite: 44]
  [cite_start]Installed: 3.0.23-1 [cite: 45]
  [cite_start]Candidate: 3.0.23-1 [cite: 46]
  [cite_start]Version table: [cite: 47]
 [cite_start]*** 3.0.23-1 500 [cite: 48]
        [cite_start]500 http://archive.ubuntu.com/ubuntu resolute/universe amd64 Packages [cite: 49]
        [cite_start]100 /var/lib/dpkg/status [cite: 50]
[cite_start]root@cubic:~# [cite: 51]

```

* b) GIMP (Editor gambar) 



```bash
[cite_start]root@cubic:~# gimp --version [cite: 53]
[cite_start]GNU Image Manipulation Program version 3.2.2 [cite: 54]
[cite_start]root@cubic:~# [cite: 55]

```

* c) Apache2 + PHP (Web server lokal) 



```bash
[cite_start]root@cubic:~# apache2 -v [cite: 57]
[cite_start]Server version: Apache/2.4.66 (Ubuntu) [cite: 58]
[cite_start]Server built:   2026-06-03T15:25:00 [cite: 59, 61]
[cite_start]root@cubic:~# [cite: 60]

```

* d) Visual Studio Code (Code editor) 



```bash
[cite_start]root@cubic:~# apt policy code [cite: 63]

```

| Detail Tabel Versi Code 

 | Keterangan 

 |
| --- | --- |
| code: |  |
| Installed: | 1.124.2-1781225536 |
| Candidate: | 1.124.2-1781225536 |
| Version table: |  |
| *** 1.124.2-1781225536 | 500 |
|  | 500 [https://packages.microsoft.com/repos/code](https://packages.microsoft.com/repos/code) stable/main amd64 Packages |
|  | 100 /var/lib/dpkg/status |
| 1.124.0-1781066808 | 500 |
|  | 500 [https://packages.microsoft.com/repos/code](https://packages.microsoft.com/repos/code) stable/main amd64 Packages |
| 1.123.2-1781044405 | 500 |
|  | 500 [https://packages.microsoft.com/repos/code](https://packages.microsoft.com/repos/code) stable/main amd64 Packages |

* e) Aplikasi Kustom Buatan Sendiri: Buat sebuah program sederhana berbasis Bash Script yang berfungsi untuk menampilkan informasi dasar perangkat keras komputer, meliputi: 


* i) Informasi Prosesor (CPU) 


* ii) Kapasitas dan Penggunaan Memori (RAM) 


* iii) Kapasitas Ruang Penyimpanan (Storage) 





```text
[cite_start]spek-komputer [cite: 69]
[cite_start]APLIKASI MONITORING HARDWARE BY RASYIQTAPS [cite: 70]

[cite_start][1] INFORMASI KEPALA / CPU: [cite: 71]
[cite_start]Model name:          11th Gen Intel(R) Core(TM) i7- [cite: 71, 75]
                     [cite_start]2.30GHz [cite: 72]
[cite_start]Socket(s):           1 [cite: 74, 77]
[cite_start]Core(s) per socket:  5 [cite: 73, 76]

[cite_start][2] INFORMASI MEMORI / RAM: [cite: 78]

```

|  | total 

 | used 

 | free 

 | shared buff/cache 

 |
| --- | --- | --- | --- | --- |
| <br>**Mem:** 

 | 5.7Gi 

 | 1.5Gi 

 | 705Mi 

 | 234Mi 4.0Gi 

 |
| <br>**Swap:** 

 | 0B 

 | 0B 

 | 0B 

 |  |

```text
[cite_start][3] INFORMASI PENYIMPANAN / STORAGE: [cite: 80]
[cite_start]Filesystem     Size  Used Avail Use% Mounted on [cite: 81, 83]
[cite_start]total          18G   7.9G  9.5G  46% [cite: 82, 83]

[cite_start]Tekan [ENTER] untuk keluar aplikasi. [cite: 84]

```

---

3) Kustomisasi Tampilan (Antarmuka) 

Ubah visual atau estetika pada Desktop Environment bawaan, yang mencakup perubahan pada: 

* a) Wallpaper (Latar belakang desktop) 



```bash
[cite_start]root@cubic:~# mkdir -p /usr/share/background/ [cite: 88]
[cite_start]root@cubic:~# nano /usr/share/glib-2.0/schemas/10_ubuntu_wallpaper.gschema.override [cite: 89]
[cite_start]root@cubic:~# glib-compile-schemas /usr/share/glib-2.0/schemas/ [cite: 89]
[cite_start]root@cubic:~# [cite: 90]

```

* b) Tema sistem (Theme) 



```bash
[cite_start]root@cubic:~# apt install papirus-icon-theme -y [cite: 92]
[cite_start]papirus-icon-theme is already the newest version (20250501+git20260316-0ubuntu1). [cite: 93]
[cite_start]Summary: [cite: 93]
[cite_start]Upgrading: 0, Installing: 0, Removing: 0, Not Upgrading: 195 [cite: 94]
[cite_start]root@cubic:~# [cite: 95]
[cite_start]root@cubic:~# nano /usr/share/glib-2.0/schemas/10_ubuntu_wallpaper.gschema.override [cite: 96]
[cite_start]root@cubic:~# glib-compile-schemas /usr/share/glib-2.0/schemas/ [cite: 96]
[cite_start]root@cubic:~# cat /usr/share/glib-2.0/schemas/10_ubuntu_wallpaper.gschema.override [cite: 97]
[cite_start][org.gnome.desktop.background] [cite: 98]
[cite_start]picture-uri = 'file:///usr/share/backgrounds/wallpaper-keren.jpg' [cite: 99]
[cite_start]picture-uri-dark = 'file:///usr/share/backgrounds/wallpaper-keren.jpg' [cite: 99]
[cite_start]picture-options = 'zoom' [cite: 100]

[cite_start][org.gnome.desktop.screensaver] [cite: 101]
[cite_start]picture-uri = 'file:///usr/share/backgrounds/wallpaper-keren.jpg' [cite: 102]
[cite_start]picture-options = 'zoom' [cite: 102]

```

* c) Paket ikon (Icons pack) 



```bash
[cite_start]root@cubic:~# ls /usr/share/icons/ [cite: 104]
Adwaita                 Bibata-Modern-Amber      Bibata-Modern-Classic
Bibata-Modern-Ice       Bibata-Original-Amber    HighContrast
Papirus                 Papirus-Dark             Papirus-Light
Yaru                    Yaru-blue                Yaru-blue-dark
Yaru-dark               Yaru-magenta             Yaru-magenta-dark
Yaru-olive              Yaru-olive-dark          Yaru-prussiangreen
Yaru-prussiangreen-dark Yaru-purple              Yaru-purple-dark
Yaru-red                Yaru-red-dark            Yaru-sage
Yaru-sage-dark          Yaru-wartybrown          Yaru-wartybrown-dark
Yaru-yellow             Yaru-yellow-dark         default
gnome-logo-text-dark.svg gnome-logo-text.svg     handhelds
hicolor                 locolor                  redglass
whiteglass              Bibata-Original-Classic  Bibata-Original-Ice
DMZ-Black               DMZ-White
[cite_start]root@cubic:~# [cite: 104, 105, 106, 107, 108, 109, 110, 111, 112, 113, 114, 115, 116, 117, 118, 119, 120, 121, 122, 123, 124, 125, 126, 127, 128, 129, 130, 131, 132, 133, 134, 135, 136, 137, 138, 139, 141]

```

---

4) Pembuatan File ISO Baru 

Lakukan proses build atau generate untuk menghasilkan file ISO baru hasil modifikasi Anda, lalu beri nama dengan format: Ubuntu-Custom-[NIM].iso (Ganti [NIM] dengan Nomor Induk Mahasiswa Anda). 

| Custom Disk... 

 | Detail Konfigurasi |
| --- | --- |
| <br>**Version** 

 | 2026.06.14 

 |
| <br>**Filename** 

 | ubuntu-custom-254107020079.iso 

 |
| <br>**Directory** 

 | /home/rasyiqtaps/remaster-ubuntu 

 |

---

5) Pengujian dan Dokumentasi 

* a) Uji coba file ISO kustom yang telah jadi menggunakan emulator/perangkat virtual seperti VirtualBox atau QEMU. 


* b) Ambil tangkapan layar (screenshot) yang menunjukkan: 


* 1. Tampilan utama desktop yang telah diubah visualnya. 


* 2. Hasil eksekusi dari Bash script (aplikasi kustom informasi perangkat keras) yang telah Anda buat di dalam terminal.