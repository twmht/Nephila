## 🏷️ Tags: #whisper

### 為什麼做這件事情
每一個 whisper 的實作版本的速度褒貶不一，不同的音檔時間處理的速度也都不同。
所花費的 vram 也會跟音檔的時間有關。
目前 github 上作者實測的例子都不多，參考度不高。
簡單來說，自己實測最準
### 希望得到的資訊
增加不同框架的 whisper 在不同硬體上面以及不同的音檔長度上面所花費的時間
### 怎麼做
* 測試 30 秒、5分鐘、30分鐘的音檔
* 測試不同框架的 whisper
	* faster-whisper
	* isanely fast whisper
	* whispers2t
	* whisperjax
	* other cloud api
### 結論

|       | google chrip api | openai whisper api | RTX 4090 faster whisper | RTX 2080 faster whisper | RTX 2080 whisper jax (cu112) | A00 faster whisper | RTX 2080 WhisperS2T TensorRT-LLM (0.8.0.dev2024012301),beam size=1 | RTX 2080 WhisperS2T Ctranslate2,beam size=1 | RTX 2080 WhisperS2T Ctranslate2,beam size=5 | RTX 2080 WhisperS2T TensorRT-LLM (0.8.0.dev2024012301),beam size=5 |
| ----- | ---------------- | ------------------ | ----------------------- | ----------------------- | ---------------------------- | ------------------ | ------------------------------------------------------------------ | ------------------------------------------- | ------------------------------------------- | ------------------------------------------------------------------ |
| 3s    |                  |                    | 0.26s                   |                         |                              | 0.97s              |                                                                    |                                             |                                             |                                                                    |
| 30s   | 7s               | 2.46s              | 1.88s                   |                         |                              |                    |                                                                    |                                             |                                             |                                                                    |
| 5min  | 7s               | 15.3s              | 15.53                   |                         |                              |                    |                                                                    |                                             |                                             |                                                                    |
| 30min | 39s              | 67.17s             | 102s                    |                         |                              |                    |                                                                    |                                             |                                             |                                                                    |
| 1hour |                  |                    |                         | 300s                    | 471.6s                       |                    | 120s                                                               | 120s                                        | 140s                                        | 129s                                                               |
|       |                  |                    |                         |                         |                              |                    |                                                                    |                                             |                                             |                                                                    |
|       |                  |                    |                         |                         |                              |                    |                                                                    |                                             |                                             |                                                                    |

|       | RTX 2080 faster whisper | RTX 2080 whisper jax (cu112) | RTX 2080 WhisperS2T TensorRT-LLM (0.8.0.dev2024012301),beam size=5 |
| ----- | ----------------------- | ---------------------------- | ------------------------------------------------------------------ |
| 1hour | 4457.50MB               | 8751.50MB                    |                                                                    |
| Fluer |                         |                              | 9353MB                                                             |

![[i-compared-the-different-open-source-whisper-packages-for-v0-ntchq1z82jrc1.webp]]

### 參考
* [Batch decoding by funboarder13920 · Pull Request #588 · SYSTRAN/faster-whisper](https://github.com/SYSTRAN/faster-whisper/pull/588)
* [Open ASR Leaderboard - a Hugging Face Space by hf-audio](https://huggingface.co/spaces/hf-audio/open_asr_leaderboard)
* [SOTA ASR Tooling: Long-form Transcription - by Amgad Hasan](https://amgadhasan.substack.com/p/sota-asr-tooling-long-form-transcription)