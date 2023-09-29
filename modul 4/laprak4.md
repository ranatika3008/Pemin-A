# Basic Routing dan Migration

## Get
Untuk menambahkan endpoint dengan method GET pada aplikasi kita, kita dapat mengunjungi file web.php pada folder routes. Kemudian tambahkan baris ini pada akhir file

```
...
$router->get('/get', function () {
    return 'GET';
});
```
Setelah itu coba jalankan aplikasi dengan command,
```
php -S localhost:8000 -t public
```
![ss 1.1](../modul%204/ss%204/1.1.png)
```
⚠ Note: Pastikan buka cmd pada folder aplikasi
```
Setelah aplikasi berhasil dijalankan, kita dapat membuka browser dengan url, `http://localhost:8000/get` , path yang akan kita akses akan berbentuk demikian, `http://{BASE_URL}{PATH}`, jika BASE_URL kita adalah `localhost:8000` dan PATH kita adalah `/get` , maka url akan berbentuk seperti diatas.

## POST, PUT, PATCH, DELETE, dan OPTIONS
Sama halnya saat menambahkan method GET, kita dapat menambahkan methode POST, PUT, PATCH, DELETE, dan OPTIONS pada file web.php dengan code seperti ini,
```
...
$router->post('/post', function () {
    return 'POST';
});
$router->put('/put', function () {
    return 'PUT';
});
$router->patch('/patch', function () {
    return 'PATCH';
});
$router->delete('/delete', function () {
    return 'DELETE';
});
$router->options('/options', function () {
    return 'OPTIONS';
});
```
![ss 1.2](../modul%204/ss%204/1.2.png) <br>
Setelah selesai menambahkan route untuk method POST, PUT, PATCH, DELETE, dan OPTIONS, kita dapat menjalankan server seperti pada saat percobaan GET. Setelah server berhasil menyala, kita dapat membuka aplikasi Postman atau Insomnia atau kita juga dapat menggunakan PowerShell (Windows) / Terminal (Linux atau Mac) untuk melakukan request ke server. Namun, pada percobaan kali ini kita akan menggunakan extensions pada VSCode yaitu Thunder Client. 
1. Kita dapat menginstall ekstensi dengan membuka panel extensions lalu mencari thunder client <br>
![ss 1.3](../modul%204/ss%204/1.3.png)
2. Setelah menginstall Thunder Client, kita akan melihat logo seperti petir pada activity bar kita (sebelah kiri).
![ss 1.4](../modul%204/ss%204/1.4.png)
3. Kita dapat membuat request dengan menekan `New Request` pada ekstensi
4. Setelah itu kita dapat memasukkan method dan url yang dituju 
5. Akses url yang baru saja ditambahkan pada aplikasi dengan methodnya<br>
![ss 1.5](../modul%204/ss%204/1.5.png) <br>
![ss 1.6](../modul%204/ss%204/1.6.png) <br>
![ss 1.7](../modul%204/ss%204/1.7.png) <br>
![ss 1.8](../modul%204/ss%204/1.8.png) <br>
![ss 1.9](../modul%204/ss%204/1.9.png)

## Migrasi Database
1. Sebelum melakukan migrasi database pastikan server database aktif kemudian pastikan sudah membuat database dengan nama `lumenapi` <br>
![ss 1.11](../modul%204/ss%204/1.11.png)
2. Kemudian ubah konfigurasi database pada file .env menjadi seperti ini

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=lumenapi
DB_USERNAME=root
DB_PASSWORD=<<password masing-masing>>
```
![ss 1.10](../modul%204/ss%204/1.10.png)
3. Setelah mengubah konfigurasi pada file .env, kita juga perlu menghidupkan beberapa library bawaan dari lumen dengan membuka file app.php pada folder bootstrap dan mengubah baris ini,
```
//$app->withFacades();
//$app->withEloquent();
```
Menjadi,
```
$app->withFacades();
$app->withEloquent();
```
![ss 1.12](../modul%204/ss%204/1.12.png)
4. Setelah itu jalankan command berikut untuk membuat file migration, 
```
php artisan make:migration create_users_table # membuat migrasi untuk tabel users 
php artisan make:migration create_products_table # membuat migrasi untuk tabel products
```
![ss 1.13](../modul%204/ss%204/1.13.png) <br>
Setelah menjalankan 2 syntax diatas akan terbuat 2 file pada folder `database/migrations` dengan format YYYY_MM_DD_HHmmss_nama_migrasi. Pada file migrasi kita akan menemukan fungsi up() dan fungsi down(), fungsi up() akan digunakan pada saat kita melakukan migrasi, fungsi down() akan digunakan saat kita ingin me-rollback migrasi
5. Ubah fungsi up pada file migrasi `create_users_table`
```
# sebelumnya
...
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }
...
# diubah menjadi
...
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
            $table->string('name');
            $table->string('email');
            $table->string('password');
        });
    }
...
```
![ss 1.15](../modul%204/ss%204/1.15.png)
6. Ubah fungsi up pada file migrasi create_products_table
```
# sebelumnya
...
    public function up()
    {
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }
...
# diubah menjadi
...
    public function up()
    {
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
            $table->string('name');
            $table->integer('category_id');
            $table->string('slug');
            $table->integer('price');
            $table->integer('weight');
            $table->text('description');
        });
    }
...
```
![ss 1.6](../modul%204/ss%204/1.6.png)
7. Kemudian jalankan command,
```
php artisan migrate
```
![ss 1.7](../modul%204/ss%204/1.7.png)