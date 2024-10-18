---
external: false
title: Laravel - 5. Pengelolaan Database dengan Eloquent ORM
description: Pelajari cara mengelola database menggunakan Eloquent ORM di Laravel, mulai dari membuat model, migrasi, query, hingga relasi dan soft deleting.
date: 2024-10-14
---

## 5.1 Apa Itu Eloquent ORM?

Eloquent ORM adalah Object-Relational Mapping (ORM) yang disertakan dalam framework Laravel untuk memudahkan pengelolaan database. ORM seperti Eloquent memungkinkan developer bekerja dengan database menggunakan model berbasis objek, yang berarti Anda tidak perlu menulis query SQL secara manual. Eloquent menyediakan sintaks yang mudah dan intuitif, sehingga developer dapat berinteraksi dengan database menggunakan metode bawaan Laravel yang menyerupai operasi pada objek.

Dalam Eloquent, setiap tabel dalam database direpresentasikan sebagai sebuah model, dan setiap baris pada tabel tersebut menjadi sebuah instance dari model. Dengan pendekatan ini, developer dapat bekerja dengan data seolah-olah sedang memanipulasi objek di dalam aplikasi, membuat pengelolaan database lebih mudah dan terstruktur.

Contoh sederhana adalah jika Anda memiliki tabel users, maka Eloquent akan secara otomatis mengasumsikan adanya model User yang dapat digunakan untuk memanipulasi data dalam tabel tersebut. Misalnya, untuk mengambil data dari tabel users, Anda bisa menulis:

```bash
$users = User::all();
```

Ini akan menghasilkan semua baris dalam tabel users, tanpa perlu menulis query SQL seperti ``SELECT * FROM`` users;. Dengan demikian, Eloquent ORM memudahkan interaksi dengan database tanpa harus memahami detail query SQL secara mendalam.

## 5.2 Migrasi dan Struktur Database

Migrasi dalam Laravel adalah cara untuk mengelola skema database Anda secara versioned dan terorganisir. Dengan migrasi, Anda dapat membuat, mengubah, atau menghapus tabel dalam database secara terstruktur dan mudah dilacak. Migrasi sangat berguna dalam proyek yang sedang berkembang, karena memungkinkan setiap anggota tim untuk memiliki struktur database yang konsisten.

Untuk membuat migrasi baru, Anda bisa menggunakan perintah Artisan:

```bash
php artisan make:migration create_users_table
```

Ini akan menghasilkan file migrasi di direktori ``database/migrations``. File tersebut akan berisi metode ``up()`` dan ``down()`` yang digunakan untuk menentukan perubahan pada skema database. Metode up() biasanya berisi instruksi untuk membuat atau mengubah tabel, sedangkan down() digunakan untuk membatalkan perubahan tersebut.

Contoh migrasi untuk membuat tabel users:

```bash
Schema::create('users', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('email')->unique();
    $table->timestamps();
});
```

Migrasi di atas akan menghasilkan tabel users dengan kolom id, name, email, dan timestamps. Dengan fitur migrasi, Anda dapat memastikan bahwa perubahan pada struktur database dapat di-deploy dengan aman dan konsisten di berbagai lingkungan (development, staging, production).

## 5.3 Membuat Model di Laravel

Setelah tabel dan struktur database dibuat menggunakan migrasi, langkah selanjutnya adalah membuat model yang mewakili tabel tersebut dalam aplikasi Laravel. Di Laravel, model adalah representasi dari entitas yang terdapat dalam tabel database. Model ini akan menjadi "jembatan" antara aplikasi dan database.

Untuk membuat model, Anda dapat menggunakan perintah Artisan berikut:

``php artisan make:model User``

Perintah di atas akan membuat model User di direktori app/Models. Model tersebut secara otomatis akan terhubung dengan tabel users dalam database, mengikuti konvensi penamaan Laravel (nama model dalam bentuk singular akan terhubung ke tabel dalam bentuk plural).

Model yang dibuat dapat digunakan untuk mengelola data pada tabel terkait. Sebagai contoh, untuk menyimpan data pengguna baru ke tabel users, Anda bisa menggunakan:

```bash
$user = new User;
$user->name = 'John Doe';
$user->email = 'johndoe@example.com';
$user->save();
```

Dengan pendekatan ini, Anda dapat memanfaatkan seluruh fitur ORM Eloquent seperti validasi data, query builder, dan relasi antar-tabel tanpa perlu menulis query SQL secara manual.

## 5.4 Menjalankan Query dengan Eloquent

Salah satu fitur utama dari Eloquent ORM adalah kemampuannya untuk menjalankan query database dengan cara yang sangat mirip dengan operasi pada objek. Eloquent menyediakan berbagai metode yang dapat digunakan untuk mengambil, menyimpan, memperbarui, dan menghapus data dalam database.

### Mengambil Data
Untuk mengambil data, Anda bisa menggunakan beberapa metode seperti ``all()``, ``find()``, ``where()``, dan lainnya. Contoh untuk mengambil semua data pengguna:

