2. Конфигурирование php-fpm

Разверните docker-контейнер php-fpm 7.4 и отправьте на него трафик с Nginx. Используйте приложенный php-файл как пример открываемой страницы.

[app1.php](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9ab24d10-5c57-4fff-9797-95d7d4935664/app1.php)

<hr>

<ins>Comments</ins>:

- start both `Nginx` and `php-fpm` by running `docker compose`:

`docker compose -f php_compose.yml up -d`
