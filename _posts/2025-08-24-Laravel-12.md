---
title: CRUD Laravel 12
date: 2025-09-21
categories: [Laravel, CRUD]
tags: [Laravel]
pin: true
---
# Tutorial CRUD Laravel 12 dengan Tailwind CSS 4

## Overview

Tutorial CRUD Laravel 12 ini akan memandu Anda membangun aplikasi manajemen produk sederhana namun fungsional dengan tampilan responsif menggunakan **Tailwind CSS 4**. Aplikasi ini akan menjadi contoh praktis implementasi konsep **MVC (Model-View-Controller)** dalam ekosistem Laravel.

Kita akan membangun aplikasi dengan studi kasus pengelolaan data produk yang memiliki atribut **`name`**, **`code`**, **`stock`**, dan **`price`**. Sepanjang tutorial, kita akan mengimplementasikan seluruh siklus pengembangan—mulai dari konfigurasi database, pembuatan model, hingga implementasi antarmuka pengguna yang intuitif.

-----

## Apa yang akan dipelajari

Dalam tutorial ini, Anda akan mempelajari:

  * Cara membuat project Laravel 12 baru dengan composer.
  * Konfigurasi database MySQL untuk aplikasi Laravel.
  * Pembuatan model dan migrasi database sesuai kebutuhan aplikasi.
  * Implementasi `Controller` dengan metode `Resource` untuk operasi CRUD terstruktur.
  * Pembuatan tampilan responsif menggunakan Blade template engine dan Tailwind CSS.
  * Validasi data input untuk menjaga integritas dan keamanan aplikasi.
  * Penggunaan `route resource` untuk mengelola endpoint API secara efisien.
  * Pengelolaan relasi antar data dengan Eloquent ORM.
  * Penerapan notifikasi *flash message* untuk feedback pengguna yang lebih baik.

-----

## Apa yang perlu dipersiapkan

Sebelum memulai tutorial ini, pastikan Anda telah menyiapkan:

  * **PHP versi 8.2** atau lebih tinggi (persyaratan minimal Laravel 12).
  * **Composer** sebagai package manager untuk PHP.
  * **MySQL** atau database serupa yang kompatibel dengan Laravel.
  * **Text editor** atau IDE (disarankan: Visual Studio Code dengan ekstensi PHP).
  * **Browser web** modern untuk pengujian aplikasi.
  * Pengetahuan dasar **PHP** dan konsep **MVC**.
  * Pemahaman dasar **HTML** dan **CSS**.
  * Akses ke **Terminal** atau **Command Prompt**.

-----

## Apa yang akan kita build

Melalui tutorial ini, Anda akan membangun aplikasi manajemen produk lengkap dengan fitur:

  * Halaman daftar produk dengan fitur **pagination** untuk navigasi data yang efisien.
  * Form untuk **menambahkan produk baru** dengan validasi input.
  * Halaman **detail** untuk melihat informasi lengkap produk.
  * Form untuk **mengedit dan memperbarui** data produk yang sudah ada.
  * Fungsionalitas **hapus data** dengan konfirmasi untuk mencegah penghapusan yang tidak disengaja.
  * Antarmuka **responsif** menggunakan Tailwind CSS untuk pengalaman pengguna yang optimal di berbagai perangkat.
  * **Validasi data** di sisi server untuk memastikan integritas informasi.
  * **Notifikasi interaktif** untuk memberi tahu pengguna tentang hasil operasi CRUD.

Tutorial ini akan menunjukkan bagaimana Laravel 12 menyederhanakan pengembangan aplikasi web modern dengan pendekatan yang bersih dan terstruktur. Konsep-konsep yang dipelajari dapat diterapkan untuk berbagai jenis proyek Laravel lainnya, memberikan Anda fondasi solid untuk terus mengembangkan keterampilan web development Anda.

-----

## Step 1: Buat Project Laravel

Buka terminal, lalu jalankan command berikut ini untuk membuat project Laravel baru.

```bash
composer create-project --prefer-dist laravel/laravel sample-app
```

Kita tunggu sampai proses *create project* menggunakan composer selesai. Setelah selesai, kita pindah dulu ke direktori project.

```bash
cd sample-app
```

Lalu kita buka project kita di code editor. Di sini saya buka direktori project menggunakan Visual Studio Code dengan menjalankan command berikut.

```bash
code .
```

