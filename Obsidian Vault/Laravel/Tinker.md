
If you try to use tinker you will probably find up arrow doesn't work.

Install this wrapper.

https://community.linuxmint.com/software/view/rlwrap

To start tinker...

```
rlwrap php artisan tinker
```

You should find the arrow keys work as expected.

To make it easier to run setup an alias for this command in the .bashrc.

```
alias tinker='rlwrap php artisan tinker'
```

Then to run just enter tinker and it should just work.

Mainly used to run factories.

App\\Models\\Job::factory(10)->create();  would create 10 jobs in the DB
