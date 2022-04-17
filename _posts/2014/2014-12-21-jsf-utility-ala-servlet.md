---
title: JSF Utility ala Servlet
author:
  name: Adinandra Dharmasurya
  link: https://adinandra.dharmasurya.id
date: 2014-12-21 +0700
categories: [Pemrograman]
tags: [java,jsf,jsp]
pin: false
image:
  src: /etc/scripting.png
  width: 350   # in pixels
  height: 350   # in pixels
  alt: Scripting
---

Berhubung sedang mempelajari dan menerapkan rapid development dengan JSF (Java Server Faces), saya menyiapkan beberapa fungsi sebagai helper seperti halnya yang biasa digunakan ketika develop dengan JSP (Java Server Page). Beberapa kebutuhan method dan function yang membuat saya sulit move-on dari JSP Servlet adalah sebagai berikut
1. Forward page
2. Set session
3. Get value from a session
4. Mengambil parameter request dari url
 
Di bawah ini adalah penjelasan dari helper yang telah saya develop.

Fungsi untuk mengabil parameter request atau query parameter dari url. Semisal terdapat url ```https://baseurl:port/endpoint?parameter=value``` maka dapat memanggil fungsinya seperti berikut ```getRequestParameter("parameter")```
```
public static String getRequestParameter(String name) {
    return (String) FacesContext
        .getCurrentInstance()
        .getExternalContext()
        .getRequestParameterMap()
        .get(name);
}
```

Method untuk menuliskan session pada web server aplikasi, semisal ingin menyimpan session dari obyek profile user yang telah login ke aplikasi. Untuk cara penggunaannya dapat mengeksekusinya sebagai berikut ```setSession("USERLOGIN", userLoginResponse)```
```
public static void setSession(String id, Object o){
        FacesContext context = FacesContext.getCurrentInstance();
        context.getExternalContext().getSessionMap().put(id, o);
}
```

Fungsi untuk mendapatkan value dari sebuah session yang tersimpan pada web server aplikasi. Semisal ingin mendapatkan session dari login dengan key **USERLOGIN** maka dapat memanggil fungsinya seperti berikut ```getSession("USERLOGIN")``` 
```
public static Object getSession(String id){
        FacesContext context = FacesContext.getCurrentInstance();
        return context.getExternalContext().getSessionMap().get(id);
}
```

Method untuk melakukan redirect ke sebuah URL pada aplikasi. Untuk melakukannya dapat memanggil methodnya seperti berikut ```redirect("/login.jsf")```
```
public static void redirect(String url){
        try {
                FacesContext.getCurrentInstance().getExternalContext().redirect(url);
        } catch (IOException e) {
                e.printStackTrace();
        }
}
```

Fungsi dan method tersebut sebaiknya dibungkus ke dalam sebuah class utility serta dapat dipanggil secara static. Sehingga tidak perlu instansiasi terhadap class tersebut menjadi object. Fungsi di atas semuanya mengandalkan API servlet karena sebetulnya JSF juga dibangun di atas servlet. Semoga helper di atas dapat bermanfaat bagi yang membutuhkan. Terimakasih.

*Disadur dari blog lama saya di WordPress.com*