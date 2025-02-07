有時候把 ssh public key 上傳到 git 後仍會出現無法 push 的情況，這時候可以用 ssh-add 把 private key 加進去，這個只在當前的 shell 環境有效

1. 如果你的系統未啟動 SSH Agent，Git 無法從中獲取私鑰，導致身份驗證失敗。

```bash
啟動 SSH Agent 的方法：
`eval "$(ssh-agent -s)"`
```
2. ssh add
```bash
ssh-add ~/.ssh/id_rsa
ssh-add -l 
```
3. ssh-add -l
```
ssh-add -l 
```
4. 連線到 github 確認可以
```bash

```

## git checkout 

`git submodule update --init --recursive`