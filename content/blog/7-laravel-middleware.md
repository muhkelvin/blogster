---
external: false
title: Laravel - 7. Konsep Middleware di Laravel: Panduan Lengkap
description: Pelajari konsep validasi dan request handling di Laravel secara mendalam, mulai dari request dan response, validasi data input, penggunaan Form Request, custom validation, hingga penanganan file upload dan error handling. Temukan tips dan trik untuk membangun aplikasi web yang aman dan terstruktur dengan Laravel.
date: 2024-10-14
---

## 7.1 Apa Itu Middleware?
Dalam ekosistem framework Laravel, salah satu konsep penting yang sering kali digunakan untuk mengatur alur logika aplikasi web adalah Middleware. Middleware adalah lapisan pemrosesan antara permintaan HTTP yang masuk (incoming requests) dan respon yang dikirimkan oleh aplikasi. Secara sederhana, konsep Middleware di Laravel berfungsi sebagai "penjaga gerbang" yang memproses permintaan sebelum mencapai controller atau bagian lain dari aplikasi, dan setelah respons dihasilkan dari controller, ia juga dapat memproses respons tersebut sebelum dikirimkan kembali ke pengguna.

Fungsi utama Middleware adalah memeriksa, memodifikasi, atau mengatur aliran permintaan dan respons berdasarkan kondisi tertentu. Sebagai contoh, aplikasi bisa menggunakan Middleware untuk memverifikasi apakah pengguna sudah terautentikasi atau memiliki izin yang sesuai sebelum mengakses halaman tertentu. Ini memungkinkan kontrol akses yang fleksibel dan terorganisir dengan baik, tanpa perlu mencampurkan logika otentikasi dan otorisasi ke dalam controller itu sendiri.

Contoh Sederhana Middleware:
Misalnya, saat pengguna mengakses dashboard admin, Middleware bisa memastikan bahwa hanya pengguna yang telah login dan memiliki peran "admin" yang dapat mengakses halaman tersebut. Jika tidak memenuhi syarat, Middleware dapat langsung mengalihkan pengguna ke halaman login atau halaman lain yang sesuai.

Di Laravel, Middleware sangat mudah digunakan dan dikonfigurasikan, baik dengan menggunakan Middleware bawaan maupun dengan membuat custom Middleware yang disesuaikan dengan kebutuhan aplikasi.

## 7.2 Menggunakan Middleware Built-in
Laravel menyediakan berbagai Middleware bawaan (built-in) yang dapat langsung digunakan dalam aplikasi. Ini adalah cara yang cepat dan efisien untuk menambahkan logika kontrol ke dalam alur aplikasi tanpa perlu menulis terlalu banyak kode.

Beberapa contoh Middleware built-in di Laravel termasuk:

auth: Middleware ini digunakan untuk memastikan bahwa pengguna telah terautentikasi sebelum mereka dapat mengakses rute tertentu.
guest: Middleware ini memastikan bahwa pengguna yang mencoba mengakses halaman tertentu belum login.
verified: Middleware ini memastikan bahwa pengguna telah memverifikasi alamat email mereka sebelum melanjutkan.
throttle: Middleware ini membatasi jumlah permintaan yang dapat dibuat oleh pengguna dalam periode waktu tertentu, berguna untuk mencegah serangan brute force atau penyalahgunaan API.
Contoh Penggunaan Middleware Built-in:
Misalnya, jika Anda ingin membatasi akses ke halaman profil pengguna hanya bagi pengguna yang telah login, Anda dapat menggunakan Middleware auth di rute seperti ini:

php
Copy code
Route::get('/profile', function () {
    // Halaman profil pengguna
})->middleware('auth');
Dengan demikian, setiap kali pengguna mencoba mengakses rute /profile, Laravel akan terlebih dahulu memeriksa apakah pengguna sudah login atau belum. Jika belum, pengguna akan diarahkan ke halaman login secara otomatis.

## 7.3 Membuat Custom Middleware
Selain menggunakan Middleware built-in, Laravel juga memungkinkan pengembang untuk membuat custom Middleware sesuai dengan kebutuhan aplikasi. Hal ini sangat berguna ketika ada logika spesifik yang harus diterapkan di seluruh aplikasi, misalnya memeriksa apakah pengguna memiliki status tertentu di basis data sebelum mengakses halaman atau memverifikasi header tertentu pada permintaan API.

Langkah-Langkah Membuat Custom Middleware:
Buat Middleware Baru: Middleware dapat dibuat menggunakan Artisan command berikut:

bash
Copy code
php artisan make:middleware CekUsia
Perintah ini akan menghasilkan file CekUsia.php di dalam folder app/Http/Middleware/.

Definisikan Logika di Middleware: Setelah file Middleware dibuat, Anda bisa menentukan logika yang diinginkan. Misalnya, kita ingin memeriksa apakah usia pengguna di atas 18 tahun sebelum mengizinkan mereka mengakses rute tertentu:

