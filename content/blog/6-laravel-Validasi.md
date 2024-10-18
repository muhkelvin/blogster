---
external: false
title: Laravel - 6. Konsep Validasi dan Request Handling di Laravel
description: Pelajari konsep controller di Laravel secara mendalam, mulai dari pembuatan controller, penggunaan route resource, dependency injection, middleware, hingga penerapan RESTful controller untuk pengembangan aplikasi dan API yang efisien dan terstruktur.
date: 2024-10-14
---

Laravel adalah salah satu framework PHP yang sangat populer di kalangan developer web karena kemudahan penggunaan, skalabilitas, dan banyak fitur bawaan yang memudahkan pengembangan aplikasi. Salah satu aspek penting dalam pengembangan aplikasi web adalah bagaimana aplikasi menangani request dari pengguna, memvalidasi data yang dikirimkan, dan memberikan response yang sesuai. Laravel menyediakan berbagai tools dan fitur untuk menangani semua ini dengan elegan. Dalam artikel ini, kita akan membahas konsep validasi dan request handling di Laravel dengan detail, mencakup beberapa poin penting seperti request dan response, validasi data input, form request dan custom validation, penanganan file upload, serta error handling dan custom error messages.

## 6.1 Memahami Request dan Response

Dalam aplikasi web, request dan response adalah dua elemen inti yang mendasari interaksi antara client dan server. Ketika pengguna mengakses aplikasi, browser mengirimkan request HTTP ke server, dan server memberikan response yang berisi informasi yang diminta.

### Request

Di Laravel, request mengacu pada semua data yang dikirimkan oleh client, yang bisa berupa data form, URL parameters, atau bahkan file. Laravel menyediakan class Illuminate\Http\Request untuk menangani semua jenis HTTP request. Melalui class ini, kita bisa mengakses data dari request dengan cara yang mudah. Beberapa metode umum yang digunakan untuk mengakses data request di Laravel adalah:

- all(): Mengambil semua data request dalam bentuk array.
- input('key'): Mengambil data spesifik berdasarkan key.
- only(['key1', 'key2']): Mengambil beberapa input spesifik berdasarkan array key.
- except(['key1', 'key2']): Mengambil semua input kecuali key yang disediakan.

Contoh sederhana:

```bash
public function store(Request $request)
{
    $data = $request->all();
    $name = $request->input('name');
    $email = $request->input('email');
    
    // Lakukan sesuatu dengan data tersebut
}
```

Selain itu, Laravel juga menyediakan cara mudah untuk mendeteksi jenis request, seperti apakah itu GET, POST, atau PUT, menggunakan metode ``isMethod()`` atau ``method()``.

### Response

Setelah memproses request, aplikasi akan memberikan response. Response ini bisa berupa HTML, JSON, redirect, atau bahkan file yang di-download oleh pengguna. Laravel membuatnya sangat mudah untuk merespons request dengan berbagai format. Contoh sederhana response bisa dilakukan dengan:

- ``return response('Hello World', 200)``: Mengirimkan response dengan pesan 'Hello World' dan status 200 (OK).
- ``return response()->json(['name' => 'John', 'age' => 25])``: Mengirimkan response dalam format JSON.
- ``return redirect('/home')``: Mengarahkan ulang pengguna ke halaman lain.
Dengan memahami konsep dasar request dan response di Laravel, kita dapat membangun aplikasi yang lebih interaktif dan dinamis, terutama ketika menangani data input dari pengguna.

## 6.2 Validasi Data Input

Setelah menerima request dari pengguna, langkah selanjutnya adalah memvalidasi data input yang dikirimkan. Laravel menyediakan fitur validasi yang sangat mudah digunakan, baik untuk validasi sederhana maupun kompleks.

### Cara Kerja Validasi di Laravel

Validasi di Laravel dapat dilakukan secara manual atau dengan menggunakan validate() method yang tersedia di controller. Validasi dilakukan dengan menentukan aturan yang harus dipenuhi oleh data yang dikirimkan oleh pengguna. Jika data tidak valid, Laravel akan otomatis mengembalikan response dengan pesan error.

