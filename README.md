##Perkenalan

Kerangka kerja PHP mini yang dibangun dari awal mengikuti standar PSR terbaru.

Kerangka kerja mencakup konsep-konsep seperti:

### Routing

`routes.php`

Anda dapat membuat GET, POST, PUT, DELETE, atau HTTP route apa pun.

#contoh penggunaan http route
// GET
$app->get('users', ['UsersController', 'index']);

// POST
$app->post('users', ['UsersController', 'store']);
```

### Responses

Anda dapat mengembalikan tampilan HTML, JSON, atau jenis konten lainnya baik dari Controllers atau dengan menggunakan fungsi anonim.

```php
# return html view
$app->get('/', ['PagesController', 'home']);

# return json
$app->get('users', ['UsersController', 'index']);

# return plain text
$app->get('status', function ($app) {
	return '<pre> I\'m OK </pre>';
});
```

### Container

File wadah aplikasi terletak di `app/Core/Container.php`.

Anda dapat mengikat item ke aplikasi dalam `bootstrap/app.php` menggunakan sintaks larik berikut.

```php
$app->bind('databaseConnection', [new App\Core\Database\Connection, 'make']);
```
Item pertama dalam array adalah instance dari kelas yang ingin Anda ikat dan yang kedua adalah metode untuk dipanggil.

Dan untuk menyelesaikan item dari wadah:

```php
$db = $app->databaseConnection;
```

### Dependency Injection

Periksa `app/Core/App.php` dan `app/Core/Database/Connection.php` untuk memahami bahwa objek container secara otomatis dimasukkan ke dalam bindingnya. Di sini pengikatannya adalah `databaseConnection`.

```php
class Connection
{
	/*
	 * App\Core\Container $container is injected 
	 * automatically by $app (bind)
	 */
    public function make($container)
    {
    	//
    }
}
```

Kode berikut dalam `app/Core/Database/QueryBuilder.php` menunjukkan bahwa kita memasukkan dependensi PDO ke dalam kelas `Connection`.

```php
public function __construct(PDO $connection)
{
    $this->connection = $connection;
}
```

### Closures

Terlepas dari sintaks array, Anda juga dapat mengikat item ke aplikasi dengan meneruskan penutupan sebagai argumen kedua, dalam `bootstrap/app.php`

```php
$app->bind('database', function ($app) {
	return new App\Core\Database\QueryBuilder($app->databaseConnection);
});
```

###Lainnya
Kerangka kerja ini mencakup banyak konsep lain seperti MVC, pola pengontrol depan, koleksi, model, file konfigurasi khusus, dll.

## Installasi

Cukup klon dan instal dengan:

`composer install`

Dan boot server dengan menjalankan:

`php -S localhost:8080 -t public/`


