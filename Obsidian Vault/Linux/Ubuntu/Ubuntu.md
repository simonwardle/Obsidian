## Environment Variables
Setting up environment variable.  It looks like we sometimes need to edit .bashrc and then other times .profile??  I really don't understand why.

The .profile and .bashrc files are run at login [[Login scripts]].

## Printing
My current printer is the Epson XP-235 to get this to work on Ubuntu I had to download a [[Printer Driver]] 

Once you have the driver downloaded run the following command.

```
sudo dpkg -i epson-inkjet-printer-escpr_1.7.26-1lsb3.2_amd64.deb
```

Finally connect the printer and try it.

This worked on my surface book and my ThinkPad P50

## Dock 

To move the start button from the right side to the left try this.

```
gsettings set org.gnome.shell.extensions.dash-to-dock show-apps-at-top true
```

![[Pasted image 20240923192821.png]]
