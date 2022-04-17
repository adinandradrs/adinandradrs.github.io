---
title: IBM Maximo – Automation Script Send Email
author:
  name: Adinandra Dharmasurya
  link: https://adinandra.dharmasurya.id
date: 2021-01-07 +0700
categories: [Pemrograman]
tags: [maximo,automationscript,custom]
pin: false
image:
  src: /etc/scripting.png
  width: 350   # in pixels
  height: 350   # in pixels
  alt: Scripting
---

Automation Script merupakan fitur custom development yang disediakan oleh platform IBM Maximo sejak versi 7.5 untuk mengakomodir proses yang membutuhkan scripting. Kenapa menggunakan pendekatan scripting? Tentu karena lebih fleksibel dibandingkan dengan menggunakan fitur workflow, apalagi jika orientasi kita lebih ke arah programming. Kebetulan saya pernah diminta oleh senior consultant untuk menyediakan fitur komunikasi untuk mengirimkan pesan email dari aplikasi work order.

Tulisan ini tidak membahas secara detail desain yang telah saya kerjakan karena rahasia kantor, namun saya ingin mengulas bagaimana inti teknis scripting mengirimkan pesan ke email melalui platform IBM Maximo. Sehingga jika ada yang membutuhkan tidak sulit untuk melakukan pengubahan sesuai dengan kebuthan masing-masing. Seharusnya tidak terlalu ribet karena cukup dasar dengan menggunakan syntax Jython. Berikut ini adalah Gist dari GitHub saya untuk send email via Automation Script.

{% gist 8be6d74ba348ccbb64ba86f1091e1366 %}

Pada snippet code di atas saya memberikan 2 contoh pengiriman email, yaitu untuk single destination email address dan multi destination email address. Single email cukup menggunakan variable berupa string sedangkan multi email dengan menggunakan array of string. Untuk mengirimkan cukup menggunakan method sendEMail. Untuk parameter diperlukan variabel email tujuan, email pengirim, judul email, dan body. Silakan dicustom sesuai kebutuhan jika ada yang memerlukannya, mari berbagi.