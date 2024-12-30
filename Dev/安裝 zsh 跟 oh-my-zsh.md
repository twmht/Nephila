以下是如何在 **Ubuntu** 上安裝 **zsh** 並配置 **oh-my-zsh** 的詳細步驟：

---

### **步驟 1：安裝 zsh**

#### **更新系統包管理器**

首先，更新系統的包管理器：

```bash
sudo apt update
sudo apt upgrade -y
```

#### **安裝 zsh**

運行以下命令安裝 zsh：

```bash
sudo apt install zsh -y
```

#### **確認安裝成功**

檢查 zsh 的版本來確認安裝成功：

```bash
zsh --version
```

---

### **步驟 2：設置 zsh 為默認 Shell**

將 zsh 設置為默認 Shell：

```bash
chsh -s $(which zsh)
```

- **說明**：
    - `chsh` 用於更改默認 Shell。
    - `$(which zsh)` 獲取 zsh 的完整路徑。

#### **登出並重新登入**

更改默認 Shell 後，登出並重新登入以啟用 zsh。

---

### **步驟 3：安裝 Oh-My-Zsh**

#### **1. 使用 curl 安裝**

如果系統中已安裝 **curl**，運行以下命令：

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

#### **2. 使用 wget 安裝**

如果沒有 curl，但有 **wget**，使用以下命令：

```bash
sh -c "$(wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

#### **3. 確認安裝成功**

安裝完成後，終端會自動切換到 Oh-My-Zsh 的默認配置界面。如果看到類似以下提示，說明安裝成功：

```bash
Oh My Zsh configured successfully!
```

---

### **步驟 4：自定義 Oh-My-Zsh**

#### **1. 更改主題**

默認主題是 `robbyrussell`，可以在 `~/.zshrc` 中更改主題。

- 編輯 `.zshrc`：
    
    ```bash
    nano ~/.zshrc
    ```
    
- 修改以下行：
    
    ```bash
    ZSH_THEME="agnoster"
    ```
    
- 保存文件並重新加載配置：
    
    ```bash
    source ~/.zshrc
    ```
    

#### **2. 安裝插件**

Oh-My-Zsh 支持多種插件，可以啟用更多功能。

- 編輯 `.zshrc`：
    
    ```bash
    nano ~/.zshrc
    ```
    
- 在 `plugins` 中添加所需插件，例如：
    
    ```bash
    plugins=(git z autosuggestions syntax-highlighting)
    ```
    
- 保存並重新加載配置：
    
    ```bash
    source ~/.zshrc
    ```
    

---

### **步驟 5：安裝實用插件**

#### **1. 安裝 zsh-autosuggestions**

- 克隆插件：
    
    ```bash
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
    ```
    
- 在 `.zshrc` 中啟用插件：
    
    ```bash
    plugins=(zsh-autosuggestions)
    ```
    
- 重新加載配置：
    
    ```bash
    source ~/.zshrc
    ```
    

#### **2. 安裝 zsh-syntax-highlighting**

- 克隆插件：
    
    ```bash
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
    ```
    
- 在 `.zshrc` 中啟用插件：
    
    ```bash
    plugins=(zsh-syntax-highlighting)
    ```
    
- 重新加載配置：
    
    ```bash
    source ~/.zshrc
    ```
    

---

### **步驟 6：測試與使用**

1. 打開新的終端窗口，應該顯示配置好的 Oh-My-Zsh 主題和插件效果。
2. 測試 zsh 的自動補全、高亮和建議功能。

---

### **常見問題**

1. **如果 zsh 啟動時報錯？**
    
    - 確保 `.zshrc` 中的插件路徑正確。
    - 刪除或重置 `.zshrc` 文件，重新安裝 Oh-My-Zsh：
        
        ```bash
        rm ~/.zshrc
        zsh
        ```
        
2. **如何恢復默認 Shell？**
    
    - 使用 `chsh` 將默認 Shell 設置回 bash：
        
        ```bash
        chsh -s $(which bash)
        ```
        

---

### **總結**

1. 安裝 zsh 並設置為默認 Shell。
2. 安裝 Oh-My-Zsh 並自定義主題與插件。
3. 測試並使用功能強大的 zsh 終端。

如果需要進一步幫助，隨時告訴我！