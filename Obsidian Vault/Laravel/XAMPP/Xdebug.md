#vscode #php #debug 

To debug php you need to install xdebug.
This has proved tricky I found this site which gives instructions for windows which kind of helped.

Basically follow the instructions here...

https://medium.com/@asd66998854/php-code-debug-using-xampp-on-vscode-editor-97c5f6cc4487

Then when it redirects to the Xdebug installation wizard follow the step it gives you after it has analysed your phpinfo() output.

![[Pasted image 20240927155302.png]]

## VS Code
Under extensions search for php debug, there will be a few to choose from.  This one just worked...

![[Pasted image 20240927164550.png]]

To test created a folder in /opt/lampp/htdocs...
![[Pasted image 20240927164804.png]]

Created a test php file.
![[Pasted image 20240927165051.png]]


I ran this command to make me the owner...
```
sudo chown simon debug debug/testXdebug.php 
```


In VS Code set a breakpoint and then click the debug icon then Run and Debug.
![[Pasted image 20240927165433.png]]

The debugger will stop at your breakpoint.
![[Pasted image 20240927165608.png]]

Just need to make it work with Laravel next.

It just works with Laravel, run php artisan serve, then add break points and start the debugger as above and it just works.