Setelah kita menjalankan command di atas, direktori project akan terbuka di Visual Studio Code.

-----

## Step 2: Setup Konfigurasi Database

Pada step ini kita akan mengatur konfigurasi database untuk project kita. Buka file `.env`, lalu sesuaikan konfigurasi database.

```ini
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=db_belajar_laravel
DB_USERNAME=root
DB_PASSWORD=password
```

Di baris kode di atas, kita atur database yang akan kita gunakan adalah **`mysql`** dengan nama database **`db_belajar_laravel`** dan credentials mysql. Untuk credential mysql (username dan password) sesuaikan dengan credential mysql di komputer masing-masing.

-----

## Step 3: Buat Model dan Migration

Pada step ini kita buat file model dan migration sesuai dengan studi kasus, yaitu model dan migration untuk data produk. Untuk membuat file model dan migration, buka terminal lalu jalankan command berikut ini.

```bash
php artisan make:model Product -m
```

Output yang ditampilkan:

```
$ php artisan make:model Product -m

   INFO  Model [app/Models/Product.php] created successfully.  

   INFO  Migration [database/migrations/2025_02_26_232350_create_products_table.php] created successfully.
```

Selanjutnya kita buka file `app/Models/Product.php`, lalu kita tambahkan atribut `$fillable` di class `Product`.

```php
<?php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Product extends Model
{
    protected $fillable = [
        'code',
        'name',
        'price',
        'stock',
    ];
}
```

Save kembali file `app/Models/Product.php`. Pada baris kode di atas, kita atur supaya kolom `code`, `name`, `price` dan `stock` dapat diisi secara massal melalui input pengguna atau data yang dikirimkan dari form.

Selanjutnya kita sesuaikan file migration dengan studi kasus tutorial ini. Buka file migration `database/migrations/xxxx_xx_xx_xxxxxx_create_products_table.php`, lalu kita tambahkan field `code`, `name`, `price` dan `stock` untuk table `products`.

```php
<?php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->string('code')->unique();
            $table->string('name');
            $table->decimal('price', 20, 2);
            $table->integer('stock');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('products');
    }
};
```

Save kembali file `database/migrations/xxxx_xx_xx_xxxxxx_create_products_table.php`.

-----

## Step 4: Run Migration

Pada step ini kita jalankan migration command untuk menambahkan table `products` di database `db_belajar_laravel`. Apabila teman-teman belum membuat file `db_belajar_laravel`, kita bisa membuat langsung ketika kita menjalankan migration command.

Sekarang kita buka kembali terminal, lalu jalankan migration command.

```bash
php artisan migrate
```

Apabila kita belum membuat database, akan tampil prompt untuk membuat database.

```
$ php artisan migrate

   WARN  The database 'db_belajar_laravel' does not exist on the 'mysql' connection.  

 ┌ Would you like to create it? ────────────────────────────────┐
 │ ● Yes / ○ No                                                 │
```

Selanjutnya kita pilih **`yes`**, lalu tekan `enter` untuk melanjutkan.

```
$ php artisan migrate

   WARN  The database 'db_belajar_laravel' does not exist on the 'mysql' connection.  

 ┌ Would you like to create it? ────────────────────────────────┐
 │ Yes                                                          │
 └──────────────────────────────────────────────────────────────┘

   INFO  Preparing database.  

  Creating migration table ...................................... 41.65ms DONE

   INFO  Running migrations.  

  0001_01_01_000000_create_users_table ......................... 144.60ms DONE
  0001_01_01_000001_create_cache_table .......................... 55.96ms DONE
  0001_01_01_000002_create_jobs_table .......................... 119.18ms DONE
  2025_02_26_232350_create_products_table ....................... 52.55ms DONE
```

Setelah selesai, kita bisa lihat ada database dan juga table ketika kita cek menggunakan tools seperti phpMyAdmin ataupun menjalankan command SQL untuk cek database.

-----

## Step 5: Coding Fitur View Daftar Product

Persiapan setup sudah selesai, sekarang kita masuk ke coding fitur yang pertama, yaitu fitur view daftar produk. Untuk membuat fitur pertama, kita buat controller untuk menangani proses manage product.

```bash
php artisan make:controller ProductController --model=Product --resource
```

Output:

```
$ php artisan make:controller ProductController --model=Product --resource

   INFO  Controller [app/Http/Controllers/ProductController.php] created successfully.
```

