# todolist
Proje Kurulumu

Projeyi klonlayarak ya da zip olarak indirin.

Terminalde ```composer install``` komutunu çalıştırın.

```.env.example``` dosyasını kopyalayarak ```.env``` olarak kaydedin.

```.env``` dosyasının içeriğini veritabanı bilgilerinize göre düzeltin.

Ardından aşağıdaki komutları terminalde sırasıyla çalıştırın.

```php artisan key:generate```

```php artisan jwt:secret```

```php artisan migrate```

```npm install```

Projeyi çalıştırmak için ilk terminale ```php artisan serve``` ikinci terminale ```npm run watch``` komutunu çalıştırarak projeyi açabilirsiniz.

Demo : http://todolist.yazilimduragi.com/
