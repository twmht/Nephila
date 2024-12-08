### 為什麼要做這件事情

主要是 whisper streaming [[Turning Whisper into Real-Time Transcription System (2307.14743v2)]] 是直接透過暴力法去做，單一個使用者去使用可能還可以，但是同時多個人去使用的話，需要的硬體資源會變得非常龐大 ([batching multi-client server · Issue #42 · ufal/whisper_streaming](https://github.com/ufal/whisper_streaming/issues/42))，GPU 會有競爭的問題，同一顆 GPU 在使用的時候，如果同時跑兩個 whisper model，速度會呈現線性下降的趨勢。
![[faster_whisper_server_benchmark_avg.png]]
![[faster_whisper_server_benchmark_max.png]]
而 whisper 本來的目的就是拿來做 offline 的模型，天生的架構其實不適合拿來做 online，而 SenseVocie-Small 補足了這個缺點，他本身是一個 non-autoregessive 的模型，速度飛快，適合拿來做 streaming 的任務。
### 希望得到的資訊
* 研究 senseVocie-small streaming 跟 whisper streaming 在速度上的差異
* 研究 small 版本的架構
### 怎麼做
```bash
uv venv .venv/funasr
git clone --recursive https://github.com/modelscope/FunASR.git
git clone --recursive https://github.com/modelscope/modelscope.git
cd FunASR && uv pip install -e .
cd modelscope && uv pip install -e .
```
### 結論
#### 

### 參考
* [FunAudioLLM/SenseVoice: Multilingual Voice Understanding Model](https://github.com/FunAudioLLM/SenseVoice)