Selanjutnya kita buka file controller `app/Http/Controllers/ProductController.php`. Pada class `ProductController`, kita modifikasi method `index()`.

```php
public function index()
{
    $products = Product::latest()->paginate(10);
    return view('products.index', compact('products'));
}
```

Save kembali file `app/Http/Controllers/ProductController.php`. Selanjutnya kita buat file untuk menampilkan daftar produk, yaitu view `resources/views/products/index.blade.php`, lalu tambahkan baris kode berikut ini.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>Product List - Tutorial CRUD Laravel 12 </title>
    <script src="https://unpkg.com/@tailwindcss/browser@4"></script>
</head>
<body>
<div class="container mx-auto mt-10 mb-10 px-10">
    <div class="grid grid-cols-8 gap-4 mb-4 p-5">
        <div class="col-span-4 mt-2">
            <h1 class="text-3xl font-bold">
                DAFTAR PRODUCT
            </h1>
        </div>
        <div class="col-span-4">
            <div class="flex justify-end">
                <a href="{{ route('product.create') }}"
                   class="inline-block px-6 py-2.5 bg-blue-600 text-white font-medium text-xs leading-tight uppercase rounded-full shadow-md hover:bg-blue-700 hover:shadow-lg focus:bg-blue-700 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 active:shadow-lg transition duration-150 ease-in-out"
                   id="add-product-btn">Add Product</a>
            </div>
        </div>
    </div>
    <div class="bg-white p-5 rounded shadow-sm">
        @if (session('success'))
            <div class="p-3 rounded bg-green-500 text-green-100 mb-4">
                {{ session('success') }}
            </div>
        @endif
        <div class="relative overflow-x-auto">
            <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
                <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
                <tr>
                    <th scope="col" class="px-6 py-3">No</th>
                    <th scope="col" class="px-6 py-3">Product name</th>
                    <th scope="col" class="px-6 py-3">Code</th>
                    <th scope="col" class="px-6 py-3">Stock</th>
                    <th scope="col" class="px-6 py-3">Price</th>
                    <th scope="col" class="px-6 py-3">Action</th>
                </tr>
                </thead>
                <tbody>
                @forelse ($products as $product)
                    <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 border-gray-200">
                        <td class="px-6 py-4">{{ $loop->iteration }}</td>
                        <td class="px-6 py-4">{{ $product->name}}</td>
                        <td class="px-6 py-4">{{ $product->code }}</td>
                        <td class="px-6 py-4">{{ $product->stock }}</td>
                        <td class="px-6 py-4">{{ $product->price}}</td>
                        <td class="px-6 py-4">
                            <form onsubmit="return confirm('Apakah Anda Yakin ?');"
                                  action="{{ route('product.destroy', $product) }}" method="POST">
                                @csrf
                                @method('DELETE')
                                <a href="{{ route('product.show', $product) }}" id="{{ $product->id }}-edit-btn"
                                   class="inline-block px-6 py-2.5 bg-blue-400 text-white font-medium text-xs leading-tight uppercase rounded-full shadow-md hover:bg-blue-500 hover:shadow-lg focus:bg-blue-500 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-600 active:shadow-lg transition duration-150 ease-in-out">View</a>
                                <a href="{{ route('product.edit', $product) }}" id="{{ $product->id }}-edit-btn"
                                   class="inline-block px-6 py-2.5 bg-blue-400 text-white font-medium text-xs leading-tight uppercase rounded-full shadow-md hover:bg-blue-500 hover:shadow-lg focus:bg-blue-500 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-600 active:shadow-lg transition duration-150 ease-in-out">Edit</a>
                                <button type="submit"
                                        class="inline-block px-6 py-2.5 bg-red-600 text-white font-medium text-xs leading-tight uppercase rounded-full shadow-md hover:bg-red-700 hover:shadow-lg focus:bg-red-700 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-red-800 active:shadow-lg transition duration-150 ease-in-out"
                                        id="{{ $product->id }}-delete-btn">Delete</button>
                            </form>
                        </td>
                    </tr>
                @empty
                    <tr>
                        <td class="text-center text-sm text-gray-900 px-6 py-4 whitespace-nowrap" colspan="6">
                            Data Empty
                        </td>
                    </tr>
                @endforelse
                </tbody>
            </table>
        </div>
    </div>
    <div class="mt-3">
        {{ $products->links() }}
    </div>
