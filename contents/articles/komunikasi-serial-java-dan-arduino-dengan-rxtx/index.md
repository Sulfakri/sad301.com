---
title: Komunikasi Serial Java & Arduino Dengan RxTx
author: sad301
date: 2013-09-01
template: article.jade
---

Setelah sekian lama menganggur diatas meja kerja, akhirnya Arduino Uno milik saya bisa dioprek lagi. Misinya kali ini adalah untuk membuat perangkat ini berkomunikasi dengan aplikasi java dengan tujuan untuk menerima data dari perangkat tersebut.

## Model Uji Coba

Agar tidak memakan waktu terlalu lama, uji coba ini akan menggunakan model yang sederhana saja. Pada sisi Arduino, saya hanya menggunakan sebuah potentiometer. Nilai dari potentiometer tersebut akan dibaca melalui aplikasi java dan ditampilkan pada komponen `JTextArea`. Sebelum dapat membaca nilai dari arduino, user harus terlebih dahulu memilih port usb yang digunakan melalui komponen `JComboBox`.

## Arduino

Seperti yang sudah disebutkan sebelumnya, pada sisi arduino kita akan menggunakan sebuah potentiometer. Komponen potentiometer sendiri memiliki 3 buah kaki, untuk menggunakannya kita perlu menghubungkan salah satu kaki (paling kanan atau paling kiri) ke pin 5V dari arduino, kaki tengah ke pin analog A0, dan sisanya ke pin GND. Untuk lebih jelasnya perhatikan ilustrasi dibawah.

![text](https://lh3.googleusercontent.com/-jdXv8BxWcEA/UiF7pkzqteI/AAAAAAAAJrc/khVoVW4TD14/s2048-Ic42/Untitled%252520Sketch_bb.png)

Setelah menyambungkan potentiometer ke arduino, selanjutnya kita tinggal menuliskan kode sketch melalui Arduino IDE (download di [arduino.cc](http://arduino.cc/en/Main/Software)) dan menguploadnya ke perangkat. Berikut kode sketch yang saya gunakan :

```java
int potPin = A0;
int potValue;

void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
}

void loop() {
  // put your main code here, to run repeatedly:
  potValue = analogRead(potPin);
  Serial.print(potValue);
  Serial.print(",");
  delay(15);
}
```

## Konfigurasi RXTX

Sebelum masuk ke tahap coding program, kita terlebih dahulu harus melakukan konfigurasi library yang akan digunakan. Pada project ini kita akan menggunakan [RXTX](http://rxtx.qbang.org), yaitu library java yang memungkinkan komunikasi serial dan parallel antara perangkat dengan aplikasi java. Untuk penjelasan lebih lanjut, silahkan berkunjung ke [website resminya](http://rxtx.qbang.org).

Hal-hal yang perlu dilakukan adalah sebagai berikut :
1. Download library RXTX dari halaman [download](http://rxtx.qbang.org/wiki/index.php/Download)
2. Extract file zip yang telah didownload
3. Copy file `rxtxSerial.dll` dan `rxtxParallel.dll` ke folder `C:\Program Files\java\jre7\bin`
4. Copy file `RXTXcomm.jar` ke direktori `C:\Program Files\java\jre7\lib\ext`

## Program Java

Langkah terakhir adalah tahap coding program. Karena kode programnya lumayan panjang, saya hanya sebutkan bagian-bagian pentingnya saja. Pertama yang akan dibutuhkan oleh program adalah daftar port yang tersedia, kutipan kode untuk bagian ini adalah sebagai berikut :

```java
Enumeration portEnum = CommPortIdentifier.getPortIdentifiers();
CBModelCommPortId model = new CBModelCommPortId(portEnum);
```

Daftar port yang sudah didapatkan akan ditampilkan dalam komponen `JComboBox`. Setelah user memilih port yang akan digunakan, selanjutnya tinggal menekan tombol yang akan mengeksekusi metode `startConnection()`. Kutipan kodenya adalah sebagai berikut :

```java
private void startConnection() {
  try {
    String portId = (String)cbCommPortId.getSelectedItem();
    CommPortIdentifier commPortId = CommPortIdentifier.getPortIdentifier(portId);
    CommPort commPort = commPortId.open(getClass().getName(), 2000);
    if(commPort instanceof SerialPort) {
      serialPort = (SerialPort)commPort;
      serialPort.setSerialPortParams(9600, SerialPort.DATABITS_8, SerialPort.STOPBITS_1, SerialPort.PARITY_NONE);
      isOpened = true;
      in = serialPort.getInputStream();
      serialPort.addEventListener(this);
      serialPort.notifyOnDataAvailable(true);
    }
  }
  catch(Exception exc) {
    taData.append(exc.toString()+"\n");
    bConnect.setSelected(false);
  }
}
```

Kemudian jika tombol yang sama ditekan lagi akan mengeksekusi metode `stopConnection()`. Kutipan kode dari metode tersebut adalah sebagai berikut :

```java
private void stopConnection() {
  if(isOpened) {
    serialPort.close();
    serialPort.removeEventListener();
  }
}
```

Terakhir, karena class java yang dibuat mengimplementasikan _interface_ `SerialPortEventListener`, kita perlu mendefinisikan metode `serialEvent()` dari interface tersebut. Kutipan kode yang saya gunakan adalah sebagai berikut :

```java
public void serialEvent(SerialPortEvent arg0) {
  // TODO Auto-generated method stub
  int data;
  try {
    int len = 0;
    while((data=in.read()) > -1) {
      if(data==',') {
        break;
      }
      buffer[len++] = (byte)data;
    }
    taData.append(new String(buffer, 0, len) + "\n");
  }
  catch(IOException exc) {
    taData.append(exc.toString() + "\n");
  }
}
```

Berikut adalah cuplikasi gambar dari hasil eksekusi program

![text](https://lh3.googleusercontent.com/-VN3OqLmkCf4/UiGYe7Ql9wI/AAAAAAAAJrs/pE-2KbpJBFE/s2048-Ic42/Untitled.png)

Kode program selengkapnya dapat dilihat di halaman [Github](https://github.com/sad301/SerialExc)
