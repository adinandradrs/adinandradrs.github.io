---
title: Koneksi Database dengan ASP Classic
author:
  name: Adinandra Dharmasurya
  link: https://adinandra.dharmasurya.id
date: 2014-03-02 +0700
categories: [Pemrograman]
tags: [asp,sql]
pin: false
image:
  src: /etc/scripting.png
  width: 350   # in pixels
  height: 350   # in pixels
  alt: Scripting
---

ASP Classic merupakan scripting yang cukup menjadi pada trend tahun 1998 - 2000an. Aplikasi dan pengembangnya sudah langka untuk ditemukan di era sekarang. Bahasanya cukup unik yaitu VBS dan JScript dan tampak seperti OOP. Saya sendiri menggunakannya karena ketika masih kuliah diminta untuk maintenance serta tuning website universitas yang berbasis ASP Classic. Sekarang sih sudah full migrasi ke PHP sejak tahun 2010 akibat dihack dengan SQL Injection hingga mengakibatkan data loss. Sedikit cerita sebelumnya ada wacana untuk menggunakan ORM atau minimal update base SQL dengan prepared statement, tetapi karena keterbatasan waktu serta tenaga akhirnya dibiarkan dan justru berakhir fatal.

Pada tulisan ini saya ingin menuliskan tentang bagaimana cara melakukan koneksi ke sebuah database dengan ASP Classic. Umumnya bahasa pemrograman VBS didukung koneksi native oleh MS SQL Server sedangkan untuk ke database lain seingat saya harus menggunakan protokol ODBC (Open Database Connectivity). Saya sendiri hampir tidak pernah menggunakan protokol tersebut, tetapi berhubung tertarik untuk membuat scripting service berbasis Classic ASP dengan database yang universal maka membutuhkan ODBC. Untuk enable ODBC dapat dikonfigurasi terlebih dahulu berdasarkan sistem operasi. Berikut adalah script koneksi menggunakan protokol ODBC agar dapat terhubung dengan database MySQL.

```
<% 
    Dim odbcStr
    odbcStr = "DRIVER={MySQL ODBC 3.51 Driver}; SERVER=$your_host; DATABASE=$your_schema; UID=$your_username;PASSWORD=$your_password; OPTION=3" 
    
    Dim dbOdbc 
    Set dbOdbc = Server.CreateObject("ADODB.Connection") 
    dbOdbc.Open(odbcStr) 
%>
```

Beberapa parameter yang harus disesuaikan adalah sebagai berikut : 

1. Host database, silakan ganti variabel ```$your_host``` dengan alamat host
2. Skema database, silakan ganti variabel ```$your_schema``` dengan nama skema
3. User database, silakan ganti variabel ```$your_username``` dengan username
4. Password database, silakan ganti variabel ```$your_password``` dengan kata sandi

Bagaimana dengan portnya? Karena menggunakan ODBC maka slot port sudah dibook berdasarkan DSN pada server sehingga tidak perlu didefinisikan. Untuk penggunaannya dapat simpan script di atas sebagai VBS dan include pada file VBS yang dipakai untuk aktivitas CRUD. Lebih baik dilakukan open koneksi sekali saja dengan pola singleton dan sisanya bermain pool agar meminimalkan aktivitas open close yang kurang bijak. Saya sendiri tidak yakin apakah teknologi ADO di VBS menggunakan versi yang sama dengan ADO versi .NET di mana telah menerapkan pooling. Khawatirnya berbeda COM. Semoga bermanfaat dan terimakasih.

*Disadur dari blog lama saya di WordPress.com*