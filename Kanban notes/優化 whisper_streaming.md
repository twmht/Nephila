## 🏷️ - Tags: #whisper #streaming

### 為什麼做這件事情
[[Turning Whisper into Real-Time Transcription System (2307.14743v2)]] 上的 github 實作有一些 issue
#### Issues
- [ ] 有時候會比對很久才去 confirm，然後會 confirm 出一長串出來
- [x] faster-whisper 閒置太久會掛掉 ✅ 2024-12-30
- [x] server 端簡轉繁體 ✅ 2024-12-30
- [ ] 詞級別轉成字級別


![[Pasted image 20241211153338.png]]

#### Features
- [ ] 🔼 優化串流塊  [whisper_streaming/whisper_online_server.py at main · ufal/whisper_streaming](https://github.com/ufal/whisper_streaming/blob/main/whisper_online_server.py#L110) ，可能要研究  webrtc
- [ ] 🔼 整合 stable-ts [jianfch/stable-ts: Transcription, forced alignment, and audio indexing with OpenAI's Whisper](https://github.com/jianfch/stable-ts)
- [x] ⏫ hotword ✅ 2024-12-30

#### Bug
- [x] ⏫  錄音錯誤: input overflow ✅ 2024-12-30
- [ ] ⏫ 標點符號輸出不穩定，可能要確認 prompt 有沒有標點符號，沒有的話就是要加上 inital prompt
### 希望得到的資訊
### 怎麼做
### 結論

### 參考