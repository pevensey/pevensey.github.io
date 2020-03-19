---
layout: post
title: Tutorial CRUD Lumen 5.8
date: 2020-03-19
excerpt: "Pembuatan dan Pengujian RESTful API menggunakan Lumen"
tags: [tutorial, php, lumen]
comments: false
---

Pada tulisan ini saya ingin berbagi pengetahuan tentang Micro-Framework PHP yang bernama Lumen. 
Sedikit cerita saya menggunakan Lumen untuk mengerjakan proyek tugas akhir saya, 
pada tugas akhir ini saya menggunakan platform Android sebagai antarmuka aplikasi sedangkan pemrosesan data menggunakan Lumen. 
Kenapa Lumen? Hmm sebenarnya pilihan pertama saya itu Laravel namun setelah baca-baca dari berbagai sumber dan bertanya ke teman akhirnya saya memilih Lumen, karena yang saya butuhkan pada bagian pemrosesan data cukup RESTful API saja sedangkan antarmuka (view) menggunakan Android.	 

### Apa itu Lumen?
Lumen merupakan Micro-Framework PHP yang berbasis dari Framework Laravel. Lumen diciptakan untuk membuat RESTful API dan microservices dengan kecepatan dan performa yang tinggi. Singkatnya Lumen merupakan framework turunan dari Laravel yang lebih fleksibel dan ringan. 
Performa Laravel vs Lumen disajikan pada tabel berikut :

{% capture images %}
	https://miro.medium.com/max/874/1*FUTIhBGItx1GDChMKXUk0A.png
{% endcapture %}
{% include gallery images=images caption="Perbandingan request per second" cols=1 %}

{% capture images %}
	https://miro.medium.com/max/918/1*x7Zrg9374XZZ_KUrReepmQ.png
{% endcapture %}
{% include gallery images=images caption="Perbandingan response time (ms)" cols=1 %}

Sumber gambar : https://medium.com/@laurencei/lumen-vs-laravel-performance-in-2018-1a9346428c01

### Instalasi dan Konfigurasi
Pada tutorial ini siapkan alat dan bahan sebagai berikut :
•	PHP >= 7.1.3
•	Composer
•	Text editor favorit (saya menggunakan VSCODE)

Kalo alat dan bahan belum siap baiknya download alat dan bahan tersebut terlebih dahulu :

PHP : [Download PHP](https://www.apachefriends.org/index.html)
Composer : [Download Composer](https://getcomposer.org/)
VSCODE [Download VSCODE](https://code.visualstudio.com/)

Oke jika alat dan bahan sudah siap kita lanjutkan ke instalasi dan konfigurasi.
Ohya sebagai catatan saya menggunakan sistem operasi Windows 10, jadi perintah-perintah dan susunan direktori pada tutorial ini mungkin akan berbeda di sistem operasi berbasis Linux, OS X dan lainnya.

Pertama-tama kita install Lumen terlebih dahulu, buka CMD kemudian pindah ke direktori 
{% highlight python %}
C:\xampp\htdocs
{% endhighlight %}







Sekian tulisan saya, maaf apabila masih terdapat banyak kekurangan, dan semoga tulisan ini bermanfaat bagi para pembaca. Terimakasih.