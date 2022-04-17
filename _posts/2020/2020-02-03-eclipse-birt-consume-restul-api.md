---
title: Eclipse Birt Consume RESTful API
author:
  name: Adinandra Dharmasurya
  link: https://adinandra.dharmasurya.id
date: 2020-02-03 +0700
categories: [Pemrograman]
tags: [birt,webservice,reporting]
pin: false
image:
  src: /etc/visual-data.png
  width: 350   # in pixels
  height: 350   # in pixels
  alt: Data Visual
---

Pada artikel ini saya ingin membahas tentang bagaimana BIRT (Business Intelligence Reporting Tools) sebuah platform open source untuk reporting dari Eclipse dapat memanfaatkan JSON dari REST API sebagai sumber data. Untuk lebih mengenal BIRT dapat mengunjungi halaman ini dan ini. Sebenarnya kenapa memilih BIRT adalah suatu pertimbangan yang relatif dan berikut adalah beberapa dari alasan tersebut :
1. BIRT merupakan platform yang dapat berdiri sendiri alias stand-alone.
2. Memiliki viewer yang web-native dengan teknologi Java dan dapat membuka file report tanpa perlu compile seperti Jasper.
3. Web viewer ini dapat menjadi solusi untuk memiliki feature atau komponen yang loose coupling sehingga setidaknya memiliki rasa “Microservice”
4. Secara koding tidak terlalu banyak menulis kode di sisi backend dan tidak dari nol karena dapat menggunakan berbagai macam data source yang sudah tersedia secara out of the box.
5. The most thing that I love about the report design is XML based.

Karena saat ini banyak aplikasi yang sedang hype untuk penggunaan REST API, muncul rasa penasaran
> Apakah BIRT dapat menggunakan JSON dari REST API sebagai data source seperti yang saya sampaikan di awal artikel?
{: .prompt-warning }

Secara eksplisit pada website Eclipse BIRT mungkin tidak dijelaskan mendetail tentang kapabilitas ini, namun ternyata BIRT memiliki kemampuan untuk mengakomodirnya yaitu dengan komponen ***Scripted Data Source***.

---

## Development
Untuk studi kasusnya akan menghasilkan report yang menampilkan data dari sebuah REST API dengan URL (akses menggunakan metode POST dengan security token tentunya) :
```
[POST] http://localhost:1081/api/sample/v1/get-revenue
```
Data yang diperoleh dari REST URL tersebut berbentuk JSON seperti di bawah ini :
```
{
	data : [
		{id : 1, customer: 'PT Brantas', revenue : '18500000000.00'},
		{id : 2, customer: 'PT Ardhaya Bima', revenue : '22305000000.00'},
		{id : 3, customer: 'CV Satelit Inovasi', revenue : '1530000105.00'},
		{id : 4, customer: 'PT Indo Persada', revenue : '71200002400.00'},
		{id : 5, customer: 'PT Malika Sari', revenue : '25100300.00'},
		{id : 6, customer: 'CV Rempoa Kholis', revenue : '1400502.00'}
	]
}
```
Untuk memulainya kita dapat mengunduh designer Eclipse BIRT dengan versi terbaru, lalu silakan membuat project report dan sebuah blank report. Mari ikuti langkah dan penjelasannya sebagai berikut :
- Buka Report Design Perspective.
- Demi keamanan API yang akan diakses tentu membutuhkan token autentikasi. Pada Bagian Report Parameters klik kanan pilih New Parameter dan set dengan tipe data String beri nama token. Value parameter ini nanti akan diberikan melalui query string.

