There is a paid for option for syncing but it is about $5 a month. So at the amount I will use Git.

I tried to just put the vault in google drive but I just couldn't read the files via obsidian.

I now do this.

Make any edits in obsidian then when I want to sync it I use git with vscode.

Via terminal 

```
cd Documents
code Obsidian
```

Enter a message then click Commit.
![[Pasted image 20240830194510.png]]

![[Pasted image 20240830194826.png]]

This will push changes to github.

Then on your other laptop open code in the same way and sync the changes.  This will pull your changes to the new device.

Found workspace.json gets in a knot when trying to use git.  So added to .gitignore like this.