Contoh validasi sederhana:

```bash
public function store(Request $request)
{
    $validated = $request->validate([
        'name' => 'required|max:255',
        'email' => 'required|email|unique:users',
        'password' => 'required|min:8',
    ]);

    // Jika validasi lolos, proses data lebih lanjut
}
```

Pada contoh di atas, aturan validasi terdiri dari:

- required: Field harus diisi.
- max:255: Field tidak boleh melebihi 255 karakter.
- email: Field harus berupa alamat email yang valid.
- unique:users: Email harus unik dalam tabel users.
- min:8: Password harus minimal 8 karakter.

### Pesan Error Default

Jika validasi gagal, Laravel akan mengarahkan ulang pengguna ke halaman sebelumnya dengan menampilkan pesan error. Pesan error ini bisa disesuaikan sesuai kebutuhan, yang akan kita bahas lebih lanjut dalam sub-bagian tentang custom error messages.

### Validasi yang Lebih Kompleks

Selain aturan validasi dasar, Laravel juga mendukung validasi yang lebih kompleks seperti:

- Validasi bersyarat menggunakan closure.
- Validasi berdasarkan input lainnya (misalnya, jika field A diisi maka field B wajib diisi).
- Validasi berdasarkan state (misalnya, jika pengguna dalam peran tertentu, validasi berubah).

```bash
$request->validate([
    'name' => 'required_if:is_admin,true',
    'password' => [
        'required',
        function ($attribute, $value, $fail) {
            if ($value !== 'my-secret') {
                $fail($attribute.' is invalid.');
            }
        },
    ],
]);
```

## 6.3 Form Request dan Custom Validations

Untuk aplikasi yang lebih besar, meletakkan aturan validasi langsung di dalam controller bisa membuat kode menjadi sulit dibaca dan dipelihara. Laravel menawarkan solusi yang lebih elegan melalui penggunaan Form Request.

### Form Request

Form Request adalah class khusus yang digunakan untuk memisahkan logika validasi dari controller. Kita bisa membuat Form Request dengan menjalankan perintah artisan berikut:

```bash
php artisan make:request StoreUserRequest
```
Class ini akan disimpan di dalam direktori ``app/Http/Requests``. Di dalam Form Request, kita bisa mendefinisikan aturan validasi di method ``rules()``:

```bash
class StoreUserRequest extends FormRequest
{
    public function rules()
    {
        return [
            'name' => 'required|max:255',
            'email' => 'required|email|unique:users',
            'password' => 'required|min:8',
        ];
    }
}
```

Setelah itu, kita bisa menggunakan Form Request ini di controller:

```bash
public function store(StoreUserRequest $request)
{
    // Validasi otomatis dijalankan, kita tinggal memproses data
}
```

### Custom Validation Rules

Laravel juga memungkinkan kita membuat aturan validasi khusus (custom validation). Aturan ini sangat berguna jika kita membutuhkan logika validasi yang tidak dapat diakomodasi oleh aturan bawaan Laravel.

Untuk membuat custom validation rule, kita bisa menjalankan perintah:

```bash
php artisan make:rule ValidPassword
```
Kemudian kita bisa mengisi logika di dalam method ``passes()``:

```bash
class ValidPassword implements Rule
{
    public function passes($attribute, $value)
    {
        return $value === 'my-secret-password';
    }

    public function message()
    {
        return 'The :attribute is invalid.';
    }
}
```

Lalu kita bisa menggunakan rule ini di dalam validasi:

```bash
$request->validate([
    'password' => ['required', new ValidPassword],
]);
```

## 6.4 Menangani File Upload

Selain validasi data input seperti teks, angka, atau email, seringkali kita juga perlu menangani file upload di dalam aplikasi. Laravel menyediakan dukungan yang sangat baik untuk penanganan file upload.

Proses Upload File
Untuk menangani file upload, pertama-tama kita harus memastikan form HTML menggunakan atribut enctype="multipart/form-data". Setelah itu, kita bisa menggunakan metode file() di dalam request untuk mengambil file yang di-upload.

