---
title: Vanilla Javascript AJAX Request
author:
  name: Adinandra Dharmasurya
  link: https://adinandra.dharmasurya.id
date: 2014-01-04 +0700
categories: [Pemrograman]
tags: [ajax,javascript]
pin: false
image:
  src: /etc/scripting.png
  width: 350   # in pixels
  height: 350   # in pixels
  alt: Scripting
---

Biasanya agar lebih praktis dan mudah dalam melakukan AJAX request ke controller saya selalu memanfaatkan jQuery. Baik dengan $.post, $.get, atau untuk concrete biasanya dengan $.ajax. Sebetulnya tanpa jQuery pun saya dapat memanfaatkan class dan fungsi sakti dari engine browser itu sendiri untuk melakukannya, yaitu dengan XMLHttpRequest hehe. Intinya sama, yaitu kita request secara background ke server dengan method POST or GET. Lalu setelah request disampaikan akan mendapat response. 

Berikut adalah contoh bagaimana cara kita request ke suatu file lalu mendapatkan responsenya. Kita dapat anggap file tersebut adalah sebagai response dari controller. File yang digunakan adalah text file yang berisikan text string "ini dari file...". Filenya saya simpan dengan nama test.txt sejajar dengan file .html yang akan discripting. Selanjutnya adalah baris kode yang memanggil file tersebut. Namun karena menggunakan engine bawaan maka akan terlihat sedikit lebih panjang kodingnya :


```
<html>
    <head>
		    <title>AJAX Pure Basic</title>
    </head>
    <body>
    <script type="text/javascript">
        function fnAjaxCall() { 
            var req = false;
            if(window.XMLHttpRequest) 
                req = new XMLHttpRequest(); 
            else 
                req = new ActiveXObject("Microsoft.XMLHTTP"); 
            req.open("POST", "test.txt", true); 
            req.onreadystatechange = function(){ 
            if(req.readyState == 4) 
                document.getElementById("load").innerHTML = hasil.responseText; 
            } 
            req.send(null); 
    		}
    </script>
		<input type="button" value"Call AJAX" onclick="javascript:fnAjaxCall();">
		    <span id="load"></span>
    </body>
</html>
```

Untuk mendapatkan data saya menggunakan http method ketika open request ke server. Pada contoh saya menggunakan POST namun dapat disesuaikan dengan keadaan lapangan. Untuk responsenya karena data yang direquest berisikan text atau string bukan dalam bentuk JSON maka dapat menggunakan function responseText secara langsung sebagai return value. Di jQuery saya memanfaatkan event "success" dan "error" sedangkan cara *oldschool* saya menggunakan lambda function terhadap onreadystatechange sebagai callback. Jika response statenya bukan bernilai 4 biasanya adalah gagal dan 4 adalah berhasil.

Untuk kustom terkait set header yang biasanya adalah standar untuk memanggil secara RESTful juga dapat dilakukan dengan fungsi berikut

```
XMLHttpRequest.setRequestHeader(header, value)
```

Memang sebetulnya penggunaan vanilla seperti ini sudah jarang terlihat karena sekarang sudah banyak framework javascript yang bertebaran. Namun tidak ada salahnya menggunakan vanilla apabila tidak ingin dibebani oleh library yang membuat website dan browser menjadi lebih berat padahal hanya untuk task yang simple. Referensi lengkap dapat dilihat pada dokumentasi [berikut](https://docs.w3cub.com/dom/xmlhttprequest). Terimakasih semoga bermanfaat.

*Disadur dari blog lama saya di WordPress.com*