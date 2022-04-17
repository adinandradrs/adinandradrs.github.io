---
title: IBM Maximo – Try Catch Automation Script
author:
  name: Adinandra Dharmasurya
  link: https://adinandra.dharmasurya.id
date: 2018-02-04 +0700
categories: [Pemrograman]
tags: [maximo,automationscript,custom]
pin: false
image:
  src: /etc/scripting.png
  width: 350   # in pixels
  height: 350   # in pixels
  alt: Scripting
---

Ketika mengerjakan project di kantor lama saya mendapat task untuk membuat Automation Script. Script tersebut digunakan untuk upload data person dari sebuah custom table ke table person di IBM Maximo. Nah kebetulan ada error yang kerap terjadi yaitu error duplicate data. 

Lalu bagaimana caranya agar saya tahu bahwa error yang terjadi adalah error duplikasi? Berikut adalah block try..catch di Automation Script dan bagaimana mendapatkan jenis exception yang terjadi

```
try :
	nperson = personSet.add();
	... #logic statement yang berpotensi terjadi error
except MXException, e :
	errkey = str(e.getErrorKey()); #cast key error sebagai string
	if errkey == 'duplicatekey' : #jika ternyata key messagenya adalah duplicate data
		... #logic untuk ketika terjadi exception
```
Yang dapat dimanfaatkan adalah class MXException karena merupakan base class Exception di Platform Maximo. Untuk mendapatkan keynya dapat menggunakan function getErrorKey(). Semoga bermanfaat, terimakasih.