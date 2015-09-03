---
title: Instalasi NodeJS di Raspberry Pi
author: sad301
date: 2013-12-03
template: article.jade
---

[NodeJS][1] adalah platform perangkat lunak untuk pengembangan aplikasi _server-side_ yang menggunakan bahasa pemrograman Javascript. Yang menarik dari software ini adalah, kita dapat menjalankan web server didalam komputer tanpa perlu menggunakan software tambahan seperti Apache Web Server atau sejenisnya.

<span class="more"></span>

Dengan mengkombinasikan Raspberry Pi dengan Node.js, kita bisa membangun sebuah web server lokal berskala kecil.

Berikut ini adalah langkah-langkah instalasi Node.js di Raspberry Pi.
1. Login kedalam Raspbian dengan menggunakan username & password anda
2. Melalui terminal (atau web browser), download software node.js untuk Raspberry Pi melalui [halaman ini](http://nodejs.org/dist/latest/)
```
pi@raspberry ~/Downloads $ wget http://nodejs.org/dist/latest/node-v0.10.22-linux-arm-pi.tar.gz
```
3. Extract file yang telah didownload ke direktori `/opt`
```
pi@raspberry ~/Downloads $ sudo tar -xzvf node-v0.10.22-linux-arm-pi.tar.gz -C /opt
```
4. Pindah ke direktori tersebut, dan buat _symbolic link_
```
pi@raspberry ~/Downloads $ cd /opt
pi@raspberry /opt $ sudo ln -s node-v0.10.22-linux-arm-pi node
```
5. Buka file `/etc/profile` dengan menggunakan text editor nano (atau sejenisnya)
```
pi@raspberry /opt $ sudo nano /etc/profile
```
6. Dalam file `/etc/profile`, tambahkan baris-baris dibawah ini sebelum baris bertuliskan `export PATH`
```
NODEJS_HOME="/opt/node"
PATH="${PATH}:${NODEJS_HOME}/bin"
```
7. Simpan semua perubahan dengan menekan `CTRL+O` (jika anda menggunakan nano) dan keluar dari text editor dengan menekan `CTRL+X`
8. Terakhir, logout dari Raspbian dan login kembali
9. Anda dapat menguji node.js dengan menggunakan perintah `node -v` melalui terminal

Selanjutnya, kita akan membuat sebuah web server sederhana. Ketikkan baris-baris kode javascript dibawah ini, kemudian simpan dengan nama `server.js` pada direktori `/home/pi`

```javascript
var http = require('http');
var fs = require('fs');

http.createServer(function(req, res) {
  fs.readFile('index.html', 'utf8', function(err, data) {
    res.writeHead(200, {'content-type':'text/html'});
    if(err) {
      res.write('<html><body>Unable to open file !</body></html>');
    }
    else {
      res.write(data);
    }
    res.end();
  });
}).listen(8124, function() {
  console.log('Bound to port 8124');
});

console.log('Server running on 8124');
```

Berikut, ketikkan kode-kode HTML dibawah ini, kemudian simpan dengan nama `index.html` pada direktori `/home/pi`

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Sample Page</title>
  </head>
  <body>
    <h1>Welcome</h1>
    <p>This node.js server runs on Raspberry Pi</p>
  </body>
</html>
```

Jalankan server dengan menggunakan perintah berikut :
```
pi@raspberry ~ $ node server.js
```

[1]: http://nodejs.org "NodeJS"
