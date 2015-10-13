---
title: MySQL CRUD Dengan NodeJS
author: sad301
date: 2015-09-03
template: article.jade
---

CRUD merupakan singkatan dari _Create_, _Retrieve_, _Update_, dan _Delete_. Keempatnya merupakan aktifitas dasar yang sering dilakukan dalam _Database Management System_ (DBMS).

<span class="more"></span>

_Create_ adalah proses membuat (memasukkan) _record_ baru kedalam tabel database, dilakukan dengan menggunakan perintah `INSERT`. _Retrieve_ adalah proses mengambil (menampilkan) _record_ dengan menggunakan perintah `SELECT`. _Update_ adalah proses memperbaharui, dengan perintah `UPDATE`, dan _Delete_ adalah proses menghapus, dengan perintah `DELETE`.

Sebelum kita masuk ke contoh program, mari kita mulai dengan membuat tabel sederhana pada DBMS MySQL. Ketikkan _script_ SQL berikut kemudian simpan dengan nama `sample.sql` :

```sql
DROP DATABASE IF EXISTS `sample`;	-- btw, kalo ada db dengan
CREATE DATABASE `sample`;			-- nama yang sama silahkan
USE `sample`;						-- dimodif sendiri

CREATE TABLE `kontak` (
	id int not null auto_increment primary key,
	nama varchar(128) not null,
	alamat text,
	no_telepon varchar(15),
	website varchar(32),
	email varchar(32)
);
```
Oke sip, selebihnya kita tinggal meng-_import_ kode SQL sebelumnya ke dalam MySQL :

```
mysql -u [user] -p [password] < sample.sql
```

Selanjutnya kita akan membuat aplikasi NodeJS yang akan mengakses database `sample` yang sudah dibuat sebelumnya. Disini kita hanya membuat aplikasi _command line_ dengan fungsi CRUD. Untuk aplikasi web dengan fungsi tersebut, akan saya coba bahas lain waktu.

Sebelum kita mulai, aplikasi ini akan membutuhkan _package_ `node-mysql`. Untuk menginstall _package_ tersebut gunakan perintah dibawah :

```
npm install node-mysql
```

Sekarang kita mulai mengkodekan programnya, buatlah file Javascript baru dengan nama `app.js`. Didalamnya sebutkan deklarasi import untuk menggunakan package `node-mysql`.

```javascript
var mysql = require('mysql');
```

[1]: http://nodejs.org "NodeJS"
[2]: http://www.mysql.com "MySQL"
[3]: https://github.com/felixge/node-mysql "node-mysql"