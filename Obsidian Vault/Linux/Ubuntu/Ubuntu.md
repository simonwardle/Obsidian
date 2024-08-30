## Environment Variables
Setting up environment variable.  It looks like we sometimes need to edit .bashrc and then other times .profile??  I really don't understand why.

The .profile and .bashrc files are run at login [[Login scripts]].

## Printing
My current printer is the Epson XP-235 to get this to work on Ubuntu I had to download a [[Printer Driver]] 

Once you have the driver installed run the following command.

```
sudo apt-get install lsb
```

Then run this command to install the driver you downloaded in step one.
```
sudo dpkg -i epson-inkjet-printer-escpr_1.7.26-1lsb3.2_amd64.deb
```

Finally connect the printer and try it.

This worked on my surface book and my ThinkPad P50