```bash
$users = User::all();
```

Untuk mengambil satu pengguna berdasarkan ``id``-nya:

``$user = User::find(1);``

Anda juga bisa menggunakan klausa ``where()`` untuk mengambil data berdasarkan kondisi tertentu:

``$user = User::where('email', 'johndoe@example.com')->first();``

### Menyimpan Data

Untuk menyimpan data baru ke dalam tabel, Anda bisa menggunakan metode save() setelah mengisi properti model:

```bash
$user = new User;
$user->name = 'Jane Doe';
$user->email = 'janedoe@example.com';
$user->save();
```

### Memperbarui Data

Untuk memperbarui data yang sudah ada, Anda bisa mencari data yang ingin diubah, kemudian menggunakan metode ``save()`` setelah melakukan perubahan:

```bash
$user = User::find(1);
$user->name = 'John Updated';
$user->save();
```

### Menghapus Data

Untuk menghapus data dari tabel, Eloquent menyediakan metode ``delete()``:

```bash
$user = User::find(1);
$user->delete();
```

Metode-metode ini menunjukkan betapa Eloquent ORM sangat mempermudah interaksi dengan database, membuat query menjadi lebih mudah dipahami dan ditulis tanpa perlu detail query SQL yang rumit.

## 5.5 Relasi dalam Eloquent (One-to-One, One-to-Many, Many-to-Many)

Salah satu fitur paling kuat dari Eloquent adalah kemampuannya untuk mendefinisikan relasi antar-tabel dengan sangat mudah. Relasi ini menggambarkan hubungan antar-entitas dalam database, seperti satu pengguna dapat memiliki banyak postingan, atau satu produk bisa dimiliki oleh banyak kategori.

### One-to-One

Relasi One-to-One digunakan ketika satu record di satu tabel hanya berhubungan dengan satu record di tabel lain. Contoh sederhana adalah relasi antara pengguna dan profilnya. Dalam model User, relasi one-to-one dapat didefinisikan sebagai berikut:

```bash
public function profile() {
    return $this->hasOne(Profile::class);
}
```

### One-to-Many

Relasi One-to-Many digunakan ketika satu entitas memiliki banyak entitas lain. Misalnya, satu pengguna dapat memiliki banyak postingan (user-to-post). Relasi ini dapat didefinisikan sebagai berikut:

```bash
public function posts() {
    return $this->hasMany(Post::class);
}
```

### Many-to-Many

Relasi Many-to-Many menghubungkan banyak entitas di satu tabel dengan banyak entitas di tabel lain. Contohnya adalah relasi antara produk dan kategori, di mana satu produk bisa berada di banyak kategori dan satu kategori bisa memiliki banyak produk. Contoh definisi relasi ini di model:

```bash
public function categories() {
    return $this->belongsToMany(Category::class);
}
```

Eloquent secara otomatis akan mengelola tabel pivot yang dibutuhkan untuk relasi many-to-many, sehingga developer tidak perlu menulis query SQL secara manual untuk menghubungkan data di kedua tabel.

## 5.6 Soft Deleting dan Timestamps

Soft Deleting adalah fitur di Eloquent yang memungkinkan Anda "menghapus" record dari database tanpa benar-benar menghapusnya. Data yang dihapus dengan soft delete akan tetap ada di database, namun tidak akan ditampilkan dalam query standar. Ini sangat berguna jika Anda perlu memulihkan data di kemudian hari.

Untuk mengaktifkan soft delete pada model, Anda hanya perlu menambahkan trait ``SoftDeletes`` ke model tersebut:

```bash
use Illuminate\Database\Eloquent\SoftDeletes;

class User extends Model {
    use SoftDeletes;
}
```

Setelah itu, setiap kali Anda menghapus data menggunakan metode delete(), Laravel tidak akan benar-benar menghapus data tersebut dari tabel, melainkan akan menambahkan ``timestamp`` pada kolom deleted_at.

### Timestamps

Secara default, Eloquent secara otomatis akan menambahkan dua kolom timestamp ke setiap tabel: created_at dan updated_at. Kolom created_at menyimpan waktu saat record dibuat, sementara updated_at menyimpan waktu saat record terakhir diperbarui. Jika Anda tidak memerlukan kolom timestamp, Anda bisa menonaktifkannya dengan menambahkan properti public $timestamps = false; pada model.

Dengan fitur Soft Deleting dan Timestamps, Eloquent ORM memberikan fleksibilitas lebih dalam mengelola data, memungkinkan penghapusan sementara dan pelacakan perubahan waktu dengan mudah.

## Kesimpulan
Dengan demikian, Eloquent ORM adalah alat yang sangat kuat dalam framework Laravel untuk mengelola database secara efisien, dari pembuatan model hingga manajemen relasi dan penghapusan data. Eloquent memungkinkan developer berfokus pada logika bisnis tanpa harus khawatir tentang query SQL yang kompleks.












