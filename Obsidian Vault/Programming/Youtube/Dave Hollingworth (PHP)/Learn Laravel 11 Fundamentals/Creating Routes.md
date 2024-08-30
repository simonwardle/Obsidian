#laravel 
Routes are created in the routes folder using the web.php file.

Here is a very simple example:

```
<?php
use Illuminate\Support\Facades\Route;

Route::get('/', function () {
return view('home');
});

Route::get('/about', function () {
return view('about');
});
```

For the above to work you would also need 2 blade templates.
home.blade.php and about.blade.php these would need to be held in the resources/views folder.

## Naming a Route
To name a route simple add the name to the end.
```
Route::get('/', function () {
return view('home');
})->name('home');
```

Then to use the named route it is as easy as this..
```
<ul>
	<li><a href="{{ route('home')}}">Home</a></li>
	<li><a href="{{ route('about')}}">About</a></li>
</ul>
```

In the above blade layout file we are using echo statements {{}}