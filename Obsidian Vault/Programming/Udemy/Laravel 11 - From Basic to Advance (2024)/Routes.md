#laravel #Routes

Routes are created in the routes folder using the web.php file.

Here is a very simple example:

```
<?php
use Illuminate\Support\Facades\Route;

Route::get('/', function () {
return view('home');
});

Route::get('/about', function () {
return "This is the About Page";
});
```

For the above to work you would also need a blade template home.blade.php this would need to be held in the resources/views folder.  The /about route will simply display your string.

The above is called a closure function.  The closure function is also known as the action see below screen shot.

![[Pasted image 20240930143956.png]]

Route:: is basically a php class with various methods or functions implemented such as get() and post().


## Passing Parameters
Below is an example of passing a parameter (id) to our /user route.

```
Route::get('/user/{id}', function () {
return "Hello user";
});
```

To use the above route in the browser simply browse to localhost:8000/user/1 where 1 could be anything.

To get the parameter that was passed in the url, very simply just change to the following.
```
Route::get('/user/{id}', function ($id) {
return "Hello user " . $id;
});
```
The result would be "Hello user 1".


### Passing multiple parameters
Passing more than one.
```
Route::get('/user/{id}/{slug}', function ($id, $slug) {
return "Hello user " . $id . '-' . $slug;
});
```
The above would return something like this...
![[Pasted image 20240930170435.png]]


## Naming a Route
To name a route simple add the name to the end.
```
Route::get('/', function () {
return view('home');
})->name('home');

Route::get('/about', function () {
return view('about');
})->name('about');

Route::get('/user/{id}/{slug}', function ($id, $slug) {
return "Hello user " . $id . '-' . $slug;
})->name('user');
```

Then to use the named route it is as easy as this..
```
<ul>
	<li><a href="{{ route('home')}}">Home</a></li>
	<li><a href="{{ route('about')}}">About</a></li>
	<li><a href="{{ route('user', ['id' => 1, 'slug' => '100'])}}">User</a></li>
</ul>
```

In the above blade layout file we are using echo statements {{  }} notice the function route() accepts an array.