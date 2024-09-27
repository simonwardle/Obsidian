#laravel 

If you clone a Git repository follow these steps.

Open the folder in VScode and then run..

```
composer install
composer update
cp .env.example .env
php artisan key:generate
php artisan migrate
```

The above will install all the Laravel packages.
The .env file is not in source control.  (need to look at encrypting it)
The database will not exist the above will make it.

Finally run...
```
php artisan serve
```

Go to localhost:8000

Done