![Parameter Security Token](https://lh3.googleusercontent.com/pw/ACtC-3c2AL5SM27_Q_hFU2eM3bdFUaVimgROrn_pVr8a9quXGlzaLYdqmNnbjNOChLCvrNZLXplo_8VED4-7VKE4jUyH_3BzWvjeygyNwwzocVCYNdLog28WoOnD_9oGGcPg1TY85T1ix8eGjarplW8Ngwadag=w700-h369-no?authuser=0 "Parameter Security Token")

- Pada bagian Data Explorer pilih Data Sources klik kanan pilih New Data Source.
- Pilih Scripted Data Source lalu silakan diberi nama serta tekan tombol Finish.

![Scripted Data Source](https://lh3.googleusercontent.com/pw/ACtC-3cAGbPwubECqlJBScdWpHxdh1Y1_YgldJQPDo-tzVAreON2S0Oq_PqjyfAaB99aYczPYXqltvPbfy2Qc1a-CsI-uBP5KNFms9gUcLw8ifPB5Gs4jnaexLdAhimvpmtxtyOCRS1edggz7MBjO-S8uSEGwg=w700-h382-no?authuser=0 "Scripted Data Source")

- Sebelum masuk ke tahap selanjutnya, apa yang sebetulnya dimaksud dengan Data Sources? Dan apa bedanya dengan Data Sets? Data Sources adalah sumber data dari sebuah report, dapat berupa JDBC Data Source, MongoDB, POJO, Web Services, dan lain sebagainya. Sedangkan Data Sets adalah tabel-tabel atau entitas object yang ada di setiap Data Sources. Kita pun dapat melakukan join antar Data Sets yang berbeda Data Sources.
- Pada bagian Data Explorer pilih Data Sets klik kanan pilih New Data Set. Pastikan value dari field Data Set Type adalah Scripted Data Set.
- Silakan diberi nama serta tekan tombol Finish.

![Scripted Data Set](https://lh3.googleusercontent.com/pw/ACtC-3f6WwET57N39kiGjybwkacLMUqT1THxG2LnFaD78Dj5Hto0zeQyt-C8eMI7N28ERiXKjscbMroKn9EUt1Exbfv0I91C1Wxggn013bvp4THHAZ58nlzNHZ93FDeyWlZ973hXqyxKm7-s56uTc58XzHr4fQ=w700-h422-no?authuser=0 "Scripted Data Set")

- Selanjutnya kita membutuhkan variable dari entitas JSON yang akan diterima sebagai kolom. Buka dialog dari Data Set yang baru saja dibuat pada bagian Output Columns tambahkan seperti berikut.

![Field JSON as Column](https://lh3.googleusercontent.com/pw/ACtC-3eK5Axkhce7M9KrQl4FtdUyvknjgMs_v06dU2xtJfRf0zJ79YbKHFH2SeTGv20HlDlooiNGtKuistkSOxGMQntDOcqT4CMXfMtrMridE-r5sOhp8pSJctu_F9cEIjH11Lf35yrjViWp_fqkug6cxjGTPw=w700-h341-no?authuser=0 "Field JSON as Column")

- Sampai dengan tahap ini, kita sudah selesai untuk menyiapkan object yang akan digunakan untuk scripting. Selanjutnya silakan pilih Data Set yang baru saja dibuat. Pada bagian editor pilih tab Script maka akan ada beberapa opsi tahapan siklus Data Set yang dapat kita manipulasi dengan scripting seperti open, describe, fetch, close, dan lain sebagainya.
- Oiya bahasa scripting yang kita ketik adalah Rhino, sebuah engine Javascript yang dibangun menggunakan Java. Jadi bagi yang sudah terbiasa dengan Javascript seharusnya tidak terlalu asing. Untuk mengeksekusi URL REST API maka silakan pilih opsi open dan silakan tambahkan baris kode sebagai berikut.

{% gist 45de643c2a278ecbc9ceeb608193fffa %}

- Selanjutnya adalah untuk mapping data dari JSON yang diterima dari opsi open ke bagian Output Columns, silakan pilih opsi fetch dan tambahkan baris kode sebagai berikut.

{% gist 07f4c2a14386c6efd723c117e89299bc %}

- Semisal kita memiliki security token sebagai berikut, kita dapat menginputkan value dari access_token tersebut sebagai default value pada parameter token.

![Input Token as Authentication](https://lh3.googleusercontent.com/pw/ACtC-3cORD7FiM-ug80M_DuIPGdsaof1QEcdlDZAPun2HDOw3WeeHrrPlt-1GzkFaOsRCCsmQizXlR16mYIBXaOasUMaBko_3x-9Ui-OE8CIvTA1lBJwjIihqkefu_y_QZE3uwI7FwJl8APlAz-p_zFMsDwMSw=w700-h195-no?authuser=0 "Input Token as Authentication")

- Selanjutnya kita dapat mencoba melihat hasil dari script kita atau Preview Results, jika benar maka akan menghasilkan result sebagai berikut dan muncul print log pada terminal Eclipse (jika kita running Eclipse melalui command prompt di Windows atau terminal di UNIX).

![Preview Result in BIRT](https://lh3.googleusercontent.com/pw/ACtC-3cUuwW0qZspI00SlQSuN7R76c1snFBcmdsn979UbjcL8Lhs0GIpb03qZnGcs2nxmNV2Poizw9_tRjakfZDfUAt7EjBi0m4bq7aLGmp4K7I6SJcmPnGEJA7dRD2rqVIUTeL1FRSFp6UrWlphXBLYNh6WAw=w700-h340-no?authuser=0 "Preview Result in BIRT")

![Check Logger Execution](https://lh3.googleusercontent.com/pw/ACtC-3fwmiu68ZajNoMvWbFuLJco87x8LM1MfIa9Irfi8sW6Ts2iPHbTPjx2Tb1Nbq-fT2eXxSqbIz89ZW0FTtS7F95muggIQrMBC_1M7PKHpp_W3sDSnu8L8cvyu-_tmPcnXLLu9gcvZRf35k4Akx7YKbfAMQ=w700-h394-no?authuser=0 "Check Logger Execution")

- Jika sudah tampil, maka kita dapat melanjutkan ke bagian Report Designer untuk menghasilkan laporan dalam bentuk web yang nanti dapat dieksport ke dalam beberapa format sesuai fitur BIRT. Untuk design report kita dapat membuatnya dalam bentuk seperti di bawah ini ***cukup dengan drag and drop Data Sets yang telah dibuat jika ingin generate table, ringkas bukan?***

![Report Design](https://lh3.googleusercontent.com/pw/ACtC-3eF26mhnSISswCjKPhQpErSUzpwxdh6-BcA4D0kkcJcX9vC6sV7yaaFswZN58wcGz_KSHvwGUgqjk8KVVXix-1ROalyGL1QQU3-kXcbFYfnmoQXL0fYKPgzdA0vyqWMiJ9HgMNEdPXGQhKdLG7DuNfzwA=w700-h348-no?authuser=0 "Report Design")

---

## Hasil Akhir

Setelah selesai dengan design tampilan, tahap terakhir adalah untuk preview hasilnya pada browser dengan menginputkan token security. Silakan tekan tombol “View Report in Web Viewer”.

![Input Token Parameter on BIRT](https://lh3.googleusercontent.com/pw/ACtC-3fBnWInpXRRg2A5KlP456YhL_fTvPxG92Gw_2HRaScXfor6m0kmU5tOHae_xi0nqbjmBLmb7uziWv-S5lI8nyucnpjTZx5XzcCkihtBfCm069thyKVcKpNKe7UXPqna_4xUe5ayFqkzOGexrIuZojUQ5w=w700-h452-no?authuser=0 "Input Token Parameter on BIRT")

By the way security token tersebut nantinya dapat dipassing input valuenya melalui query parameter URL seperti berikut :
```
http://birt-viewer/nama_report.rptdesign?token=[security_token_anda]
```

![Final Result](https://lh3.googleusercontent.com/pw/ACtC-3e8nwYAJLBig_mNHP4EXJcQnNhzTjNN0qlhFi_I_hmNubXdaTxiJweaun0aobZZpSWtBJxUUg84z5rlhc9LeUFvTqBQgqkypAxrR2kzwQ0N83jgxJ6hAC_nxPKS8tuDV3m0H705_yVDz87F7fWE1cExww=w700-h248-no?authuser=0 "Final Result")

Untuk template report beserta source code secara keseluruhan ada pada Gist di bawah ini.

{% gist b5c72988c2c5e4eb87f6cbf46dd38005 %}

Jika seandainya menginginkan improvisasi seperti format money atau fungsi-fungsi lainnya, kita dapat memanfaatkan built-in function dari BIRT tanpa perlu merubah koding dari backend atau sisi API. Cukup lengkap, praktis, dan siap digunakan.

![Additonal Functionalities](https://lh3.googleusercontent.com/pw/ACtC-3c7ZK5f0Ucr9-Y3fyBMHhISoWYfGODYKDhgbTuJuNmQfDX3Q13xCLDlrdbWhgo65HjiNk9gZvLXQd-9iVucl3ORcDcA-NhdTffdgPzaohKgGg1Wrb34RFmsy7I5DYWNjvPoqhmNfb29jw6tD0wg0CWqCA=w700-h221-no?authuser=0 "Additional Functionalities")

Oke sekian dari saya jika ada komentar atau diskusi yuk mari kita berbagi ilmu. Jika ada yang kurang jelas maaf ya karena maklum ini adalah artikel pertama saya hehe. Terimakasih 😀