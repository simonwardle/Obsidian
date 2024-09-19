
To check if you have node installed.

In the terminal
```
node -v
```

You should get the version number or a message saying it is not installed.

To install 

```
sudo apt install nodejs
```

Then you will need to install npm.

```
sudo apt install npm
```

If you want to completely remove node and npm.

```
sudo apt-get remove nodejs
sudo apt-get remove npm
sudo apt autoremove
```

Now remove .node and .npm from your home directory.

```
cd ~
rm -rf .npm
```

Next run these (I found none of these existed)
```
sudo rm -rf /usr/local/bin/npm 
sudo rm -rf /usr/local/share/man/man1/node* 
sudo rm -rf /usr/local/lib/dtrace/node.d 
sudo rm -rf ~/.npm 
sudo rm -rf ~/.node-gyp 
sudo rm -rf /opt/local/bin/node 
sudo rm -rf opt/local/include/node 
sudo rm -rf /opt/local/lib/node_modules  

sudo rm -rf /usr/local/lib/node*
sudo rm -rf /usr/local/include/node*
sudo rm -rf /usr/local/bin/node*
```
