#whisper
#tpu
### 為什麼做這件事情
主要是想要把 [[Turning Whisper into Real-Time Transcription System (2307.14743v2)]] 這套弄到 TPU 上面，TPU 價格便宜很多，但 faster-whisper 是用 CUDA 而不是用 TPU，必須要確認 whisper-jax 怎麼弄到 GCP 上面，以及跑起來的體感有沒有跟 faster-whisper 一樣
### 希望得到的資訊
* 弄到 whisper streaming 上
* 可不可以弄到 TPU 上面
* 功能面有沒有比跟 faster-whisper 一樣
	* 準確度
	* 速度體感有沒有在一到三秒以內

### 怎麼做
### 結論
* whisper-jax 目前沒有 word level timstamp，但是 whisper streaming 需要這功能
* whisper-jax 也沒有 vad 的功能
* 目前看起來 huggingface 版本也有 jax 跟 tf 版本，但 jax 沒有這功能，tf 版本跑起來會有錯誤，似乎是 bug
* whisper-jax 目前有人說測試起來，錯誤率比 faster whisper 高很多 [Minimizing (repetitive) hallucinations and improving accuracy · Issue #148 · sanchit-gandhi/whisper-jax](https://github.com/sanchit-gandhi/whisper-jax/issues/148) 
* 根據以上資訊，whisper-jax 目前無法直接弄到 whisper-streaming 上，至少要花一周時間修改並且確認準確度
### 參考
* [whisper-jax TPU is much slower than faster-whisper T4 · Issue #130 · sanchit-gandhi/whisper-jax](https://github.com/sanchit-gandhi/whisper-jax/issues/130)
* [sanchit-gandhi/whisper-jax: JAX implementation of OpenAI's Whisper model for up to 70x speed-up on TPU.](https://github.com/sanchit-gandhi/whisper-jax)
* [AttributeError: module 'jax.core' has no attribute 'NamedShape' · Issue #199 · sanchit-gandhi/whisper-jax](https://github.com/sanchit-gandhi/whisper-jax/issues/199)