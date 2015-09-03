---
title: Server HTTP dengan Java
author: sad301
date: 2014-03-09
template: article.jade
---

Segala sesuatu yang "berbasis web" pastinya tidak akan terlepas dari yang namanya server HTTP, baik itu di skala internet, intranet atau bahkan localhost sekalipun. Server HTTP adalah program yang dapat menerima permintaan _(request)_ pada protokol HTTP, dan memberikan balasan _(response)_ atas permintaan tersebut.

<span class="more"></span>

Sebenarnya program jenis ini cukup mudah ditemukan, jika anda terbiasa membuat website dengan PHP pasti sudah familiar dengan yang namanya [Apache HTTPD](http://httpd.apache.org/), atau kalau anda programmer java web, pasti sudah kenal dengan [Apache Tomcat](http://tomcat.apache.org/) atau [Glassfish](https://glassfish.java.net/).

## The Story Behind

Awal cerita dimulai ketika saya menerima kiriman Raspberry Pi yang rencananya akan dijadikan web server java. Saya coba jalankan server Apache Tomcat di RPi tersebut dan hasilnya kurang memuaskan, karena performa RPi jadi agak sedikit menurun (alias lemot). Hal ini saya rasa wajar saja, karena RPi hanya ditenagai prosessor ARM 750MHz dan 512MB memory.

Jadi saya mulai mencari cara alternatif dan ternyata solusinya tidak jauh-jauh dari JRE yaitu di library `com.sun.net.httpserver.HttpServer` (dokumentasi library ini ada di direktori JRE bukan JDK, aneh bukan ?). Jadi pada intinya, dengan memanfaatkan library ini, kita dapat membuat sebuah program java yang dapat bekerja pada protokol HTTP.

## Let's Code

Jadi langsung saja, jalankan text editor anda dan ketikkan _source code_ java berikut ini :

```java
package com.sad301.exc;

public class SampleHTTPServer {
  public static void main(String[] args) {
    /* masih kosong */
  }
}
```

Objek `HttpServer` hanya bisa dibentuk melalui metode statis `create()`, yang menggunakan dua buah parameter, yaitu objek `java.net.InetSocketAddress` dan nilai integer `backlog`, yaitu jumlah maksimum koneksi yang akan masuk ke server. Yang perlu diingat metode `create()` akan melemparkan eksepsi `IOException`. Pada kode selanjutnya kita akan menambahkan deklarasi `import` dan membentuk objek `HttpServer`.

```java
package com.sad301.exc;

import com.sun.net.httpserver.HttpServer;
import java.io.IOException;
import java.net.InetSocketAddress;

public class SampleHTTPServer {
  public static void main(String[] args) throws IOException {
    InetSocketAddress addr = new InetSocketAddress(8080);
    HttpServer server = HttpServer.create(addr, 0);
  }
}
```

Oke sip, selanjutnya kita perlu membuat objek `HttpContext` untuk server yang telah dibuat sebelumnya. `HttpContext` disini adalah representasi pemetaan antara URL dan layanan (konten HTML) yang akan diberikan pada URL tersebut. Objek `HttpContext` dibuat dengan menggunakan metode `createContext()`, yang disuplai dengan dua parameter, yaitu String URL dan objek implementasi `HttpHandler`. String URL disini menentukan dimana konten HTML akan disediakan, sedangkan objek implementasi `HttpHandler` adalah objek yang akan dieksekusi pada saat URL tersebut diakses.

`HttpHandler` sendiri adalah sebuah _interface_. Class yang mengimplementasikan interface ini perlu mendefinisikan metode `handle()`. Metode ini memiliki sebuah parameter `HttpExchange` yang merupakan enkapsulasi permintaan _(request)_ HTTP yang masuk dan balasan _(response)_ yang akan dikirim.

Langkah berikutnya adalah membuat class baru yang mengimplementasikan _interface_ `HttpHandler` dan mendefinisikan metode `handle()` yang terdapat didalamnya. Perhatikan contoh kode berikut :

```java
class SampleHttpHandler implements HttpHandler {
  @Override
  public void handle(HttpExchange exchange) {
    String content = "<!DOCTYPE html>";
    content += "<html>";
    content += "<body>";
    content += "<h1>Java HTTP Server</h1>";
    content += "<p>Quick Brown Fox Jumps Over The Lazy Dog !</p>";
    content += "</body>";
    content += "</html>";
    try {
      exchange.getResponseHeaders().set("Content-Type", "text/html");
      exchange.sendResponseHeaders(200, content.length());
      exchange.getResponseBody().write(content.getBytes());
      exchange.getResponseBody().close();
    }
    catch(IOException exc) {
      exc.printStackTrace();
    }
  }
}
```

Class tersebut dapat dibentuk secara langsung pada metode `createContext()`. Dan selebihnya kita dapat langsung memulai server HTTP dengan menggunakan metode `start()`.

```java
package com.sad301.exc;

import com.sun.net.httpserver.HttpServer;
import java.io.IOException;
import java.net.InetSocketAddress;

public class SampleHTTPServer {
  public static void main(String[] args) throws IOException {
    InetSocketAddress addr = new InetSocketAddress(8080);
    HttpServer server = HttpServer.create(addr, 0);
    server.createContext("/", new SampleHttpHandler());
    server.start();
    System.out.println("Server berjalan pada port 8080");
  }
}
```

Sip selesai !, selanjutnya anda cukup meng-_compile_ kode tersebut kemudian menjalankannya melalui aplikasi Command Prompt. Jalankan aplikasi browser yang anda miliki, pada address bar ketikkan alamat [http://127.0.0.1:8080](http://127.0.0.1:8080)