</div>
</body>
</html>
```

Save kembali `resources/views/products/index.blade.php`. Setelah coding controller dan view selesai, kita tambahkan route baru di file `routes/web.php`.

```php
Route::resource('product', \App\Http\Controllers\ProductController::class);
```

Untuk mengecek route yang sudah ditambahkan kita bisa jalankan command berikut ini.

```bash
php artisan route:list
```

Output:

```
$ php artisan route:list

  GET|HEAD        / .......................................................... 
  GET|HEAD        product ............ product.index › ProductController@index
  POST            product ............ product.store › ProductController@store
  GET|HEAD        product/create ... product.create › ProductController@create
  GET|HEAD        product/{product} .... product.show › ProductController@show
  PUT|PATCH       product/{product} product.update › ProductController@update
  DELETE          product/{product} product.destroy › ProductController@destr…
  GET|HEAD        product/{product}/edit product.edit › ProductController@edit
  GET|HEAD        storage/{path} ............................... storage.local
  GET|HEAD        up ......................................................... 

                                                           Showing [10] routes
```

Seperti yang terlihat pada output yang ditampilkan, route yang kita tambahkan akan menangani semua proses untuk manage product.

-----

## Step 6: Coding Fitur Create Data

Pada step ini kita akan menambahkan fitur untuk menambahkan data produk. Kita buka kembali `app/Http/Controllers/ProductController.php`, lalu kita modifikasi method `create()`.

```php
public function create()
{
    return view('products.create');
}
```

Seperti yang terlihat pada baris kode di atas, file view untuk menampilkan form create data produk adalah `resources/views/products/create.blade.php`. Buat file `resources/views/products/create.blade.php`, lalu kita coding baris kode berikut ini.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>Create New Product - Tutorial CRUD Laravel 12 </title>
    <script src="https://unpkg.com/@tailwindcss/browser@4"></script>
</head>
<body>
<div class="container mx-auto mt-10 mb-10 px-10">
    <div class="grid grid-cols-8 gap-4 p-5">
        <div class="col-span-4 mt-2">
            <h1 class="text-3xl font-bold">
                CREATE NEW PRODUCT
            </h1>
        </div>
    </div>
    <div class="bg-white p-5 rounded shadow-sm">
        <form action="{{ route('product.store') }}" method="POST">
            @csrf

            <div class="mb-5">
                <label for="name">Name</label>
                <input type="text" class="form-control block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded-full transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" name="name" value="{{ old('name') }}" required>
                @error('name')
                <div class="bg-red-400 p-2 shadow-sm rounded mt-2">
                    {{ $message }}
                </div>
                @enderror
            </div>

            <div class="mb-5">
                <label for="code">Code</label>
                <input type="text" class="form-control block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded-full transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" name="code" value="{{ old('code') }}" required>
                @error('code')
                <div class="bg-red-400 p-2 shadow-sm rounded mt-2">
                    {{ $message }}
                </div>
                @enderror
            </div>

            <div class="mb-5">
                <label for="price">Price</label>
                <input type="text" class="form-control block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded-full transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" name="price" value="{{ old('price') }}" required>
                @error('price')
                <div class="bg-red-400 p-2 shadow-sm rounded mt-2">
                    {{ $message }}
                </div>
                @enderror
            </div>

            <div class="mb-5">
                <label for="stock">Stock</label>
                <input type="text" class="form-control block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded-full transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" name="stock" value="{{ old('stock') }}" required>
                @error('stock')
                <div class="bg-red-400 p-2 shadow-sm rounded mt-2">
                    {{ $message }}
                </div>
                @enderror
            </div>

            <div class="mt-3">
                <button type="submit" class="inline-block px-6 py-2.5 bg-blue-600 text-white font-medium text-xs leading-tight uppercase rounded-full shadow-md hover:bg-blue-700 hover:shadow-lg focus:bg-blue-700 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 active:shadow-lg transition duration-150 ease-in-out">
                    Save
                </button>
                <a href="{{ route('product.index') }}" class="inline-block px-6 py-2.5 bg-gray-200 text-gray-700 font-medium text-xs leading-tight uppercase rounded-full shadow-md hover:bg-gray-300 hover:shadow-lg focus:bg-gray-300 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-gray-400 active:shadow-lg transition duration-150 ease-in-out">back</a>
            </div>
        </form>
    </div>
</div>
</body>
</html>
```

