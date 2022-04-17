---
title: IBM Maximo – Automation Script Export Object to XML
author:
  name: Adinandra Dharmasurya
  link: https://adinandra.dharmasurya.id
date: 2022-03-05 +0700
categories: [Pemrograman]
tags: [maximo,automationscript,custom]
pin: false
image:
  src: /etc/scripting.png
  width: 350   # in pixels
  height: 350   # in pixels
  alt: Scripting
---

Pada posting berikut ini saya ingin share tentang bagaimana cara ekspor data dari IBM Maximo menjadi file XML dengan format bebas (user defined). Hal ini didasarkan oleh ketika terdapat kebutuhan semisal ingin melakukan data loading dari Maximo ke aplikasi lain namun tidak dalam bentuk flat file, melainkan dalam bentuk XML yang formatnya harus ada kesepakatan antara 2 belah pihak. Kita dapat melakukannya menggunakan Automation Script. Contoh jika ingin share data item, cukup memanfaatkan Automation Script dengan launch point action pada object item.

Beberapa hal yang cukup bermanfaat serta dapat kita mainkan adalah pustaka dari Java untuk parsing ke XML yaitu W3C DOM dan Java XML Parser. Pada Gist di bawah ini saya share coding Automation Script yang berisikan perintah untuk ekspor data ke file XML. Skeleton script berikut adalah catatan silam saya ketika implementasi aplikasi EAM IBM Maximo di project kantor lama sehingga jika ada yang membutuhkan dapat disesuaikan saja dengan kebutuhan yang bersangkutan. Penjelasan dapat dilihat pada bagian hashtag atau komentar.

{% gist 414530a86d7ab38e54906fe01e051c1b %}

Ekspektasi output dari eksekusi script di atas adalah sebagai berikut :

```
<?xml version="1.0" encoding="UTF-8">
<Item itemnum="XXXXXXXXX">
	<Description>YYYYYYYYYY</Description>
</Item>
```

Dari sini kita ambil kesimpulan bahwa XML Parser dari pustaka Java dapat digunakan secara out of the box di IBM Maximo. Sebetulnya tidak hanya pustaka XML Parser namun semua kapabilitas pustaka pada JDK yang bertugas sebagai runtime juga dapat dimanfaatkan serta dieksekusi oleh IBM Maximo. Mungkin akan sangat menarik serta mempermudah kita jika reactive streaming yang ada di Java 8 ke atas dapat digunakan pada scripting Automation Script. Untuk inovasi dan pengembangannya sendiri tergantung kreativitas dari masing-masing pengembangnya. Semoga bermanfaat, salam.