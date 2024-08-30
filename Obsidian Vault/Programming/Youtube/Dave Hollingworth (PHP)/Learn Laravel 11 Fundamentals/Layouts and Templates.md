#laravel 

![[Pasted image 20240722163014.png]]
Two dynamic parts.

	1. title
	2. content

The about page will simply insert the words 'About us' into the layout at the correct yield point.
When using HTML in a @section you need to also provide an @endsection.

The above code is converted into this at run time...

![[Pasted image 20240722163831.png]]

The final version of the rendered template is sent as a response to the client.

The @yield('title') is know as a blade directive.