Save kembali file `resources/views/products/create.blade.php`. Proses tambah data pada form diarahkan ke route `post.store` yang diarahkan ke method `store()`. Buka kembali `app/Http/Controllers/ProductController.php`, lalu kita modifikasi method `store()`.

```php
public function store(Request $request)
{
    $request->validate([
        'name' => 'required',
        'code' => 'required|unique:products',
        'price' => 'required',
        'stock' => 'required',
    ]);

    Product::create([
        'name' => $request->name,
        'code' => $request->code,
        'price' => $request->price,
        'stock' => $request->stock,
    ]);

    return redirect()->route('product.index')
        ->with('success','Product created successfully.');
}
```

Save kembali `app/Http/Controllers/ProductController.php`.

-----

## Step 7: Coding Fitur View Satu Data

Untuk menambahkan fitur view satu data, kita modifikasi kembali controller `app/Http/Controllers/ProductController.php`. Pada controller kita modifikasi method yang menangani fitur tersebut, yaitu method `show()`.

```php
public function show(Product $product)
{
    return view('products.show',compact('product'));
}
```

Selanjutnya kita buat file view `resources/views/products/show.blade.php`, lalu coding baris kode berikut ini.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>Product Detail {{ $product->name }} - Tutorial CRUD Laravel 12 </title>
    <script src="https://unpkg.com/@tailwindcss/browser@4"></script>
</head>
<body>
<div class="container mx-auto mt-10 mb-10 px-10">
    <div class="grid grid-cols-8 gap-4 mb-4 p-5">
        <div class="col-span-4 mt-2">
            <h1 class="text-3xl font-bold">
                Product Detail {{ $product->name }}
            </h1>
        </div>
    </div>
    <div class="bg-white p-5 rounded shadow-sm">
        <div class="relative overflow-x-auto">
            <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
                <tbody>
                <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 border-gray-200">
                    <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                        Product Name
                    </th>
                    <td class="px-6 py-4">{{ $product->name}}</td>
                </tr>
                <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 border-gray-200">
                    <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                        Product Code
                    </th>
                    <td class="px-6 py-4">{{ $product->code }}</td>
                </tr>
                <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 border-gray-200">
                    <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                        Stock
                    </th>
                    <td class="px-6 py-4">{{ $product->stock }}</td>
                </tr>
                <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 border-gray-200">
                    <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                        Price
                    </th>
                    <td class="px-6 py-4">{{ $product->price }}</td>
                </tr>
                </tbody>
            </table>
        </div>
    </div>
    <a href="{{ route('product.index') }}" class="mt-3 inline-block px-6 py-2.5 bg-gray-200 text-gray-700 font-medium text-xs leading-tight uppercase rounded-full ">back</a>
    <a href="{{ route('product.edit', $product) }}" class="inline-block px-6 py-2.5 bg-blue-400 text-white font-medium text-xs leading-tight uppercase rounded-full" id="edit-product-btn">Edit Product</a>
