---
title: Instalasi VNC Server untuk Remote Desktop Raspberry Pi
author: sad301
date: 2015-01-29
template: article.jade
---

Kita mulai dengan pertanyaan "apa itu _Remote Desktop_ ?" - Sederhananya, _remote desktop_ disini adalah fitur yang disediakan dalam perangkat lunak atau sistem operasi untuk menjalankan lingkungan _desktop_ pada satu perangkat komputer dan menampilkannya pada perangkat lain. Fitur _remote desktop_ ini akan bermanfaat bagi anda yang menggunakan Raspberry Pi (atau komputer lain) secara _headless_ atau tanpa monitor.

_Virtual Network Computing_ (VNC) sendiri adalah sistem _desktop sharing_ yang memungkinkan _user_ untuk mengontrol sebuah perangkat komputer dari perangkat lainnya. Dalam implementasinya, sistem VNC (dan sistem _remote desktop_ lainnya) akan berjalan pada jaringan komputer dan akan membutuhkan dua macam program, salah satunya berjalan di sisi _server_ (komputer yang akan di-_share_ desktopnya) dan yang lain berjalan di _client_ (perangkat yang akan mengakses desktop dari _server_).

Disini kita akan menggunakan program [TightVNC](http://www.tightvnc.com) untuk memberikan akses ke desktop Raspbian dan program [VNC Viewer](https://www.realvnc.com) untuk menampilkan desktop Raspberry Pi di komputer yang kita gunakan. Untuk menginstall TightVNC di Raspbian ketikkan perintah dibawah pada aplikasi LXTerminal atau melalui terminal Putty.

```
sudo apt-get install tightvncserver
```

Setelah instalasi selesai, ketikkan perintah `tightvncserver` untuk konfigurasi awal. Disini program akan meminta anda untuk memasukkan password, yang nantinya akan diminta setiap kali kita mencoba mengakses desktop Raspbian melalui program VNC Client. Setelah itu kita mulai bisa menjalankan _service VNC_ dengan mengetikkan perintah dibawah ini :

```
vncserver :0 -geometry 640x480 -depth 24
```

Dari sini kita mulai bisa mengakses desktop Raspbian, jalankan aplikasi VNC Viewer pada komputer/laptop anda. Pada jendela aplikasi tersebut masukkan alamat IP dari Raspberry Pi kemudian klik tombol Connect (perhatikan gambar). Aplikasi kemudian akan meminta anda untuk memasukkan password, ketikkan password yang anda masukkan pada saat konfigurasi kemudian klik tombol OK.

![text](https://lh3.googleusercontent.com/-kQ4gV9aInvw/VMnUfBUmmyI/AAAAAAAAOCk/r-aX-mkwaIk/s2048-Ic42/Untitled.png)

Dan hasilnya adalah sebagai berikut :

![text](https://lh3.googleusercontent.com/-BFrkBE4oubo/VMnVM6qU2OI/AAAAAAAAOCs/WOVBEHntdu8/s2048-Ic42/Untitled.png)
