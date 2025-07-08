## Found this answer on the web

`~/.profile`is only called when you first log into your account. Any changes you make after that it would be wise to log out and back in so settings take effect. `~/.bashrc` is called every time you launch a terminal window. There is another `profile` file and it is in the `/etc/` directory. The main difference between the two is that the `/etc/profile` is called when anyone logs into the system, and the `~/.profile` is called when only the user logs in.

If your `export` lines are only used in a terminal session then I would add them to the `~/.bashrc` file as they are only valid during the terminal (bash) session. But, if you want them to be there with or without a terminal open, add them to the `~/.profile` file, but do not put bash specific commands in the `~/.profile` file.

If you screw up these files, there are default files stored in the `/etc/skel/` directory that you can copy back over to your home directory. Also, the `/etc/skel/` files are also used when you boot to a LiveUSB/CD/DVD, so if you modify those on your live media and then when it completes the boot you can have your own variables set.