---
title: Joinfaces JSF Spring
author:
  name: Adinandra Dharmasurya
  link: https://adinandra.dharmasurya.id
date: 2019-08-17 +0700
categories: [Pemrograman]
tags: [jsf,java,spring]
pin: false
image:
  src: /etc/scripting.png
  width: 350 # in pixels
  height: 350 # in pixels
  alt: Scripting
---
4 tahun saya tidak menyentuh Java Server Face (JSF) karena memang kebetulan di Indonesia sudah menurun popularitas serta permintaannya. Terakhir saya handle development sistem menggunakan JSF adalah ketika mengerjakan modul psikotest untuk rekrutmen karyawan di sebuah perusahaan kosmetik yang terintegrasi dengan Moodle. Apabila ada yang belum mengetahui apa itu JSF berikut adalah penjelasan singkatnya. **JSF adalah sebuah framework MVC** untuk mengembangkan aplikasi web dengan user interface (UI) pada server-side. Sehingga UI dengan fungsi standar (View) serta proses backendnya (Model & Controller) berada di dalam satu wadah besar.

Bagi mereka yang pernah menyentuh aplikasi web berbasis ASP.NET mungkin tidak merasa asing dengan gaya development JSF. Setiap komponen pada canvas atau view adalah berupa tag element dengan format XML. Di mana pada tag elemen tersebut deklarasi komponen, event, dan data dapat dibinding dengan class model dan controller. JSF di mata saya masih sangat seksi, menggoda, dan kompak. Di mana saya benar-benar dapat fokus mengerjakan sebuah proyek di dalam satu workspace. Tidak memperlukan ekstra proses komunikasi antara klien dan server. Saya mampu berkonsentrasi pada bisnis proses. Versi terakhir dari JSF yaitu adalah versi 2.3 dengan dukungan Java 8.

> Apakah JSF sulit untuk dipelajari dan digunakan? Menurut saya tidak. Sudah ada standarnya dan lebih simple dibandingkan arsitektur jaman sekarang yang semakin kompleks karena proses komunikasi menggunakan web service.

