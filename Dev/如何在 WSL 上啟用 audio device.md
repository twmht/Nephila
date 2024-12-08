
WSL 預設不支援 audio device，但可以透過一些方式讓他可以支援

1. 先去下載 pulseAudio on windows  [PulseAudio on Windows – PulseAudio](https://www.freedesktop.org/wiki/Software/PulseAudio/Ports/Windows/Support/) ，解壓縮，並且修改 `etc/pulse/default.pa` ，加入
```
load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1
load-module module-native-protocol-tcp auth-anonymous=1
```
2. 開啟 powershell 啟動 pusleAudio
```bash
pulseaudio.exe --exit-idle-time=-1
```
3. 檢查 pulseAudio 是否有在監聽
```bash
netstat -ano | findstr 4713
```
5. 開啟 wsl 安裝 pulseAudio
```bash
sudo apt install pulseaudio alsa-utils
```
6.  開啟 wsl 隨便選一個音檔撥放
```bash
paplay test.wav
```