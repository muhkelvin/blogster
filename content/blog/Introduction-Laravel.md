---
external: false
title: Laravel - Introduction Laravel
description: Laravel introduction
date: 2024-10-14
---

# Apa Itu Controller di Laravel? Pengertian dan Contoh Pembuatannya

Controller di Laravel adalah komponen penting dalam pola arsitektur MVC (Model-View-Controller). Controller bertugas untuk mengelola logika aplikasi dan menghubungkan antara model (data) dan view (tampilan). Dengan menggunakan controller, pengelolaan kode menjadi lebih terstruktur dan mudah dipelihara.

## Fungsi Controller di Laravel

Dalam Laravel, controller memungkinkan pemisahan logika bisnis dari routing dan tampilan, sehingga kode lebih terorganisir. Controller bertindak sebagai penghubung antara pengguna dan aplikasi, menerima permintaan (request) dari pengguna, memprosesnya, dan mengembalikan respon yang sesuai (response).

### Keuntungan Menggunakan Controller
- **Organisasi Kode yang Baik**: Memisahkan logika aplikasi dan tampilan, sehingga lebih mudah dipelihara.
- **Reusabilitas**: Kode yang terdapat di dalam controller bisa digunakan kembali di berbagai tempat.
- **Mudah Diuji**: Controller memungkinkan pengujian logika bisnis dengan lebih baik.

## Cara Membuat Controller di Laravel

Untuk membuat controller di Laravel, kita dapat menggunakan perintah artisan, yaitu perintah CLI bawaan Laravel. Berikut langkah-langkah dan contoh pembuatannya.

### 1. Membuat Controller dengan Artisan

Laravel menyediakan perintah `make:controller` yang memudahkan kita dalam membuat controller baru. Jalankan perintah berikut di terminal:

Berikut ini contoh kode untuk membuat controller di Laravel:
```bash

<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class ProdukController extends Controller
{
    public function index()
    {
        $produk = ['Produk A', 'Produk B', 'Produk C'];
        return view('produk.index', ['produk' => $produk]);
    }
}
