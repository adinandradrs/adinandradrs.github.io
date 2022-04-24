---
title: Go Starter dengan Gin Framework
author:
  name: Adinandra Dharmasurya
  link: https://adinandra.dharmasurya.id
date: 2022-01-30 +0700
categories: [Pemrograman]
tags: [go,gin,startup]
pin: false
image:
  src: /etc/api.png
  width: 450   # in pixels
  alt: Scripting
---

Tentunya bagi seorang pengembang yang akan mengawali project software engineering akan jauh lebih mudah ketika sudah ada base project yang menjadi acuan. Base project dapat dikatakan sebagai starter pack, a set of articles or equipment providing the essential items and instructions for taking up a particular activity or process for the first time. Bentuk dari starter pack dapat berupa SDK (Software Development Kit) yang terstandarisasi dan idealnya terdapat boiler. Dengan adanya SDK serta boiler tersebut pengembang dapat memahami pola serta mampu mengikuti standar yang sudah ada seperti struktur project, set framework, kumpulan pustaka yang siap digunakan, dan hal teknis lainnya.

Sebagai pemula di ranah Go, tentunya saya juga sempat mengalami kesulitan untuk memulai pekerjaan yang dituntut harus menggunakan bahasa pemrograman satu ini. Kesulitan paling awam yang menjadi challenge atau kesulitan utama adalah struktur project sebetulnya, karena ibarat membangun sebuah bangunan maka rangka awal dan struktur denah menjadi krusial ketika dirasa kurang tepat. Belajar dari pengalaman bekerja di kantor sebelumnya, saya merasa sangat nyaman dengan flavour Single Responsibility Principle yang diterapkan yaitu 1 class hanya punya 1 method public utama untuk dieksekusi. Method tambahan biasanya diakses secara private. Class tersebut biasanya berbentuk kata kerja (verb), semisal SignInService, AddUserService, atau UpdateRoleService.

Berhubung saya mulai melanjutkan project lama yang mulai dicicil untuk dikerjakan, sekalian bereksperimen dengan bahasa pemrograman Go. Untuk itu saya juga menyiapkan starter pack berbentuk SDK serta boilernya agar semua microservice yang nantinya akan saya kembangkan memiliki pattern serta standar yang sama. Meskipun dirasa SRP pada paragraf di atas kohesinya kurang di Go, namun selama dirasa nyaman dan aman tidak ada salahnya diterapkan. Sehingga baik Java atau Go yang saya gunakan memiliki pola pikir yang sama dan tidak terlalu banyak gap ketika berpindah stack. Untuk basenya sendiri saya menggunakan [Gin](https://github.com/gin-gonic/gin), [Jackc/PGX](https://github.com/jackc/pgx), dan [Go-Redis](https://github.com/go-redis/redis).

Untuk repositorynya dapat dicheckout pada link berikut :

- [boiler-go-common](https://github.com/adinandradrs/boiler-go-common) untuk base interface service, model entity, model request, dan model response yang digunakan secara general untuk membangun input (request), proses (service), serta output (response).
- [boiler-go-service](https://github.com/adinandradrs/boiler-go-service) untuk base interface repository, base interface adaptor, base config untuk caching dengan Go-Redis, base config untuk database dengan PGX, rest client, utils, serta constants yang generic dapat digunakan di semua microservice.

---

Untuk penggunaannya dapat dilihat pada boiler plate yang saya jadikan contoh, yaitu pada repository [omni-customer](https://github.com/adinandradrs/omni-customer). Berikut beberapa penjelasan dari struktur projectnya yang dibangun. Saya lengkapi juga dengan snippet code yang kira-kira menjadi base implementasinya.

- **Configuration**, digunakan untuk konfigurasi terhadap aplikasi misal seperti load environment, koneksi database, konfigurasi log, dan konfigurasi lainnya.
- **Model**
    * **Entity** digunakan untuk kumpulan file Go yang memiliki struct untuk merepresentasikan table dan projection.
    * **File struct** request response yang merepresentasikan input dan output dari suatu proses dalam 1 file Go.

{% gist bf4fb49a93f623445c36d2bcf80013aa %}

- **Repository**, digunakan untuk akses data (DAO) yang memiliki turunan capsule dari base interface repository. Dibentuk menggunakan struct capsule serta interface untuk dependency injection yang harus harus diimplementasikan.

{% gist f02cf32243a740cd48be06c2fabc8a86 %}

- **Service**, digunakan sebagai business logic per fungsi yang spesifik. Berupa interface turunan base service yang harus diimplementasikan.

{% gist 5ca1f8e1f75876a7c1d847d172b1e873 %}

- **Controller**, digunakan sebagai prosesor input dan memberikan respon dalam bentuk view ataupun model

{% gist bf1069b00d8cdc7e0d7f710a88a0ec5a %}

Dari contoh snippet di atas dapat diambil kesimpulan bahwa semuanya serba interface. Untuk melakukan injection dapat melakukan instansiasi object dengan memanggil fungsi New saat pertama kali start-up aplikasi. Kira-kira mirip dengan proses di Spring Boot. Kenapa ketika pertama kali? Karena pattern dari Dependency Injection salah satunya adalah singleton patterns sehingga dapat sekali membuat object dapat dipakai ke semua fungsi yang kira-kira dibutuhkan. Untuk literasinya dapat dilihat pada Wikipedia berikut. Terlampir snippet codenya.

{% gist fc8140701ac38dd2a41896124a4b378b %}

Sebagai penutup saya memberikan kesimpulan bahwa dengan pattern yang kita bentuk sendiri diharapkan menjadi penunjang produktivitas agar lebih menghemat waktu. Dan saya sendiri juga lebih memilih untuk menggunakan pustaka yang bertebaran di internet daripada membuat sendiri. Efisiensi dan estimasi effort juga harus diperhitungkan ketika membangun sebuah fondasi. Semoga tulisan ini bermanfaat dan dapat memberikan gambaran, silakan jika ada yang ingin menggunakan source code dari repository di atas sebagai base project. Jika ada masukkan juga diperbolehkan, masukkan yang membangun juga diperlukan sebagai bahan pertimbangan. Terimakasih.