## 🏷️ - Tags: #GCP #whisper #whisper_streaming

### 為什麼做這件事情
1. 把整個 socker 服務架上去
2. 驗證 Nvidia L4 的速度跟體感
### 希望得到的資訊
1. 使用者可以從遠端直接使用服務
2. 評估網路的 latency
3. 使用者斷掉之後能把 process 清空後繼續做
4. 跟 RTX-4090 、RTX-2080 的體感比較
### 怎麼做
1. 在 Google compute engine 上架設 GCP
	1. 選擇 nvidia L4
	2. 選擇最小 cpu ram 配置方案
	3. 選擇 GCP 提供的 cuda image 版本 12.1
	4. 把 ssh public key 上傳
	5. 修改使用者端
```
# 修改 /home/acer/.ssh/config，把 gcp 的 host 加進去，注意每次重啟後都會改變 ip


```
```
```
	6. 把 previledges 跟 firewall 的限制都打開來
	7. 在 firewall 新增規則，設定 port number，來源是 0.0.0.0/0
	8. windows clinet 開 powershell 測試
		1. Test-NetConnection -ComputerName  34.81.217.104  -Port 43007 
3. 把 whisper_streaming 移植過去
4. 如果 client 端是 windows 的話需要裝 wsl [[如何在 WSL 上啟用 audio device]]
5. 把 clinet 端改成 gui 加上執行檔的方式，方便 demo
6. whisper 目前會輸出簡體，在 client 端轉成繁體輸出
### 結論
1. 送到 gcp 的延遲時間是 20ms，，其實差不多

### 參考
1. [SSH 進 GCP的3種方式 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10251134)