</div>
</body>
</html>
```

Save kembali `resources/views/products/show.blade.php`.

-----

## Step 8: Coding Fitur Update Data

Buka kembali `app/Http/Controllers/ProductController.php`. Selanjutnya kita modifikasi method `edit()` untuk menampilkan form edit data produk.

```php
public function edit(Product $product)
{
    return view('products.edit',compact('product'));
}
```

Setelah selesai, selanjutnya kita buat file view `resources/views/products/edit.blade.php`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>EDIT Product - Tutorial CRUD Laravel 12 </title>
    <script src="https://unpkg.com/@tailwindcss/browser@4"></script>
</head>
<body>
<div class="container mx-auto mt-10 mb-10 px-10">
    <div class="grid grid-cols-8 gap-4 p-5">
        <div class="col-span-4 mt-2">
            <h1 class="text-3xl font-bold">
                EDIT PRODUCT
            </h1>
        </div>
    </div>
    <div class="bg-white p-5 rounded shadow-sm">
        <form action="{{ route('product.update', $product) }}" method="POST">
            @csrf
            @method('PUT')

            <div class="mb-5">
                <label for="name">Name</label>
                <input type="text" class="form-control block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded-full transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" name="name" value="{{ old('name', $product->name) }}" required>
                @error('name')
                <div class="bg-red-400 p-2 shadow-sm rounded mt-2">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-5">
                <label for="code">Code</label>
                <input type="text" class="form-control block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded-full transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" name="code" value="{{ old('code', $product->code) }}" readonly>
                @error('code')
                <div class="bg-red-400 p-2 shadow-sm rounded mt-2">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-5">
                <label for="price">Price</label>
                <input type="text" class="form-control block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded-full transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" name="price" value="{{ old('price', $product->price) }}" required>
                @error('price')
                <div class="bg-red-400 p-2 shadow-sm rounded mt-2">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-5">
                <label for="stock">Stock</label>
                <input type="text" class="form-control block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded-full transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" name="stock" value="{{ old('stock', $product->stock) }}" required>
                @error('stock')
                <div class="bg-red-400 p-2 shadow-sm rounded mt-2">{{ $message }}</div>
                @enderror
            </div>

            <div class="mt-3">
                <button type="submit" class="inline-block px-6 py-2.5 bg-blue-600 text-white font-medium text-xs leading-tight uppercase rounded-full shadow-md hover:bg-blue-700 hover:shadow-lg focus:bg-blue-700 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 active:shadow-lg transition duration-150 ease-in-out">
                    Save Changes
                </button>
                <a href="{{ route('product.index') }}" class="inline-block px-6 py-2.5 bg-gray-200 text-gray-700 font-medium text-xs leading-tight uppercase rounded-full shadow-md hover:bg-gray-300 hover:shadow-lg focus:bg-gray-300 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-gray-400 active:shadow-lg transition duration-150 ease-in-out">back</a>
            </div>
        </form>
    </div>
</div>
</body>
</html>
```

Save kembali file `resources/views/products/edit.blade.php`. Pada baris kode di atas, proses update data diarahkan ke route `post.update` yang mengarah ke method `update()`. Sekarang kita lengkapi method `update()` yang menangani proses update data produk.

```php
public function update(Request $request, Product $product)
{
    $request->validate([
        'name' => 'required',
        'price' => 'required',
        'stock' => 'required',
    ]);
    
    $product->update([
        'name' => $request->name,
        'price' => $request->price,
        'stock' => $request->stock,
    ]);

    return redirect()->route('product.index')
        ->with('success','Product updated successfully');
}
```

Save kembali file `app/Http/Controllers/ProductController.php`.

-----

## Step 9: Coding Fitur Delete Data

Sekarang kita coding fitur terakhir dari operasi CRUD, yaitu delete data produk. Untuk menambahkan fitur delete data, kita buka kembali file controller. Pada class `ProductController`, kita modifikasi method `destroy()` yang menangani proses delete data.

```php
public function destroy(Product $product)
{
    $product->delete();
    
    return redirect()->route('product.index')
        ->with('success','Product deleted successfully');
}
```

Save kembali file `app/Http/Controllers/ProductController.php`.

-----

## Step 10: Uji Coba

Setelah menyelesaikan semua langkah coding fitur CRUD untuk aplikasi manajemen produk Laravel 12, saatnya kita menguji aplikasi yang telah kita buat. Berikut ini langkah-langkah untuk melakukan uji coba:

### Jalankan Server Laravel

Pertama, buka terminal dan jalankan server Laravel dengan menjalankan perintah:

```bash
php artisan serve
```

Output:

```
$ php artisan serve

   INFO  Server running on [http://127.0.0.1:8000].  

  Press Ctrl+C to stop the server
```

### Akses Aplikasi di Browser

Buka browser dan akses URL **`http://127.0.0.1:8000/product`** untuk melihat halaman daftar produk. Pada awalnya, halaman akan menampilkan "Data Empty" karena belum ada data produk yang ditambahkan.

### Menambahkan Data Produk Baru

  * Klik tombol "**Add Product**" di bagian kanan atas halaman.
  * Isi form dengan data produk contoh:
      * **Name**: Laptop ASUS ROG
      * **Code**: LP001
      * **Price**: 15000000
      * **Stock**: 10
  * Klik tombol "**Save**" untuk menyimpan data.
  * Sistem akan menampilkan halaman daftar produk dengan notifikasi "*Product created successfully*".

### Melihat Detail Produk

  * Pada halaman daftar produk, klik tombol "**View**" pada produk yang ingin dilihat.
  * Sistem akan menampilkan halaman detail produk dengan informasi lengkap.

