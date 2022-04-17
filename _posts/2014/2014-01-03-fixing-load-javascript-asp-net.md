---
title: Fixing Load Javascript ASP.NET
author:
  name: Adinandra Dharmasurya
  link: https://adinandra.dharmasurya.id
date: 2014-01-03 +0700
categories: [Pemrograman]
tags: [asp.net,javascript]
pin: false
image:
  src: /etc/scripting.png
  width: 350   # in pixels
  height: 350   # in pixels
  alt: Scripting
---

Ketika membangun website berbasis ASP.NET 3.5, load file javascript pada tag script terkadang mengalami kendala file tersebut tidak ikut terdeploy di IIS. File javascript tersebut dianggap tidak ada di server aplikasi atau terkena **runat server error** (istilah mudahnya adalah "not yet run at server"). Efeknya ada banyak variabel dan fungsi yang berasal dari javascript tidak teridentifikasi dan menghasilkan exception seperti misalnya undefined. Error yang diakibatkan oleh "not yet run at server" dapat dicek melalui console debugger milik (IE 8/9) atau melalui debugger Visual Studio. Jadi bagaimana cara mengatasi masalah tersebut?

```
<asp:ScriptManager ID="script_mgr_1" runat="server"> 
    <Scripts> 
        <asp:ScriptReference Path="lokasi file javascript 1" />
        <asp:ScriptReference Path="lokasi file javascript 2" />
        <asp:ScriptReference Path="lokasi file javascript 3" />
        <asp:ScriptReference Path="lokasi file javascript n" />
    </Scripts>
</asp:ScriptManager>
```

Snippet kode di atas adalah solusi untuk menangani error "not yet run at server" dengan memanfaatkan tag ScriptManager dari ASP.NET di halaman ASPX yang membutuhkan. Melalui tag ScriptReference maka file javascript akan turut terdeploy di IIS atau server lokal. Biasanya saya menggunakannya untuk load file jQuery. Karena beberapa waktu yang lalu saya mengalami file jQuery dianggap tidak ada di server aplikasi. Untuk mengatasinya saya harus set relative pathnya berdasarkan solution project di ScriptReference. Simple, just like a piece of cake.

*Disadur dari blog lama saya di WordPress.com*