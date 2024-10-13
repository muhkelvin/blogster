---
external: false
title: Laravel - Pengantar Laravel
description: Laravel introduction
date: 2024-10-14
---

## Introduction

Laravel adalah salah satu framework PHP yang paling populer dan banyak digunakan di kalangan developer web. Framework ini mempermudah pengembangan aplikasi web dengan menyediakan alat dan struktur yang efisien. Pada artikel ini, kita akan membahas secara mendalam tentang Laravel, mulai dari pengenalan dasar hingga langkah-langkah memulai proyek Laravel.

## 1.1 Apa Itu Laravel?

Laravel adalah framework open-source berbasis PHP yang dirancang untuk pengembangan aplikasi web. Laravel mengikuti arsitektur MVC (Model-View-Controller) yang memungkinkan pemisahan yang jelas antara logika aplikasi, data, dan presentasi. Ini memberikan developer fleksibilitas untuk mengelola berbagai aspek pengembangan web dengan cara yang terstruktur dan mudah dipahami. Dengan Laravel, developer dapat membuat aplikasi web yang modern, dinamis, dan skalabel, tanpa harus berurusan dengan kompleksitas dan pengulangan kode yang berlebihan.

Laravel juga dilengkapi dengan berbagai fitur yang memudahkan pengelolaan otentikasi pengguna, routing, controller, sesi, validasi data, dan lain-lain. Salah satu fitur unggulan Laravel adalah Eloquent ORM, yang memungkinkan interaksi dengan basis data melalui objek model yang intuitif dan mudah digunakan. Dengan berbagai fitur tersebut, Laravel menjadi pilihan yang sangat menarik bagi developer yang ingin membangun aplikasi web yang handal dan efisien.

## 1.2 Mengapa Memilih Laravel?

Salah satu alasan utama mengapa banyak developer memilih Laravel adalah karena kemudahannya. Laravel dirancang untuk meningkatkan produktivitas developer dengan menyediakan berbagai alat dan fungsi yang siap pakai. Misalnya, fitur Blade templating engine yang memungkinkan pembuatan tampilan web secara dinamis dan fleksibel, serta Eloquent ORM yang menyederhanakan pengelolaan basis data.

Selain itu, Laravel memiliki dokumentasi yang sangat lengkap dan komunitas yang aktif. Ini berarti bahwa jika Anda mengalami kesulitan atau memiliki pertanyaan, kemungkinan besar solusi atau jawaban sudah tersedia secara online. Ekosistem Laravel juga dilengkapi dengan berbagai paket dan library, seperti Laravel Passport untuk otentikasi API, dan Laravel Horizon untuk memantau antrian pekerjaan. Ini memudahkan developer untuk menambahkan fitur-fitur canggih ke dalam aplikasi mereka tanpa harus membangun semuanya dari nol.

Keamanan juga merupakan salah satu alasan penting mengapa Laravel sering dipilih. Framework ini dilengkapi dengan berbagai mekanisme keamanan bawaan, seperti perlindungan terhadap serangan SQL Injection, Cross-Site Request Forgery (CSRF), dan Cross-Site Scripting (XSS). Dengan adanya fitur-fitur ini, developer tidak perlu khawatir tentang keamanan aplikasi mereka, karena Laravel sudah dirancang untuk melindungi aplikasi dari berbagai ancaman umum di web.

## 1.3 Sejarah Singkat Laravel

Laravel pertama kali dirilis oleh Taylor Otwell pada tahun 2011 sebagai alternatif dari framework PHP lain yang ada pada saat itu, seperti CodeIgniter. Pada saat itu, Otwell merasa bahwa framework-framework yang ada kurang menyediakan fitur-fitur modern yang dibutuhkan oleh developer untuk membangun aplikasi web yang kompleks dan canggih. Dari kebutuhan inilah lahir Laravel, yang dalam beberapa tahun saja berhasil menjadi salah satu framework PHP yang paling dominan di dunia.

Versi pertama Laravel dirilis pada Juni 2011, dan sejak saat itu, Laravel telah mengalami banyak pembaruan dan perbaikan. Dengan rilis Laravel 5 pada tahun 2015, framework ini semakin solid dan mulai dikenal sebagai pilihan utama bagi developer yang menginginkan framework modern, lengkap dengan fitur seperti middleware, sistem pengelolaan paket yang efisien melalui Composer, dan banyak lagi. Hingga saat ini, Laravel terus berkembang dengan berbagai fitur baru dan peningkatan performa yang menjadikannya tetap relevan di industri pengembangan web.

## 1.4 Struktur Dasar Laravel