### Mengedit Data Produk

  * Dari halaman daftar produk, klik tombol "**Edit**" pada produk yang ingin diubah.
  * Sistem akan menampilkan form edit dengan data produk yang telah ada.
  * Ubah data yang diinginkan, misalnya:
      * **Name**: Laptop ASUS ROG Strix
      * **Price**: 16500000
      * **Stock**: 8
  * Klik tombol "**Save Changes**" untuk menyimpan perubahan.
  * Sistem akan menampilkan halaman daftar produk dengan notifikasi "*Product updated successfully*".

### Menghapus Data Produk

  * Dari halaman daftar produk, klik tombol "**Delete**" pada produk yang ingin dihapus.
  * Sistem akan menampilkan konfirmasi "*Apakah Anda Yakin?*".
  * Klik "**OK**" untuk mengkonfirmasi penghapusan.
  * Sistem akan menampilkan halaman daftar produk dengan notifikasi "*Product deleted successfully*".

### Validasi Input

Anda juga dapat menguji validasi input dengan mencoba:

  * Menambahkan produk dengan kode yang sudah ada (untuk menguji validasi `unique` pada code).
  * Mengirimkan form dengan field kosong (untuk menguji validasi `required`).

### Pagination

Tambahkan lebih dari 10 produk untuk melihat fitur pagination bekerja, karena kita telah mengatur jumlah produk per halaman sebanyak 10 item.

### Memastikan Semua Fitur Berfungsi

Uji semua fitur CRUD untuk memastikan aplikasi berjalan dengan baik:

  * **Create**: Tambahkan beberapa produk baru.
  * **Read**: Lihat daftar produk dan detail produk.
  * **Update**: Edit beberapa produk yang sudah ada.
  * **Delete**: Hapus produk yang tidak diperlukan.

Dengan melakukan uji coba ini, Anda dapat memastikan bahwa aplikasi manajemen produk Laravel 12 yang Anda buat telah berfungsi dengan baik dan siap digunakan.

-----

## Penutup

Selamat\! Anda telah berhasil menyelesaikan tutorial CRUD (Create, Read, Update, Delete) dengan Laravel 12. Mari kita rangkum apa yang telah kita pelajari dan capai melalui tutorial ini:

  * **Persiapan Awal**: Kita memulai dengan membuat project Laravel baru dan mengonfigurasi database untuk aplikasi.
  * **Pemodelan Data**: Kita membuat model `Product` beserta migrasi database yang diperlukan untuk menyimpan informasi seperti kode, nama, harga, dan stok produk.
  * **Implementasi Controller**: Kita mengembangkan `ProductController` dengan metode resource untuk menangani seluruh operasi CRUD.
  * **Pembuatan View**: Kita membuat beberapa halaman view menggunakan Blade template engine dan Tailwind CSS untuk antarmuka yang responsif dan modern.
  * **Pengembangan Fitur CRUD**:
      * **Create**: Form untuk menambahkan produk baru dengan validasi input.
      * **Read**: Tampilan daftar produk dengan pagination dan halaman detail produk.
      * **Update**: Form untuk mengedit data produk yang sudah ada.
      * **Delete**: Fungsionalitas untuk menghapus data dengan konfirmasi.

Dengan tutorial ini, Anda tidak hanya mempelajari dasar-dasar CRUD dengan Laravel 12, tetapi juga praktik-praktik terbaik dalam pengembangan aplikasi web modern, termasuk validasi data, pengelolaan route, dan pembuatan antarmuka yang responsif.

### Langkah Selanjutnya

Untuk mengembangkan aplikasi ini lebih lanjut, Anda dapat mencoba:

  * Menambahkan sistem autentikasi pengguna.
  * Mengimplementasikan fitur pencarian dan filter produk.
  * Mengelola upload gambar produk.
  * Membuat REST API untuk aplikasi mobile.
  * Menambahkan unit testing untuk memastikan kualitas kode.

### Kesimpulan

Laravel 12 menawarkan berbagai peningkatan yang memudahkan pengembangan aplikasi web yang kuat dan skalabel. Dengan memahami konsep dasar CRUD, Anda telah membangun fondasi yang kokoh untuk mengembangkan aplikasi yang lebih kompleks di masa depan.

Semoga tutorial ini bermanfaat bagi Anda dalam memahami dan menerapkan Laravel 12 untuk kebutuhan pengembangan web Anda. Selamat berkarya\!
