#laravel 

To run Laravel you first need to install php and apache see XAMPP

If you have a version of Laravel already installed and you just want to delete it and start again with a newer version this is the what I did.

Removed old version of Laravel

```
composer global remove laravel/installer
```


Then remove composer if that is an old version.

```
sudo apt-get remove composer -y
```

Install latest version of composer..

```
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/bin --filename=composer
```

To create a new Laravel project change to your desired directory and run..

```
composer create-project laravel/laravel [project_name]
```

Then I open VS Code with..

```
code [project_name]
```

In the vscode terminal run.

```
php artisan serve
```

Then start a browser http://localhost:8000/

The latest version is in use...

![[Pasted image 20240828192241.png]]

If you then run...
```
composer global require laravel/installer
```

You will then be able to create a project using..

```
laravel new [project_name]
```

The installer runs a kind of wizard.  Once complete you should see this..

![[Pasted image 20240828192710.png]]