```bash
public function store(Request $request)
{
    $request->validate([
        'file' => 'required|mimes:jpg,png,jpeg|max:2048',
    ]);

    if ($request->file('file')->isValid()) {
        $path = $request->file('file')->store('uploads');

        // Lakukan sesuatu dengan path file yang disimpan
    }
}
```
Dalam contoh di atas:

- mimes:jpg,png,jpeg: Memastikan file yang di-upload adalah gambar dengan ekstensi JPG, PNG, atau JPEG.
- max:2048: Membatasi ukuran file maksimal 2 MB.
- store('uploads'): Menyimpan file ke folder uploads di dalam storage.
- Penyimpanan File
Laravel menyediakan beberapa pilihan lokasi penyimpanan file. Secara default, file akan disimpan di direktori storage/app. Namun, kita bisa mengonfigurasi sistem penyimpanan ini melalui config/filesystems.php. Laravel mendukung penyimpanan di berbagai platform seperti local, Amazon S3, dan FTP.

Selain metode ``store()``, ada juga beberapa metode lain untuk menangani file:

- storeAs('uploads', 'filename.ext'): Menyimpan file dengan nama yang kita tentukan.
- move('uploads', 'filename.ext'): Memindahkan file ke lokasi tertentu.
- getClientOriginalName(): Mengambil nama asli file yang di-upload.
- Menangani Multiple File Upload
Laravel juga mendukung multiple file upload. Kita hanya perlu mengubah form HTML menjadi mendukung pengiriman banyak file (multiple), lalu menggunakan loop untuk memproses setiap file di server.

```bash
public function store(Request $request)
{
    $request->validate([
        'files.*' => 'required|mimes:jpg,png,jpeg|max:2048',
    ]);

    foreach ($request->file('files') as $file) {
        $file->store('uploads');
    }
}
```

## 6.5 Error Handling dan Custom Error Messages

Dalam proses request handling dan validasi, error handling sangat penting untuk memberikan feedback yang baik kepada pengguna. Laravel menyediakan mekanisme yang fleksibel untuk menangani error dan menampilkan pesan kesalahan yang sesuai.

### Default Error Handling

Secara default, Laravel akan mengarahkan ulang pengguna ke halaman sebelumnya jika validasi gagal, dengan membawa pesan error melalui session. Di dalam blade template, kita bisa menampilkan error tersebut dengan menggunakan kode seperti ini:

```bash
@if ($errors->any())
    <div class="alert alert-danger">
        <ul>
            @foreach ($errors->all() as $error)
                <li>{{ $error }}</li>
            @endforeach
        </ul>
    </div>
@endif
```

### Custom Error Messages

Laravel memungkinkan kita untuk menyesuaikan pesan error sesuai dengan kebutuhan kita. Kita bisa menambahkan custom error message dalam validasi seperti ini:

```bash
$request->validate([
    'name' => 'required|max:255',
    'email' => 'required|email|unique:users',
], [
    'name.required' => 'Nama wajib diisi.',
    'email.required' => 'Email wajib diisi.',
    'email.email' => 'Format email tidak valid.',
]);
```
Selain itu, kita juga bisa membuat custom messages yang lebih dinamis menggunakan placeholder ``:attribute``, ``:min``, ``:max``, dll.

### Handling Validation Exception
Laravel menangani validasi secara otomatis, tetapi jika kita ingin menangani error secara manual atau membuat handling yang lebih kompleks, kita bisa melakukannya dengan menggunakan exception handling. Laravel menyediakan ValidationException yang bisa kita tangani di app/Exceptions/Handler.php.

## Kesimpulan
Dengan fitur-fitur validasi dan request handling yang lengkap di Laravel, kita bisa membangun aplikasi yang aman, terstruktur, dan mudah dikelola. Penanganan request dan validasi yang baik adalah kunci untuk menjaga kualitas dan keamanan aplikasi, terutama dalam menangani data input dari pengguna. Laravel memberikan solusi yang fleksibel dan kuat untuk kebutuhan ini, memungkinkan developer untuk fokus pada pengembangan fitur aplikasi tanpa harus khawatir tentang validasi dan request handling yang rumit.
