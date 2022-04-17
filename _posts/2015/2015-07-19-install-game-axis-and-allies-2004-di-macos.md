---
title: Install Game Axis and Allies (2004) di Mac OS
author:
  name: Adinandra Dharmasurya
  link: https://adinandra.dharmasurya.id
date: 2015-07-19 +0700
categories: [Teknologi]
tags: [wineskin,mac,game,retro]
pin: false
---

[Axis & Allies](https://en.wikipedia.org/wiki/Axis_%26_Allies_(2004_video_game)) adalah game strategi perang dunia 2 besutan Atari, yang pertama kali saya mainkan di sekitar tahun 2004. Mainnya numpang di rumah Martdoman Sitorus karena dia satu-satunya teman SMP yang punya PC high-end di jaman itu. Berhubung game tersebut adalah game untuk PC maka perlu diporting terlebih dahulu. 

Setelah menemukan game Axis & Allies di torrent, kemarin malam saya langsung download menggunakan Bolt. Tidak sampai setengah jam 1.5 GB sudah selesai *ya iyalah dapat 800 kb/s* Step yang saya jalankan agar dapat memainkannya di MacBook :

1. Nyalakan VM Windows XP.
2. Install Daemon Tools, mount, install gamenya.
3. Setelah selesai install game, jangan lupa overwrite dengan cracknya agar tidak perlu mount CD lagi.
4. Copy installed game ke local Mac.
5. Buka Wineskin dan buatkan satu wrapper, misal saya beri nama "Axis & Allies".
6. Klik show content untuk wrappernya, move folder yang tadi udah di pindah ke local Mac ke ```drive_c/Program Files/```.
7. Double click Wineskin di dalam wrapper. Di bagian Advance, set Windows EXEnya in example seperti ini ```/Program Files/Atari/Axis & Allies/AA.exe```.
8. Pada bagian Screen saya set full screen dan adjust resolusi sesuai real screen resolution yang ada di MacBook.
9. Jangan tick "Use Mac Driver instead of X11" soalnya layarnya nati jadi kedip-kedip.
10. Test dulu sebelum direlease.

Langkah-langkahnya di atas sebetulnya sama dengan ketika porting game DotA, AoE, DivineKids, dan lain sebagainya. Saatnya bernostalgila. Sampai dengan pukul 2.15 pagi ini saya baru selesai bermain game Axis & Allies. Durasi sekali skirmish cukup lama hingga 2.5 jam.

*Disadur dari blog lama saya di WordPress.com*