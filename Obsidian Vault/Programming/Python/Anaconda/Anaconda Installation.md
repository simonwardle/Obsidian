Website for installation instructions
https://www.anaconda.com/docs/getting-started/anaconda/install#linux-installer

Basically run these commands
```
curl -O https://repo.anaconda.com/archive/Anaconda3-2024.10-1-Linux-x86_64.sh
```

This will pull the installation script into the current directory, then run this command.

```
bash ~/Anaconda3-2024.10-1-Linux-x86_64.sh
```

It will ask you to view terms & conditions hit space bar to scroll through them then enter yes.
Once it has finished you will be asked the following, it's best to say yes.

![[Pasted image 20250614192812.png]]

Then to update the current shell enter this command

```
source ~/.bashrc
```

Then this to check installation.

```
conda --version
```
