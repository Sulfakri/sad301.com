---
title: HTTP Basic Access Authentication Menggunakan NodeJS
author: sad301
date: 2015-02-04
template: article.jade
excerpt: Pada postingan kali ini saya akan membahas sedikit tentang teknik implementasi HTTP _Basic Access Authentication_ untuk autentikasi dalam aplikasi web menggunakan [NodeJS](http://nodejs.org)
---

Pada postingan kali ini saya akan membahas sedikit tentang teknik implementasi HTTP _Basic Access Authentication_ untuk autentikasi dalam aplikasi web menggunakan [NodeJS](http://nodejs.org). Sebagai referensi dasar, anda bisa membaca artikel di halaman [Wikipedia](http://en.wikipedia.org/wiki/Basic_access_authentication) atau untuk lebih detailnya ke halaman [Request for Comment (RFC) 1945](http://tools.ietf.org/html/rfc1945#section-11.1).

<span class="more"></span>

Sederhananya, urutan rangkaian komunikasi antara _client_ dan _server_ dengan metode autentikasi ini adalah sebagai berikut :

Aplikasi _client_ atau _User-Agent_ akan mengirimkan permintaan _(HTTP request)_ ke _server_. Untuk bisa dilayani oleh _server_, permintaan tersebut harus disertai dengan _header_ `authorization`. Jika diskenariokan dalam kondisi yang sebenarnya (semisal dalam aplikasi _web browser_), umumnya _request_ awal tidak menyertakan _header_ tersebut.

```http
GET / HTTP/1.1
User-Agent: curl/7.39.0
Host: 192.168.1.90:8080
Accept: */*
```

Pada saat _request_ tersebut diterima di sisi _server_, program _server_ kemudian akan memeriksa keberadaan _header_ `authorization`. Karena seperti yang kita ketahui, _header_ tersebut tidak ada dalam _request_ yang dikirim sebelumnya, maka _server_ akan memberikan balasan _(response)_ dengan _status code_ 401 _(Not Authorized)_. Dalam _response_ tersebut juga disertakan _header_ `WWW-Authenticate`. Jika diakses melalui _web browser_ biasanya ditandai dengan munculnya kotak dialog yang meminta user untuk memasukkan _username_ dan _password_.

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: basic
```

Dari _response_ yang didapatkan sebelumnya, kita mengetahui bahwa _header_ `authorization` akan diperlukan, jadi _User-Agent_ perlu mengirimkan `request` baru (kali ini disertai dengan _header_ `authorization`). _header_ `authorization` akan berisi teks yaitu metode autentikasi (dalam hal ini adalah _Basic_) kemudian diikuti dengan kombinasi text `username:password` yang di-_encode_ dengan format Base64. Jika kita menggunakan kombinasi _username_ dan _password_ `admin:nimda` (nilai Base64-nya adalah `YWRtaW46bmltZGE=`), maka contoh _request_-nya adalah sebagai berikut :

```http
GET / HTTP/1.1
User-Agent: curl/7.39.0
Host: 192.168.1.90:8080
Accept: */*
Authorization: Basic YWRtaW46bmltZGE=
```

Karena _request_ yang kedua ini sudah menyertakan _header_ `authorization`, yang perlu dilakukan _server_ adalah membandingkan _username_ dan _password_ yang terdapat dalam _header_ tersebut dengan _username_ dan _password_ yang tersimpan dalam server (baik itu dalam bentuk _file_ atau dalam _database_). Jika salah, server akan mengembalikan _response_ dengan _status code_ 403 _(Forbidden)_, sebaliknya _server_ dapat mengembalikan _response_ dengan _status code_ 200 _(OK)_ disertai dengan konten yang diminta.

```http
HTTP/1.1 200 OK
Content-Type: text/html
```

Rangkaian proses diatas dapat diilustrasikan sebagai berikut :

<p class="img-container"><img align="middle" src="https://lh3.googleusercontent.com/-W0J_93BPAqk/VNHw8G7MPoI/AAAAAAAAODI/wB0VV7eu08Q/s2048-Ic42/HTTP%252520Basic%252520Authentication.png" alt="text"/></p>

Diantara pembaca mungkin ada yang bertanya kenapa cuma Base64 ? kenapa tidak dienkripsi ?. Sebenarnya _Basic Access Authentication_ ini memang lebih direkomendasikan untuk digunakan pada protokol HTTPS. Jika sekiranya anda membutuhkan metode yang lebih aman, lebih direkomendasikan menggunakan metode seperti [_Digest Access Authentication_](https://en.wikipedia.org/wiki/Digest_access_authentication) atau [OAuth](https://en.wikipedia.org/wiki/OAuth).

Selanjutnya kita coba terapkan dalam kode program. Yang pertama, kita akan membutuhkan 3 macam halaman web, yaitu `index.html` sebagai halaman muka (ditampilkan jika autentikasi benar), halaman `401.html` ditampilkan pada saat autentikasi, dan `403.html` yang ditampilkan jika autentikasi gagal.

Berikutnya kita akan mengetikkan _source code_ Javascript untuk program server (kita berikan nama _file_ `server.js`). Dalam program ini kita akan membutuhkan dua macam _package_ yaitu `http`, karena program akan menggunakan protokol HTTP dan `fs` karena kita akan melayani _request_ menggunakan file HTML yang sudah dibuat sebelumnya.

```javascript
var http = require('http');
var fs = require('fs')
```

Berikutnya kita buat daftar _user_ (beserta _password_-nya) yang akan diperbolehkan untuk mengakses halaman `index.html`. Untuk mempermudah, saya hanya membuat daftar _user_ ini dalam format array Javascript.

```javascript
var users = [
  {"username":"admin", "password":"nimda"},
  {"username":"user", "password":"resu"}
]
```

Untuk mempermudah dalam melayani _request_ dalam pengkodean program, disini saya membuat sebuah fungsi sederhana yang akan mengembalikan _response_ dengan file yang kita inginkan dan dengan status code dan _header_ yang bisa disesuaikan.

```javascript
function handleWithFile(res, fileName, code, _header_) {
  fs.readFile(fileName, function (err, data) {
    res.writeHead(code, _header_);
    res.write(data);
    res.end();
  });
}
```

Kemudian kita buat fungsi utama yang akan mengatur mekanisme penerimaan _request_ dan pengembalian _response_.

```javascript
function handle(req, res) {
  // kode berikutnya diketik disini
}
```

Yang kita perlukan pertama adalah mekanisme menentukan ada tidaknya _header_ `authorization` pada _request_ yang diterima. Jika _header_ tersebut tidak ada (atau `undefined`) maka kita kembalikan _response_ dengan _status code 401_.

```javascript
if(typeof req._header_s.authorization === 'undefined') {
  handleWithFile(res, '401.html', 401, {
    'WWW-Authenticate': 'Basic',
    'Content-Type': 'text/html'
  });
}
else {
  // kode berikutnya diketik disini
}
```

Sebaliknya jika _header_ `authorization` ada, kita perlu men-_decode_ nilai Base64 yang terdapat dalam _header_ tersebut.

```javascript
var token = req._header_s.authorization.split(/\s+/).pop();
var auth = new Buffer(token, 'base64').toString().split(':');
```

Variabel `auth` adalah sebuah array yang berisi _username_ dan _password_. Jika nilai Base64 yang dikirimkan benar (merupakan bentuk _encoded_ dari `username:password`) seharusnya variabel tersebut memiliki panjang 2. Jika benar variabel `auth` sudah bisa diproses, sebaliknya kita kembalikan response dengan _status code_ 401.

```javascript
if(auth.length === 2) {
  // kode berikutnya diketik disini
}
else {
  handleWithFile(res, '401.html', 401, {
    'WWW-Authenticate': 'Basic',
    'Content-Type': 'text/html'
  });
}
```

Sekarang kita mulai bisa membandingkan _username/password_ dari _request_ dengan _username/password_ dalam daftar yang sudah dibuat sebelumnya. Disini saya gunakan variabel _boolean_ `valid` untuk menentukan apakah _username/password_ dalam _request_ tersebut benar.

```javascript
var valid = false;
for(var i=0; i<users.length; i++) {
  if((auth[0] === users[i].username) &&
  (auth[1] === users[i].password)) {
    valid = true;
    break;
  }
}
```

Selebihnya, anda bisa tebak sendiri, kita tinggal menggunakan nilai variabel `valid`. Jika variabel tersebut bernilai `true` maka kita kembalikan _response_ dengan _status code_ 200 sebaliknya kita kembalikan _response_ dengan _status code_ 403.

```javascript
if(valid) {
  handleWithFile(res, 'index.html', 200, {
    'Content-Type': 'text/html'
  });
}
else {
  handleWithFile(res, '403.html', 403, {
    'Content-Type': 'text/html'
  });
}
```

Selanjutnya kita tinggal membuat objek server dengan meregister metode `handle()` yang sudah dibuat sebelumnya kedalam metode `http.createServer()` dan kita set server tersebut untuk "mendengarkan" pada port 8080.

```javascript
http.createServer(handle).listen(8080)
console.log('ready!');
```

Simpan kode program yang sudah diketik, dan kemudian anda mulai bisa menjalankan program dengan mengetikkan perintah dibawah

```
node server.js
```

dan aplikasi dapat diakses melalui web browser dengan mengetikkan URL [http://127.0.0.1:8080](http://127.0.0.1:8080). Kode selengkapnya dapat dilihat di [halaman Github](https://github.com/sad301/http-basic-auth-sample).