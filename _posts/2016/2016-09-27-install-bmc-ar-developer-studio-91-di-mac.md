---
title: Install BMC AR Developer Studio 9.1 di Mac
author:
  name: Adinandra Dharmasurya
  link: https://adinandra.dharmasurya.id
date: 2016-09-27 +0700
categories: [Teknologi]
tags: [remedy,mac,arsys]
pin: false
image:
  src: https://lh3.googleusercontent.com/pw/AM-JKLWf9ob6xoxBTdLHjzpX-PrwMiHxaiCAhve0h2bB4Wd8IN05Am2NNcVVJ31koLiT401aWAipMeJPSqeJECm7FypH3K_U3yoTTtDz6pFQoW8MoIPcPX--fyJIaqKJ93OMMKjcwchpkOzu7Qn2zE7NzunS1w=w1280-h800-no?authuser=0
  width: 600   # in pixels
  alt: BMC AR Developer
---

Akhirnya saya dapat install BMC AR Developer Studio di Mac OS dan menjalankannya secara native. MacBook saya terinstall OS X El-Capitan. Pada dasarnya BMC AR Developer Studio adalah sebuah IDE yang berbasiskan Eclipse. Jadi untuk menyeusaikan versi 9.1 setidaknya saya membutuhkan Eclipse dengan versi 4.5 atau dengan kode "Mars"

Ide orisinil saya mendapatkannya dari forum BMC di [sini](https://communities.bmc.com/ideas/2032). Lalu bagaimana cara saya melakukan instalasinya? Posting ini juga berfungsi sebagai backup seandainya artikel pada forum tersebut dihapus atau hilang :
1. Memiliki BMC AR Developer Studio yang telah terinstall di mesin Windows
2. Unduh Eclipse versi 4.5 untuk Mac (64 bit cocoa)
3. Extract dan rename app dari ```Eclipse.app``` misal menjadi ```ARDeveloperStudio.app```, jangan ada spasi karena dapat membuat Eclipsenya tidak tereksekusi nantinya
4. Klik kanan pada ```ARDeveloperStudio.app``` dan akses sub-menu "Show Package Contents"
5. Masuk ke folder Contents ▶︎ Eclipse ▶︎ dropins
6. Pada BMC AR Developer Studio di mesin Windows, copy folder plugin dan features lalu paste di folder dropins pada step nomor 5
7. Pindah ke folder Contents ▶︎ Eclipse ▶︎ configuration. Buka config.ini menggunakan text editor dan modifikasi section osgi.splashPath seperti berikut ```osgi.splashPath=platform\:/base/dropins/plugins/com.bmc.arsys.studio.ui```
8. Selesai sudah, untuk pemanis kita dapat mengganti icon launcher Contents ▶︎ Resources ▶︎ Eclipse.icns dengan icon yang kita inginkan.

Berikut adalah screenshot BMC AR Developer Studio di MacBook saya

![](https://lh3.googleusercontent.com/pw/AM-JKLXYVAUxrEeDdRjhm_Kp6lhmNRWE48wEvpgkdZ7dpB2U2yUpzoTNJk4LGy3sftHJQnDDJ0Qo9Ybe6bEguKP-h7j1eHQz6ovZqoHeDD944VQLAQ81mW-5UfyXedatTgKZFR1d0u4JmI3euX5bf4kxy75xSw=w800-no?authuser=0)

Credit to Andres, thanks for your feedback :
> You would need to run the eclipse executable with the -clean option on the first run. it will cleanup and reload all plugins. For Mac, go inside the eclipse package and run the eclipse executable (at the ```Contents/MacOS``` Folder) from a Terminal window

*Disadur dari blog lama saya di WordPress.com*