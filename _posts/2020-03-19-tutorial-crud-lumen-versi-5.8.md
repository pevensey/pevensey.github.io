---
layout: post
title: Tutorial CRUD Lumen 5.8
date: 2020-03-19
excerpt: "Pembuatan dan Pengujian RESTful API menggunakan Lumen"
tags: [tutorial, php, lumen]
feature: https://i1.wp.com/wp.laravel-news.com/wp-content/uploads/2015/04/lumen.png?resize=2200%2C1125
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
1. PHP >= 7.1.3
2. Composer
3. Text editor favorit (saya menggunakan VSCODE)

Kalo alat dan bahan belum siap baiknya download alat dan bahan tersebut terlebih dahulu :

PHP : [Download PHP](https://www.apachefriends.org/index.html)<br>
Composer : [Download Composer](https://getcomposer.org/)<br>
VSCODE [Download VSCODE](https://code.visualstudio.com/)<br>

Oke jika alat dan bahan sudah siap kita lanjutkan ke instalasi dan konfigurasi.
Ohya sebagai catatan saya menggunakan sistem operasi Windows 10, jadi perintah-perintah dan susunan direktori pada tutorial ini mungkin akan berbeda di sistem operasi berbasis Linux, OS X dan lainnya.

Pertama-tama kita install Lumen terlebih dahulu, buka CMD kemudian pindah ke direktori.

{% highlight shell %}
C:\xampp\htdocs>
{% endhighlight %}

Kemudian jalankan perintah berikut untuk menginstall Lumen versi 5.8.

{% highlight php %}
composer create-project laravel/lumen LatihanLumen "5.8.*" --prefer-dist 
{% endhighlight %}


Tunggu hingga instalasi selesai. Jika instalasi sudah selesai kita lakukan beberapa konfigurasi terlebih dahulu.
Buka  folder instalasi LatihanLumen (C:\xampp\htdocs\LatihanLumen) menggunakan VSCODE atau text editor favorit kalian. Struktur folder Lumen akan terlihat seperti ini :

<figure>
    <a href="{{ site.url }}/assets/img/ss-vscode-1.png"><img src="{{ site.url }}/assets/img/ss-vscode-1.png"></a>
</figure>

Buka bootstrap > app.php. Kita akan mengedit beberapa konfigurasi. Hapus komentar pada baris 24 dan 26.

{% highlight php %}
<?php
...

 $app->withFacades(); //baris 24
 
 $app->withEloquent(); //baris 26
{% endhighlight %}

Masih di bootstrap > app.php kemudian scroll down sampai baris 79 kemudian hapus komentar pada baris 79 sampai 81.

{% highlight php %}
<?php
...
  
$app->register(App\Providers\AppServiceProvider::class); //baris 79
$app->register(App\Providers\AuthServiceProvider::class); //baris 80
$app->register(App\Providers\EventServiceProvider::class); //baris 81
{% endhighlight %}

Lanjut ke konfigurasi database, buka file .env kemudian sesuaikan nama database, username dan password. Kalo database belum ada buat terlebih dahulu. Kurang lebih seperti ini jadinya konfigurasi di file .env :

{% highlight sql %}
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=dbmahasiswa
DB_USERNAME=root
DB_PASSWORD=
DB_TIMEZONE=+07:00
{% endhighlight %}

Nama database yang saya gunakan “dbmahasiswa”, kenapa? karena pada tutorial ini akan membuat daftar mahasiswa sederhana hehe. Kemudian username “root” dan password kosong sesuai konfigurasi database pada komputer yang saya gunakan.
Setelah itu kita buat migration dengan perintah berikut (jalankan di C:\xampp\htdocs\LatihanLumen).

{% highlight php %}
php artisan make:migration tabel_mahasiswa --create=mahasiswa
{% endhighlight %}

Tunggu migration selesai kemudian buka database > migration > tabel_mahasiswa.php
Kemudian tambahkan field nama dan nim seperti baris kode berikut :

{% highlight php %}
<?php
...
  
public function up()
{
    Schema::create('mahasiswa', function (Blueprint $table) {
        $table->bigIncrements('id');
        $table->string('nama', 100);
        $table->integer('nim');
        $table->timestamps();
    });
}
{% endhighlight %}

Kemudian jalankan perintah ini di (C:\xampp\htdocs\LatihanLumen) :

{% highlight php %}
php artisan migrate
{% endhighlight %}

Perintah tersebut digunakan untuk membuat tabel mahasiswa pada “dbmahasiswa” dengan field id (big int), nama(varchar), nim(integer) created_at (timestamp) dan updated_at (timestamp)

<figure>
    <a href="{{ site.url }}/assets/img/ss-vscode-1.png"><img src="{{ site.url }}/assets/img/ss-vscode-3.png"></a>
</figure>

### Membuat Model
Buka folder app kemudian buat file baru disana dengan nama “ModelMahasiswa.php”.

<figure>
    <a href="{{ site.url }}/assets/img/ss-vscode-1.png"><img src="{{ site.url }}/assets/img/ss-vscode-4.png"></a>
</figure>

Kemudian salin kode berikut:

{% highlight php %}
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class ModelMahasiswa extends Model
{
   protected $table = 'mahasiswa'; 
}
{% endhighlight %}


Catatan : $table diisi nama tabel yang kita buat pada database.
Lanjut ke pembuatan controller.

### Membuat Controller
Lumen merupakan microframework turunan Laravel dengan beberapa komponen yang sudah dilepas sehingga kita tidak bisa menggunakan php artisan untuk membuat controller. Kenapa dilepas? karena dengan dilepasnya beberapa komponen/library dari Laravel bisa membuat Lumen semakin ringan. Jadi mau tidak mau kita harus membuat file controller secara manual.
Oke, sekarang kita buat file baru di app > Http > Controllers dengan nama “MahasiswaController.php”.

{% highlight php %}
<?php

namespace App\Http\Controllers;

use App\ModelMahasiswa;
use Illuminate\Http\Request;

class MahasiswaController extends Controller
{
    /**
     * Create a new controller instance.
     *
     * @return void
     */
    public function __construct()
    {
        //
    }

    public function getall(){
        $data = ModelMahasiswa::all();
        return response($data);
    }
    public function getbyid($id){
        $data = ModelMahasiswa::where('id',$id)->get();
        return response ($data);
    }
    public function save(Request $request){
        $data = new ModelMahasiswa();
        $data->nama = $request->input('nama');
        $data->nim = $request->input('nim');
        $data->save();
    
        return response('Berhasil Menambah Data');
    }
    public function update(Request $request, $id){
        $data = ModelMahasiswa::where('id',$id)->first();
        $data->nama = $request->input('nama');
        $data->nim = $request->input('nim');
        $data->save();
    
        return response('Berhasil Merubah Data');
    }
    
    public function delete($id){
        $data = ModelMahasiswa::where('id',$id)->first();
        $data->delete();
    
        return response('Berhasil Menghapus Data');
    }
}
{% endhighlight %}

Setelah controller selesai dibuat lanjut ke pembuatan routes.

### Membuat Routes
Jika sudah membuat model lanjutkan dengan membuat routes dan endpoint. Secara keseluruhan isi file web.php akan seperti ini :

{% highlight php %}
<?php
...
    
$router->get('/', function () use ($router) {
    return $router->app->version();
});

$router->get('/mahasiswa', ['uses' => 'MahasiswaController@getall']);
$router->get('/mahasiswa/{id}', ['uses' => 'MahasiswaController@getbyid']);
$router->post('/mahasiswa', ['uses' => 'MahasiswaController@save']);
$router->put('/mahasiswa/{id}', ['uses' => 'MahasiswaController@update']);
$router->delete('/mahasiswa/{id}', ['uses' => 'MahasiswaController@delete']);
{% endhighlight %}

Kemudian jalankan perintah berikut untuk menyalakan Lumen (jalankan di C:\xampp\htdocs\LatihanLumen)

{% highlight php %}
php -S localhost:8000 -t ./public
{% endhighlight %}

Untuk mengakses Lumen buka browser kemudian arahkan ke localhost:8000, Jika berhasil akan muncul seperti ini:

<figure>
    <a href="{{ site.url }}/assets/img/ss-vscode-1.png"><img src="{{ site.url }}/assets/img/ss-vscode-5.png"></a>
</figure>

### Pengujian
Pada pengujian ini saya menggunakan Postman desktop versi 7.20.1. Gunakan JSON untuk mengirim data.
1. Pengujian POST

<figure>
    <a href="{{ site.url }}/assets/img/ss-vscode-1.png"><img src="{{ site.url }}/assets/img/pengujian-post-request.png"></a>
</figure>

Jika berhasil maka akan return response :

<figure>
    <a href="{{ site.url }}/assets/img/ss-vscode-1.png"><img src="{{ site.url }}/assets/img/pengujian-post-response.png"></a>
</figure>

2. Pengujian GET ALL

Gunakan method GET dengan endpoint http://localhost:8000/mahasiswa.
<figure>
    <a href="{{ site.url }}/assets/img/ss-vscode-1.png"><img src="{{ site.url }}/assets/img/pengujian-getall-response.png"></a>
</figure>

Jika berhasil akan return response :
<figure>
    <a href="{{ site.url }}/assets/img/ss-vscode-1.png"><img src="{{ site.url }}/assets/img/pengujian-getall-response.png"></a>
</figure>

3. Pengujian GET by Id
Gunakan method GET dengan endpoint http://localhost:8000/mahasiswa/{id} .
<figure>
    <a href="{{ site.url }}/assets/img/ss-vscode-1.png"><img src="{{ site.url }}/assets/img/pengujian-getbyid-request.png"></a>
</figure>

Jika berhasil akan return response :
## error
<figure>
    <a href="{{ site.url }}/assets/img/ss-vscode-1.png"><img src="{{ site.url }}/assets/img/pengujian-getbyid-response.png"></a>
</figure>

4. Pengujian UPDATE
Gunakan method PUT dengan endpoint http://localhost:8000/mahasiswa/{id}. Kita akan mengubah nama dari “Yulianto Pambudi” menjadi “Budi”.
<figure>
    <a href="{{ site.url }}/assets/img/ss-vscode-1.png"><img src="{{ site.url }}/assets/img/pengujian-put-request.png"></a>
</figure>

Jika berhasil akan return response :
<figure>
    <a href="{{ site.url }}/assets/img/ss-vscode-1.png"><img src="{{ site.url }}/assets/img/pengujian-put-response.png"></a>
</figure>
5. Pengujian DELETE
Gunakan method DELELTE dengan endpoint http://localhost:8000/mahasiswa/{id}.
<figure>
    <a href="{{ site.url }}/assets/img/ss-vscode-1.png"><img src="{{ site.url }}/assets/img/pengujian-delete-request.png"></a>
</figure>

Jika berhasil akan return response :
<figure>
    <a href="{{ site.url }}/assets/img/ss-vscode-1.png"><img src="{{ site.url }}/assets/img/pengujian-delete-response.png"></a>
</figure>

Jika kalian tidak mau repot-repot mengikuti tutorial ini secara keseluruhan, bisa lihat repository git yang telah saya buat. Namun, alangkah baiknya jika kalian mengikuti tutorial ini secara step by step agar semakin paham cara kerja Lumen.
Sekian tulisan saya, maaf apabila masih terdapat banyak kekurangan, dan semoga tulisan ini bermanfaat bagi para pembaca. Terimakasih.

Repository git nya bisa liat disini.
Kesimpulan dari tutorial ini Lumen merupakan micro-framework turunan dari Laravel yang di desain untuk performa yang cepat dan ringan. Kita juga sudah bisa membuat fungsi CRUD menggunakan Lumen versi 5.8. Kemudian melakukan pengujian terhadap fungsi CRUD yang sudah dibuat menggunakan Postman.
Oke sekian tutorial CRUD menggunakan Lumen 5.8, jika ada kurang dan lebih mohon maaf. Semoga ilmunya bermanfaat, selamat belajar dan terima kasih :D.

Referensi
Lumen vs Laravel performance in 2018;
Tutorial CRUD Lumen 5.4 : Microframework RESTful API untuk Laravel.