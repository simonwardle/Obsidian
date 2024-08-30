#laravel 


## List

This command has many options to get a list of the options enter.

```
php artisan list
```


## Views

To create a view 
```
php artisan make:view hello
```

This will create a hello.blade.php file in the correct folder.

For help proceed the command with the work help.

```
php artisan help make:view
```

![[Pasted image 20240723135850.png]]

To create a view in a sub folder use a .
```
php artisan make:view products.index
```

This would give you this..
![[Pasted image 20240723144143.png]]

## Migrations
#mysql

To create a database table you first need to edit the .env to point to the Db server.
Here is an example:
![[Pasted image 20240723171815.png]]
Then to create the first table.
```
php artisan make:migration create_product_table
```

This command creates a file in the database/migrations folder.  The naming convention allows Laravel to guess the table name.

![[Pasted image 20240723172516.png]]


Populate the Schema section with your required columns:

![[Pasted image 20240723173010.png]]

To run the migration, which will create the Db table product for you.

```
php artisan migrate
```

The first time you run the above command it should detect that the Db Laravel doesn't exist and ask if you want to create it.
![[Pasted image 20240723174035.png]]It should then just run the migration creating your table plus some tables required by Laravel.
![[Pasted image 20240723174222.png]]
![[Pasted image 20240723174621.png]]


To rollback the most recent migration 
```
php artisan migrate:rollback
```

### Refresh Database
If you want to start a fresh version of your database dropping all your data then enter this command.  Note be careful it will lose all your data and give you  a blank database with just your tables.
  
```
php artisan migrate:fresh
```


## Components

To make a component named layout.

```
php artisan make:component layout --view
```

The above command creates this.
![[Pasted image 20240724142934.png]]
Here is simple example code to place in the layout file.
![[Pasted image 20240724143559.png]]

$slot is where the content of your other blade would slot in.

Here is an example home page utilising the layout component above.
![[Pasted image 20240724144015.png]]
Whatever is inside the layout tags will be passed to the layout blade element into the slot position.

## Routes
The routes are defined in the web.php file and look like this..
![[Pasted image 20240725151053.png]]

To view a list of all routes from the command line use this..
```
php artisan route:list
```


