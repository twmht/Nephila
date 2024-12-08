
This guide aims to configure PowerShell to mimic Linux functionality, including installing essential tools and setting up aliases.

---

## 1. Install MinGW
```powershell
admin -> choco install mingw
```

## 2. Install LazyVim
1. Install NeoVim and set environment variable:
   ```powershell
   # Add NeoVim to PATH
   $env:Path += ";C:\path_to_nvim"
   ```

2. Check PowerShell Profile Path:
   ```powershell
   Test-Path $PROFILE
   ```
   If it doesn't exist, create it:
   ```powershell
   New-Item -ItemType File -Path $PROFILE -Force
   ```

3. Edit Profile to Alias NeoVim:
   ```powershell
   notepad $PROFILE
   # Add the following line
   Set-Alias vim nvim
   ```
   Reload Profile:
   ```powershell
   . $PROFILE
   Get-Alias vim
   Write-Output $PROFILE
   ```

## 3. Configure Git
1. Disable SSL Verification (for testing purposes only):
   ```powershell
   git config --global http.sslVerify false
   ```

2. Generate SSH Key:
   ```powershell
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```
   Output the public key:
   ```powershell
   Get-Content ~/.ssh/id_rsa.pub
   ```

3. Test SSH Connection:
   ```powershell
   ssh -T git@github.com
   ```

4. Clone LazyVim:
   ```powershell
   git clone https://github.com/LazyVim/starter $env:LOCALAPPDATA\nvim
   ```

## 4. LazyVim Configuration
1. Check if the Lua directory exists:
   ```powershell
   Test-Path "C:\Users\<YourUsername>\AppData\Local\nvim\lua"
   ```
2. If it exists, remove it:
   ```powershell
   Remove-Item -Path "C:\Users\<YourUsername>\AppData\Local\nvim\lua" -Recurse -Force
   ```
3. Move your configuration files:
   ```powershell
   Move-Item -Path .\lazyvim-configs -Destination "C:\Users\<YourUsername>\AppData\Local\nvim\lua"
   ```

## 5. Install and Configure Modules
1. Install `posh-git`:
   ```powershell
   Install-Module posh-git -Scope CurrentUser
   ```

2. Install `oh-my-posh`:
   ```powershell
   Install-Module oh-my-posh -Scope CurrentUser
   ```

3. Add to Profile:
   ```powershell
   notepad $PROFILE
   # Add the following lines
   Import-Module posh-git
   Import-Module oh-my-posh
   oh-my-posh init pwsh | Invoke-Expression
   ```

## 6. Alias Configuration
```powershell
Set-Alias mv Move-Item
Set-Alias cp Copy-Item
Set-Alias rm Remove-Item
Set-Alias ls Get-ChildItem
Set-Alias cat Get-Content
Set-Alias pwd Get-Location
```

## 7. NodeJS Configuration
Disable strict TLS for development:
```powershell
$env:NODE_TLS_REJECT_UNAUTHORIZED = "0"
```

## 8. PyInstaller Example
Create a standalone Python executable:
```powershell
pyinstaller --onefile --noconsole audio_client_app.py
```

---

With these configurations, PowerShell will operate more like a Linux terminal, improving productivity for users familiar with Linux workflows.

