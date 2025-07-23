To install.

Ubuntu 22.04 X86_64
Add gpd key
```
wget -qO - https://deb.tableplus.com/apt.tableplus.com.gpg.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/tableplus-archive.gpg > /dev/null
```

Add repo

```
sudo add-apt-repository "deb [arch=amd64] https://deb.tableplus.com/debian/22 tableplus main"
```

Install

```
sudo apt update
sudo apt install tableplus
```

To run look for this...

![[Pasted image 20240829195900.png|200]]

