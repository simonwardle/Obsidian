```bash
sudo install -d -m 0755 /etc/apt/keyrings
```

```bash
curl -fsSL https://packages.microsoft.com/keys/microsoft.asc | sudo gpg --dearmor -o /etc/apt/keyrings/microsoft.gpg
```

```bash
sudo chmod 0644 /etc/apt/keyrings/microsoft.gpg
```

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list > /dev/null
```

```bash
sudo apt update
```

```bash
sudo apt install code
```