Standar komponen, penggunaan fungsi, dan dokumentasi dapat merujuk di [situ](https://www.oracle.com/topics/technologies/jsf.html), di [sini](https://javaee.github.io/javaserverfaces-spec/) dan di [sana](https://docs.oracle.com/javaee/6/javaserverfaces/2.1/docs/vdldocs/facelets/). Berbicara tentang komponen, UI kit vendornya pun beragam sehingga memiliki banyak opsi. Untuk menghias tampilan turut dipermudah dengan komponen-komponen yang sudah disediakan pada palette. Komponen-komponen apa saja yang ada pada pallete tersebut? Sebetulnya tergantung dari vendornya karena setiap pengembang memiliki masing-masing tema serta kelebihannya. Secara umum serta out of the box JSF sudah menyediakan komponen input, data grid atau table, panel, fungsi, dan helper lainnya yang sesuai standar JSR dari JCP.

> Yang perlu dipelajari adalah lifecyclenya dan jenis bean (termasuk dengan scopenya) yang dapat dibaca pada website [berikut](https://www.tutorialspoint.com/jsf/jsf_life_cycle.htm).

Sama seperti .NET yang didukung beberapa penyedia komponen seperti Telerik (KendoUI), DevExpress, Metro UI, dan lain sebagainya. JSF juga didukung oleh banyak vendor, baik yang open source hingga yang berbayar. Untuk framework aplikasi berbasiskan JSF yang sempat hits adalah ZKoss yang seingat saya diextend dengan format *.zul dan Vaadin. JSF sendiri pernah salah satu trend setter karena di tahun 2010 sedang marak tentang **RIA** (Rich Internet Application). Pihak ketiga yang mendukung komponen JSF secara native di antaranya adalah :

1. [Apache MyFaces (Tobago)](https://myfaces.apache.org/#/)
2. [RedHat RichFaces](https://richfaces.jboss.org/)
3. [IceFaces](http://www.icesoft.org/java/projects/ICEfaces/overview.jsf)
4. [ButterFaces](http://butterfaces.org/)
5. [Primefaces](https://www.primefaces.org/)
6. Dan lainnya.

Sedangkan utility yang sering saya gunakan adalah [OmniFaces](https://omnifaces.org/) seperti untuk validator input, cookie, session, dan lain sebagainya. Dari beberapa komponen di atas saya selalu menggunakan RichFaces karena sudah menjadi pustaka standar di kantor lama. Di kantor sekarang ada beberapa proyek yang juga menggunakan teknologi JSF dan dianggap sebagai barang legacy. Sedih memang karena nasibnya tergerus framework lain yang lebih lengkap serta kekinian seperti Spring. Selain itu penerapan karir, arsitektur, dan teknologi yang terpisah antara backend dengan frontend juga membuat JSF semakin sekarat. *Selamat tinggal fullstack*~

> Penerapan kustom tampilan serta fungsi yang lebih jauh cenderung sulit apabila menggunakan JSF, sehingga dulu banyak user yang komplain terkait tampilan yang kaku dan seperti aplikasi desktop. Untuk Spring menurut saya daripada sekedar framework maka lebih cocok dikatakan sebagai toserba. Apa yang kita butuhkan di situ sudah tersedia semua. 
{: .prompt-info }

Di sela kesibukan, beberapa waktu yang lalu saya mencoba membuka barisan koding di harddisk eksternal. Saya rindu dengan RichFaces tetapi tidak dengan JSFnya. Ditambah sudah hampir 3 tahun dimanjakan dengan Spring Boot dengan berbagai kemudahannya sehingga tidak mungkin saya akan menggunakan JSF. Akhirnya saya iseng mencari apakah tetap dapat menggunakan RichFaces untuk menjadi layer di depan. Pada umumnya Spring Boot menggunakan template engine Thymeleaf atau kustom dengan Apache Velocity sebagai view. Setelah mencari di mesin pencarian saya pun menemukan [Joinfaces](http://joinfaces.org/), sebuah proyek open source yang melakukan "porting" JSF ke dalam Spring Boot.

Porting yang dilakukan memungkinkan saya untuk menggunakan seluruh komponen JSF dan Spring Boot di dalam satu aplikasi. Jelas paling mantap adalah dapat menggunakan IoC Spring di dalam JSF (*and vice versa*). Sebagai contoh saya dapat membuat interface service atau repository lalu diinject ke dalam managed bean tanpa perlu ada konfigurasi lebih lanjut di container terkait scanning class secara manual. CDI pada JSF pun sejauh yang saya coba dapat diresolve tanpa ada kendala konflik dengan komponen yang ada di Spring Boot. Lalu bagaimana cara untuk memulai setup proyek dengan Joinfaces? Di artikel berikut saya mencatat bagaimana cara settingnya.

> Sebagai informasi tambahan secara out of the box Joinfaces sudah dibundle dengan beberapa UI kit dan siap digunakan.

## Environment

Untuk JDK di lokal yang digunakan adalah versi 11 sehingga pada Maven target runtime pun saya set menjadi versi 11. Versi JDK yang minimal harus terinstall berdasarkan dokumentasi adalah versi 8. Sehingga bagi yang menggunakan versi di bawah minimal sebaiknya melakukan update atau install JDK terbaru secara terpisah. Pastikan menggunakan IDE yang standar untuk memulai proyek Maven, saya sendiri menggunakan Visual Code.

## Setup Maven

Membuat proyek dengan Maven adalah hal pertama yang harus dilakukan. Di mana yang perlu diset terlebih dahulu adalah pada bagian properties. Untuk target runtime semua saya sesuaikan dengan JDK yang ada di lokal yaitu JDK 11. Semua versi pustaka yang dibutuhkan juga saya letakkan pada properties. Tidak lupa saya sertakan parent proyek yaitu menggunakan Spring Boot dengan versi 2.3.

```
<modelVersion>4.0.0</modelVersion>
<groupId>id.codefun.omni.jsf</groupId>
<artifactId>omni-jsf-sample</artifactId>
<version>1.0</version>
<packaging>jar</packaging>

<name>Service JSF</name>
<description>Sample Omni with JSF</description>

<properties>
  <maven.compiler.source>11</maven.compiler.source>
  <maven.compiler.target>11</maven.compiler.target>
  <java.version>11</java.version>
  <joinfaces.version>4.3.8</joinfaces.version>
  <jaxb-core.version>3.0.1</jaxb-core.version>
  <jaxb-api.version>2.3.1</jaxb-api.version>
  <jstl.version>1.2</jstl.version>
  ...
</properties>

<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>2.3.10.RELEASE</version>
  <relativePath/>
</parent>
```

Perlu diperhatikan terkait dependency untuk menguduh pustaka karena banyak pustaka yang dibutuhkan oleh compiler dan runtime ketika menjalankan web berbasis JSF. Di antaranya adalah pustaka seperti JAX, Servlet API, serta JSTL. Turut diunduh pustaka untuk operasi pada servlet apabila dibutuhkan bermain di low level. Untuk running aplikasi saya tetap menggunakan bawaan dari Spring Boot yaitu Tomcat, apabila ingin menggunakan web server atau application server lainnya dapat menyesuaikannya pada Mavennya.

```
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.joinfaces</groupId>
      <artifactId>joinfaces-dependencies</artifactId>
      <version>${joinfaces.version}</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>
  </dependencies>
</dependencyManagement>

<dependencies>
  <dependency>
    <groupId>org.joinfaces</groupId>
    <artifactId>richfaces-spring-boot-starter</artifactId>
  </dependency>
  <dependency>
    <groupId>javax.enterprise</groupId>
    <artifactId>cdi-api</artifactId>
    <scope>provided</scope>
  </dependency>
  <dependency>
    <groupId>javax.xml.bind</groupId>
    <artifactId>jaxb-api</artifactId>
    <version>${jaxb-api.version}</version>
    <scope>provided</scope>
  </dependency>
  <dependency>
    <groupId>com.sun.xml.bind</groupId>
    <artifactId>jaxb-core</artifactId>
    <version>${jaxb-core.version}</version>
    <scope>provided</scope>
  </dependency>
  <dependency>
    <groupId>com.sun.xml.bind</groupId>
    <artifactId>jaxb-impl</artifactId>
    <version>${jaxb-core.version}</version>
    <scope>provided</scope>
  </dependency>
  <dependency>
    <groupId>javax.activation</groupId>
    <artifactId>activation</artifactId>
    <version>1.1.1</version>
  </dependency>
  <dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>servlet-api</artifactId>
    <version>2.5</version>
    <scope>provided</scope>
  </dependency>
  <dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
    <version>${jstl.version}</version>
    <scope>provided</scope>
  </dependency>
  ...
</dependencies>
```

Setelah setting Maven dapat dicoba untuk melakukan clean project dan inisialisasi serta download library dengan menggunakan perintah ```mvn clean install```. Perlu diperhatikan apakah perintah tersebut gagal atau berhasil, dari pengalaman ketika mencoba rata-rata gagal karena ada beberapa pustaka yang ketinggalan dan perlu diunduh pada dependency.

## Konfigurasi YAML

Pada folder resource saya membuat sebuah file YAML untuk konfigurasi Spring Boot dengan ```application.yml```. Pada file konfigurasi tersebut perlu mendefinisikan beberapa konfigurasi yang dibutuhkan untuk setup Joinfaces. Beberapa property joinfaces yang diperlukan pada YAML tersebut adalah sebagai berikut :

**application.yml**

```
joinfaces:
  richfaces:
    base-skin: ruby
    skin: ruby
```

Pada konfigurasi di atas saya setup UI kit yang digunakan adalah dengan RichFaces. Tema atau skin default pada RichFaces yang saya gunakan yaitu Ruby. Namun apabila ingin menggunakan UI kit yang lain dapat melihat pada dokumentasi yang ada pada Joinfaces terkait pengaturan vendor komponen. Untuk pengaturan server disetup pada konfigurasi terpisah dengan value yang tidak dapat dioverride ketika running.

**bootstrap.yml** 

```
server:
  port: 8080
spring:
  application:
    name : "omni-jsf-sample"
```

Untuk port dan application name dapat disesuaikan dengan kebutuhan serta keadaan setiap environment.

## Set Tomcat Handler

Ketika mencoba menjalankan aplikasi saya terkena exception pustaka tidak dapat diresolve, seingat saya adalah pustaka JAXB. Untuk web server saya menggunakan default Spring Boot yaitu Tomcat. Agar Tomcat (atau mungkin web server lainnya) dapat berfungsi dengan normal maka dibutuhkan handler agar dapat resolve pustaka *.jar yang ada pada JRE atau JDK. Ada 2 class yang perlu dibuat sebagai konfigurasinya. 

1. ```TomcatServletWebServerFactoryConfiguration``` pada package configuration yang inherit dari class ```TomcatServletWebServerFactory```, digunakan untuk override method ```postProcessContext``` yang melakukan scan dan set property TLD pustaka JAXB agar dapat diresolve.
2. ```TomcatConfiguration``` pada package configuration, digunakan untuk load object dari class ```TomcatServletWebServerFactoryConfiguration``` sebagai **default factory Tomcat web server** pada container ketika aplikasi running.

Berikut adalah snippet code di Gist

**TomcatServletWebServerFactoryConfiguration.java**

{% gist 38b2cb95e1434590bc726e409e39b15d %}

**TomcatConfiguration.java**

{% gist a239289aefd00f5ec5f1e5f9442c48a0 %}

## Memulai Coding

Setelah ritual pada setiap section selesai saya eksekusi maka sudah waktunya mencoba coding untuk menampilkan sebuah halaman JSF sederhana. Yang pertama harus dilakukan adalah membuat main class, sample terlampir pada Gist berikut :

{% gist 41eabf67d3cc33d81dca3fb66cb150e0 %}

Setelah main class selesai maka selanjutnya coding sebuah halaman index dengan nama ```index.xhtml``` yang dapat diakses pada root path semisal ```http://host:port/index.jsf```. File xhtml tersebut saya buat pada direktori ```${project_path}/resources/META-INF/resources/```. Pada halaman tersebut nanti akan melakukan render sebuah panel beserta judulnya seperti terlampir. Untuk running aplikasi dapat menggunakan perintah dengan Maven sebagai berikut ```mvn spring-boot:run```

![](https://lh3.googleusercontent.com/pw/AM-JKLWH7mkt0jZYuUCoEGgOnW3_73-yv1_05eYUc4RB87S6xaOJt_HpBUa4I9p7nG7ZIVXt_jaEwi-wOU1ihz5QttG3rGhx9-zXrQf5LmzjrRn5KFM3QjTI6EQpvndKS1-8PVyhn7-CqxFWHz6GjnOLJLtXyQ=w800-no?authuser=0)

Untuk melakukan rendering halaman seperti di atas terlampir Gist untuk index.xhtml. Format yang digunakan pada XHTML mirip dengan HTML, namun terdapat namespace yang harus didefinisikan berdasarkan vendor UI kit pada Joinfaces. Sebagai contoh saya menggunakan RichFaces dengan ```xmlns:rich```. Untuk namespace standar JSF yang wajib digunakan adalah ```xmlns:h="http://xmlns.jcp.org/jsf/html"``` untuk render komponen HTML dan ```xmlns:f="http://xmlns.jcp.org/jsf/core"``` untuk menggunakan fungsi dari framework JSF di HTML.

{% gist e664bf1edfa9881b97574b99d77af368 %}

Lalu bagaimana cara melakukan binding data dengan Joinfaces? Dengan Spring Boot saya dapat melakukannya dengan ala "Baeldung" seperti pada Gist berikut.  Dengan membuat sebuah package beans dan class bernama ```IndexBean```. Pada contoh sederhana berikut saya menggunakan Lombok sehingga apabila tidak kompatibel, kode di bawah ini dapat disesuaikan dengan keadaan.

{%gist 1d2ec2dca51cbcb93d7ae930a939c5e5 %}

Untuk di halaman web kita dapat menambahkan tag label di antara panel yang valuenya adalah binding field content pada class ```IndexBean``` seperti baris kode terlampir.

```
<rich:panel style="width:100%;" header="This is a panel - Code4fun" headerClass="panel-header" bodyClass="login-body">
	<h:outputText value="#{indexBean.getContent()}" />
</rich:panel>
```

Hasilnya akan menampilkan data seperti gambar di bawah ini.

![](https://lh3.googleusercontent.com/pw/AM-JKLUfx2hRpDJV637elZBUWDQdfTOdwBuBoviSWzg0phPepHIFoxM4fXbWEDTyWC37VM720vkXV2NSZxKAH-bLgMe7bkA5wKyo1CWKuPz9iUxdn0QFO_vBGxDT9JM0DYIagagCNeVkNUOxAEZIg3ULzO4p8Q=w800-no?authuser=0)

## Kesimpulan

Dari semua tahapan di atas saya dapat mengimplementasikan Joinfaces dengan Spring Boot untuk menggunakan JSF sebagai view. Dengan menggunakan Spring Boot sebagai backend maka binding data serta fungsi pada view pun dapat lebih kekinian dengan IoC serta dekoratornya yang sudah disediakan oleh Spring. Apabila lebih nyaman dengan gaya old school JSF bean yang menggunakan anotasi tipe bean dan scopenya, dengan Joinfaces saya masih dapat membuat class bean old school dan teresolve dengan baik. Termasuk EL dari view juga mampu dibinding ke dalam bean class, seperti misalnya untuk keperluan menangkap event pagination dari tabel meskipun jujur belum mencoba lebih jauh dengan fungsi kustom. 

Semoga artikel ini bermanfaat dan apabila ada masukkan boleh dishare. Terimakasih dan salam.

NB : *Artikel ini telah disunting kembali pada tanggal 11 Agustus 2021 sehingga apabila terdapat perbedaan versi pada pom.xml yang tampak lebih baru dari tanggal artikel harap maklum* 
