---
title: Instalasi Headless Raspbian Linux di Raspberry Pi
author: sad301
date: 2016-03-19
template: article.jade
---

Pada tutorial kali ini, kita akan belajar bagaimana menginstall Raspbian Linux di Raspberry Pi tanpa menggunakan monitor atau biasa dikenal dengan istilah _headless_. Tutorial ini akan bermanfaat bagi anda yang menggunakan Raspberry Pi tidak sebagai komputer utama atau hanya sebagai alat tambahan.

<span class="more"></span>

Hardware dan software yang dibutuhkan antara lain :
* Raspberry Pi (model B atau model B+)
* SD Card (untuk RPi model B) atau MicroSD Card + Adapter (untuk RPi model B+)
* Power adapter 5v (bisa menggunakan charger handphone)
* Kabel ethernet
* Card Reader
* File _image_ <a href="https://www.raspberrypi.org/downloads/raspbian/" target="_blank">Raspbian</a> versi terbaru
* <a href="https://www.sdcard.org/downloads/formatter_4/eula_windows/" target="_blank">SD Formatter</a> (untuk OS Windows)
* <a href="https://sourceforge.net/projects/win32diskimager/" target="_blank">Win32 Disk Imager</a> (untuk OS Windows)

Instalasi dengan OS Linux
-------------------------

Disini kita akan coba menginstall OS Raspbian kedalam Raspberry Pi dengan menggunakan OS Linux. distribusi linux yang saya gunakan disini adalah Ubuntu 14.04 LTS.

Setelah mengunduh file _image_ Raspbian dari websitenya, kita perlu mengekstrak terlebih dahulu file unduhan tersebut, karena _image_ Raspbian biasanya didistribusikan dalam format file `.zip`. untuk ekstraksi kita bisa gunakan perintah `unzip` seperti dibawah ini :

```
user@host:~/Downloads$ unzip 2016-02-26-raspbian-jessie.zip -d .
```

Langkah selanjutnya yang perlu dilakukan adalah menghapus partisi SD Card/MicroSD Card (jika ada) dan kemudian memformat ulang SD Card/MicroSD Card tersebut dengan _filesystem_ FAT32. Untuk tahap ini, kita bisa gunakan _tool_ `fdisk`.

Sebelumnya, kita perlu tahu nama perangkat SD Card/MicroSD Card yang kita miliki

```
user@host:~$ ls -l /dev | grep mmc
brw-rw----  1 root disk    179,   0 Apr 23 15:50 mmcblk0
brw-rw----  1 root disk    179,   1 Apr 23 15:50 mmcblk0p1
brw-rw----  1 root disk    179,   2 Apr 23 15:50 mmcblk0p2
```



Instalasi dengan OS Windows
---------------------------

Quick Brown Fox Jumps Over The Lazy Dog