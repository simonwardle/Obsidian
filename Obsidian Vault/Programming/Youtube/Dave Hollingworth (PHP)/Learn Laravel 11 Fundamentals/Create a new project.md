#laravel

After you have installed PHP and Composer, you may create a new Laravel project via Composer's `create-project` command:

```
composer create-project laravel/laravel example-app
```

Or, you may create new Laravel projects by globally installing the Laravel Installer via Composer. The Laravel installer allows you to select your preferred testing framework, database, and starter kit when creating new applications:

```
composer global require laravel/installer

laravel new example-app
```

To get this working I found I also had to change my .profile.
I added the following line.

```
export PATH="$HOME/.config/composer/vendor/bin:$PATH"

```

