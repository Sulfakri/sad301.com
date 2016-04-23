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

Penjelasan untuk proses instalasi kita kelompokkan menjadi dua bagian, proses instalasi untuk pembaca yang menggunakan OS berbasis Linux dan Windows.

Instalasi dengan OS Linux
-------------------------

Disini kita akan coba menginstall OS Raspbian kedalam Raspberry Pi dengan menggunakan OS Linux. distribusi linux yang saya gunakan disini adalah Ubuntu 14.04 LTS.

Setelah mengunduh file _image_ Raspbian dari websitenya, kita perlu mengekstrak terlebih dahulu file unduhan tersebut, karena _image_ Raspbian biasanya didistribusikan dalam format file `.zip`. untuk ekstraksi kita bisa gunakan perintah `unzip` seperti dibawah ini :

```
user@host:~$ unzip file.zip -d .
```

