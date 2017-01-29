---
layout: post
title: "Login & Security"
date: 2017-01-26
categories:
  - Sharing
description: Software Development about Login and Security
image: https://images.unsplash.com/photo-1468070454955-c5b6932bd08d?dpr=1&auto=format&fit=crop&w=1500&h=860&q=80
image-sm: https://images.unsplash.com/photo-1468070454955-c5b6932bd08d?dpr=1&auto=format&fit=crop&w=500&h=300&q=80
---

Sebagai seorang pelajar perisian komputer, sudah pasti perkara paling wahid & utama pada perisian saudara/i
adalah Login atau nama Malaysia-nya adalah Log Masuk. Dimana, log masuk menggunakan data yang telah disimpan
di dalam database berdasarkan ketetapan pengguna.

Rule of thumb, proses log masuk hanya memerlukan dua perkara; Username (boleh jadi email atau id yang unik) dan juga
password. Jadi bagaimana menghasilkan sebuah proses log masuk yang bermutu? yang berkualiti tinggi ?

Pada pandangan saya, perkara ini boleh dikategorikan kepada dua; 1) Ciri Keselamatan 2) Cara Simpanan Data.

Data yang digunakan semasa log masuk merupakan data yang sensitif dan seharusnya dilindungi.
Mengikut laman web [OWASP](http://www.owasp.org/index.php/Authentication_Cheat_Sheet "OWASP"), perkara berikut merupakan beberapa element yang patut dilaksanakan.

 1. Case insensitive - User bernama 'fauzan' dan 'Fauzan' merupakan pengguna yang sama.
 2. Email as user ID - Penggunaan email sebagai username semakin banyak diguna-pakai.
 3. Password length - Panjang password perlulah diletakkan minimum (8 atau 10 character) dan maximum (128 character).
 4. Password complexity - Campuran penggunaan huruf kecil dan besar (a-Z) , nombor (0-9) serta simbol atau [special character](http://www.owasp.org/index.php/Password_special_characters).

Laman [Vertabelo - How to store authentication data](http://www.vertabelo.com/blog/technical-articles/how-to-store-authentication-data-in-a-database-part-1),
menerangkan cara penyimpanan data dan salah satu perkara yang disentuh adalah Hash the password.

PHP telah menyediakan fungsi [*password_hash*](http://php.net/manual/en/function.password-hash.php) yang menukarkan ayat - string - kepada sebuah aksara berpanjang maksimum 72 aksara.
Penggunaan fungsi ini sangat bagus, [Hashing vs Encrypting](http://www.darkreading.com/safely-storing-user-passwords-hashing-vs-encrypting/a/d-id/1269374)
menerangkan penggunaan hashing adalah lebih bagus berbanding encrypting.

Dengan menggunakan perkara-perkara diatas, perkara 1) dapat diselesaikan.

Code suggestion :-


[code language="php"]
```php
<?php

      //dua variable yang akan di-hash
      $string_password = 'passwordsaya123';
      
      //Hash password menggunakan format BCRYPT & fungsi password_hash()
      $hashed_password = password_hash($string_password, PASSWORD_BCRYPT);

      //php akan mengubah $string_password kepada $2y$10$hrT7ZPYuK3IaHwKTDNJ9D.xUf6iTuha0rRUGIsYwEyNfze/l82DXK
      //dengan length 60 character.
      //$hashed_password yang sudah di-Hash akan disimpan ke dalam database.

      //Cara untuk melihat sekiranya password yang dihash sama atau tidak
      //$value_from_DB merupakan password dari Database dan $value_from_user input dari user

      password_verify($value_from_user, $value_from_DB)

      //password_verify akan return TRUE atau FALSE value
      //dan sangat sesuai untuk membuat If/Else

      if (password_verify($value_from_user, $value_from_DB)) {
      //return TRUE jika password sama
      echo 'password sama';

      } else {
      //return FALSE jika password tidak sama
      echo 'password tidak sama';

      }

```
[/code]