Laravel memiliki struktur folder yang rapi dan mudah dipahami, sehingga memudahkan developer untuk mengorganisasi kode mereka. Ketika Anda pertama kali menginstal Laravel, Anda akan melihat beberapa direktori utama seperti ``app ``, ``config``, ``database``, ``public``, dan lain-lain.

- **``App``**: Di sinilah logika bisnis aplikasi Anda berada. Anda akan menemukan folder untuk controller, model, middleware, dan lainnya di dalam direktori ini.
- **``Config``**: Folder ini berisi semua file konfigurasi untuk aplikasi Anda. Anda dapat mengatur berbagai konfigurasi, seperti pengaturan database, mail, dan lain-lain.
- **``Database``**: Direktori ini digunakan untuk menyimpan migrasi dan seeder basis data. Dengan fitur migrasi Laravel, Anda dapat dengan mudah mengelola skema database Anda tanpa harus menulis kueri SQL secara manual.
- ``Public``: Folder ini berisi file-file yang dapat diakses publik, seperti file CSS, JavaScript, dan gambar.
- ``Routes``: Di dalam folder ini, Anda akan menemukan file-file yang mendefinisikan rute-rute aplikasi Anda. Laravel menyediakan file web.php untuk rute yang terkait dengan antarmuka web, dan api.php untuk rute API.
Struktur yang jelas dan terorganisir ini memudahkan developer untuk memahami dan mengembangkan aplikasi secara efisien.

## 1.5 Instalasi Laravel

Untuk memulai dengan Laravel, Anda harus memastikan bahwa Anda telah menginstal PHP dan Composer di sistem Anda. Composer adalah dependency manager untuk PHP, dan Laravel menggunakan Composer untuk mengelola berbagai library dan paket yang dibutuhkan.

Berikut adalah langkah-langkah untuk menginstal Laravel:

- **Instal Composer**: Pastikan Composer telah terinstal di komputer Anda. Anda bisa mengunduhnya dari situs resmi Composer.
- **Instal Laravel**: Setelah Composer terinstal, Anda bisa menginstal Laravel melalui command line dengan perintah berikut:

1. ``composer create-project --prefer-dist laravel/laravel nama_proyek``

Dan Anda Bisa Juga Dengan Comman Line Seperti Ini Setelah Anda Sudah Menginstall composer

2. ``laravel new example-app``

Perintah Di Atas akan mengunduh semua dependensi yang diperlukan dan membuat folder baru dengan nama proyek Anda.

Konfigurasi Lingkungan: Setelah instalasi selesai, Anda perlu mengonfigurasi file ``.env`` untuk mengatur detail database dan pengaturan lain yang diperlukan oleh aplikasi Anda. File ``.env`` ini memungkinkan Anda untuk mengatur berbagai variabel lingkungan tanpa harus menyentuh kode utama aplikasi Anda.

3. Setelah semua konfigurasi selesai, Anda bisa menjalankan server lokal Laravel dengan perintah:

``php artisan serve``

Perintah ini akan menjalankan aplikasi Anda di server lokal, biasanya dapat diakses di ``http://localhost:8000``

## 1.6 Memulai Proyek Laravel

Setelah Laravel terinstal, Anda bisa memulai proyek Anda dengan menambahkan fungsionalitas sesuai kebutuhan. Laravel menyediakan berbagai alat yang memudahkan developer untuk membangun aplikasi secara efisien, seperti:

- **Artisan CLI**: Laravel dilengkapi dengan alat command-line interface (CLI) yang disebut Artisan. Dengan Artisan, Anda bisa menjalankan berbagai perintah untuk mengelola proyek Anda, seperti membuat model, controller, dan migrasi basis data.

- **Blade Templating Engine**: Laravel menggunakan Blade sebagai engine template bawaannya. Blade memungkinkan Anda untuk membuat tampilan dengan kode PHP yang bersih dan mudah dibaca. Blade juga mendukung fitur seperti template inheritance, yang memudahkan Anda untuk mengelola struktur tampilan yang kompleks.

- **Eloquent ORM**: Laravel menyediakan Eloquent sebagai Object-Relational Mapping (ORM) untuk berinteraksi dengan database. Dengan Eloquent, Anda bisa mengelola data database Anda menggunakan model PHP, tanpa harus menulis kueri SQL secara langsung.

Dengan semua fitur dan alat yang disediakan, Laravel memberikan fondasi yang kuat bagi siapa saja yang ingin membangun aplikasi web dengan PHP. Ini membuat Laravel tidak hanya populer di kalangan developer, tetapi juga menjadi salah satu framework yang sangat direkomendasikan bagi pemula yang ingin belajar pengembangan web.



