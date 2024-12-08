
1. admin -> choco install mingw
2. 安裝 lazyvim
3. 安裝 nvim 設定環境變數 PATH
4. `Test-Path $PROFILE`
5. - `New-Item -ItemType File -Path $PROFILE -Force`
6. `notepad $PROFILE`
7. `Set-Alias vim nvim`
8.  `. $PROFILE`
9. `Get-Alias vim`
10. `Write-Output $PROFILE`
11.  `$PROFILE`
12. git config --global http.sslVerify false
13. `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`
14. - `Get-Content ~/.ssh/id_rsa.pub`
15. `ssh -T git@github.com`
16. git clone https://github.com/LazyVim/starter $env:LOCALAPPDATA\nvim 
17. `Test-Path "C:\Users\1409021\AppData\Local\nvim\lua"`
18. - `Remove-Item -Path "C:\Users\1409021\AppData\Local\nvim\lua" -Recurse -Force`
19. `Move-Item -Path .\lazyvim-configs -Destination "C:\Users\1409021\AppData\Local\nvim\lua"`
20. Install-Module posh-git -Scope CurrentUser
21. Install-Module oh-my-posh -Scope CurrentUser
22. notepad $PROFILE
```
Import-Module posh-git
Import-Module oh-my-posh
oh-my-posh init pwsh | Invoke-Expression
```

或用 ALIAS

Set-Alias mv Move-Item
Set-Alias cp Copy-Item
Set-Alias rm Remove-Item 
Set-Alias ls Get-ChildItem 
Set-Alias cat Get-Content
Set-Alias pwd Get-Location
$env:NODE_TLS_REJECT_UNAUTHORIZED = "0"

`pyinstaller --onefile --noconsole audio_client_app.py`