php
Copy code
namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class CekUsia
{
    public function handle(Request $request, Closure $next)
    {
        if ($request->age < 18) {
            return redirect('home');
        }

        return $next($request);
    }
}
Dalam contoh ini, Middleware akan memeriksa apakah parameter age di permintaan lebih kecil dari 18. Jika ya, pengguna akan diarahkan kembali ke halaman home. Jika tidak, permintaan akan diteruskan ke tahap selanjutnya (controller atau Middleware lainnya).

Registrasi Middleware: Setelah Middleware dibuat, kita harus mendaftarkannya di file Kernel.php yang ada di dalam folder app/Http/. Di sana, Anda bisa mendaftarkan custom Middleware di bagian $routeMiddleware, seperti berikut:

php
Copy code
protected $routeMiddleware = [
    'cekusia' => \App\Http\Middleware\CekUsia::class,
];
Terapkan Middleware di Rute: Setelah registrasi, Anda bisa menerapkan Middleware ini di rute mana pun yang sesuai. Misalnya:

php
Copy code
Route::get('/konten-dewasa', function () {
    // Halaman khusus dewasa
})->middleware('cekusia');
Dengan demikian, Middleware CekUsia akan memeriksa usia pengguna sebelum mengizinkan mereka mengakses konten dewasa.

7.4 Mengaplikasikan Middleware pada Rute atau Grup
Setelah Middleware dibuat dan terdaftar, kita bisa menerapkannya pada rute individu atau grup rute untuk mengelola akses secara lebih efisien. Laravel memungkinkan penerapan Middleware pada dua tingkat:

Rute Individu: Middleware bisa diterapkan pada satu rute saja untuk membatasi akses pada rute tertentu.

php
Copy code
Route::get('/dashboard', function () {
    // Halaman dashboard
})->middleware('auth');
Grup Rute: Middleware juga bisa diterapkan pada sekelompok rute yang memiliki karakteristik serupa. Ini sangat membantu jika Anda memiliki banyak rute yang membutuhkan logika Middleware yang sama. Contohnya:

php
Copy code
Route::middleware(['auth', 'verified'])->group(function () {
    Route::get('/profile', function () {
        // Halaman profil
    });

    Route::get('/orders', function () {
        // Halaman pesanan
    });
});
Dalam contoh ini, semua rute yang ada dalam grup hanya dapat diakses oleh pengguna yang telah login dan telah memverifikasi email mereka.

## 7.5 Global vs Lokal Middleware
Dalam Laravel, Middleware bisa diterapkan secara global atau lokal. Keduanya memiliki perbedaan penting dalam hal cara penerapannya:

1. Global Middleware:
Global Middleware adalah Middleware yang diterapkan ke seluruh permintaan yang masuk ke aplikasi, terlepas dari rute apa pun yang diakses. Middleware ini biasanya digunakan untuk logika yang harus berlaku untuk semua permintaan, seperti logging, pengaturan bahasa, atau memeriksa apakah aplikasi sedang dalam mode maintenance.

Untuk mendaftarkan Middleware sebagai global, Anda cukup menambahkannya ke array $middleware di Kernel.php, seperti berikut:

php
Copy code
protected $middleware = [
    \App\Http\Middleware\CekMaintenanceMode::class,
    \App\Http\Middleware\TrimStrings::class,
];
Global Middleware akan dieksekusi untuk setiap permintaan, tanpa memandang rute atau controller yang akan dituju.

2. Lokal Middleware:
Lokal Middleware adalah Middleware yang diterapkan hanya pada rute atau grup rute tertentu. Ini memungkinkan kontrol yang lebih granular, di mana Anda dapat menentukan dengan tepat kapan dan di mana Middleware akan digunakan. Sebagai contoh, Middleware auth sering kali digunakan secara lokal untuk membatasi akses hanya ke beberapa rute yang memerlukan otentikasi.

Dalam praktiknya, penggunaan Middleware di Laravel sangat fleksibel dan bisa disesuaikan dengan kebutuhan aplikasi. Baik menggunakan Middleware bawaan maupun membuat custom Middleware, Laravel menyediakan antarmuka yang mudah dan efisien untuk mengelola aliran permintaan dan respons di aplikasi web.

Kesimpulan
Konsep Middleware di Laravel adalah salah satu fitur yang sangat kuat dan serbaguna untuk mengontrol alur logika aplikasi. Dengan menggunakan Middleware, pengembang bisa mengatur otentikasi, otorisasi, logging, dan berbagai tindakan lainnya di sepanjang siklus hidup permintaan HTTP. Laravel menyediakan Middleware built-in untuk tugas-tugas umum, namun juga memungkinkan pengembang untuk membuat custom Middleware sesuai dengan kebutuhan spesifik aplikasi.

Kemampuan untuk menerapkan Middleware baik secara global maupun lokal membuatnya sangat fleksibel, memungkinkan pengembang untuk dengan mudah mengatur akses dan memanipulasi aliran logika di seluruh aplikasi dengan cara yang terorganisir dan efisien.
