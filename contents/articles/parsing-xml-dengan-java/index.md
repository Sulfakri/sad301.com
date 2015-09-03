---
title: Parsing XML Dengan Java
author: sad301
date: 2013-12-02
template: article.jade
---

Pada postingan kali ini, saya akan menuliskan sedikit tentang salah satu teknik parsing XML dengan menggunakan bahasa pemrograman java. Saya tidak akan menjelaskan secara mendetail tentang apa itu XML, selengkapnya bisa anda baca melalui website-website berikut :

* http://en.wikipedia.org/wiki/XML
* http://www.w3schools.com/xml/

Parsing sendiri adalah sebuah istilah yang merujuk kepada proses ekstraksi data/informasi dari sebuah struktur dokumen. Sederhananya, proses parsing XML dilakukan dengan mentransformasikan file XML menjadi Document Object Model (DOM). Dari DOM tersebut kita dapat melakukan penelusuran _(traverse)_ di tiap-tiap node hingga menemukan data yang diinginkan. Sebagai contoh, semisal kita memiliki file XML dengan struktur berikut :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<daftar-kontak>
  <kontak id="1">
    <nama>Ahmad</nama>
    <alamat>Kendari</alamat>
    <phone>12345</phone>
  </kontak>
  <kontak id="2">
    <nama>Budi</nama>
    <alamat>Makassar</alamat>
    <phone>90876</phone>
  </kontak>
  <kontak id="3">
    <nama>Dharmawan</nama>
    <alamat>Surabaya</alamat>
    <phone>213423</phone>
  </kontak>
</daftar-kontak>
```

Untuk mengambil data dari file tersebut, saya menggunakan kode java seperti dibawah ini :

```java
package com.sad301.exc;

import java.io.File;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

public class XMLParsing {

  public static void main(String[] args) {
    new XMLParsing();
  }

  public XMLParsing() {
    try {
      tryParse();
    }
    catch(Exception exc) {
      exc.printStackTrace();
    }
  }

  private void tryParse() throws Exception {
    File file = new File("daftar-kontak.xml");
    DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
    DocumentBuilder db = dbf.newDocumentBuilder();
    Document d = db.parse(file);
    // ambil root node dari DOM
    Element elRoot = d.getDocumentElement();
    // ambil child node dari root dengan tag <kontak>
    NodeList nlKontak = elRoot.getElementsByTagName("kontak");
    // loop disemua child node
    for(int i=0; i<nlKontak.getLength(); i++) {
      Element elKontak = (Element)nlKontak.item(i);
      NodeList nlNama = elKontak.getElementsByTagName("nama");
      NodeList nlAlamat = elKontak.getElementsByTagName("alamat");
      NodeList nlPhone = elKontak.getElementsByTagName("phone");
      String id = elKontak.getAttribute("id");
      String nama = nlNama.item(0).getTextContent();
      String alamat = nlAlamat.item(0).getTextContent();
      String phone = nlPhone.item(0).getTextContent();
      String str = id + ":" + nama + ":" + alamat + ":" + phone;
      System.out.println(str);
    }
  }
}
```

Dan hasilnya adalah sebagai berikut :

![text](https://lh3.googleusercontent.com/-LZTopR60ETo/UpyAO2RKmII/AAAAAAAAKsw/9q1sAOQ1ff8/s2048-Ic42/sample.png)

Referensi :

* http://docs.oracle.com/javase/7/docs/api/
* http://www.mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/
