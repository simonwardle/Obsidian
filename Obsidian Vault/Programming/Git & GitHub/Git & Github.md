## Setting up ssh

First do you have an SSH key setup already?
```
ls -la ~/.ssh
```
If you have files in this folder something like this...

**id_ed25519**

Then you already have ssh at least started.

## Generating a new ssh key

```
ssh-keygen -t ed25519 -C "simon.wardl@outlook.com"
```

This will ask for a file name just hit enter to accept the default.  I didn't enter a passphrase just hit enter.

## Start ssh-agent

```
eval "$(ssh-agent -s)"
```

## Adding SSH private key to ssh-agent

```
ssh-add ~/.ssh/id_ed25519
```

## Add Key to Account

In github you need to add the public key to you account.
![[Pasted image 20241001204234.png|300]]

![[Pasted image 20241001204347.png]]

When you add a new key simply enter a title and then copy the contents of the file ~/.shh/id_e25519.pub into the area provided.

## Cloning a repos

When you clone a repository make sure to select SSH from the drop down menu.

![[Pasted image 20241001204745.png]]

## Basic Git commands

git clone git@github.com:simonwardle/Obsidian.git  - This will clone from github.
git add .   - This adds any new files into the local repository.
git commit -m "Added git instructions"    - This commits all your files locally with a message
git push  - This pushes your changes up to Github.
git pull  - This pull down from Github any new changes into your local repository.

A better option to git pull is 
	git pull --rebase 
	If this fails with merge conflicts do this 
	git rebase --abort 
	Then go back to using git pull and resolve the merge conflicts. 

