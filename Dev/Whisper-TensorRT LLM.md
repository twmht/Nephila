1. git clone [shashikg/WhisperS2T: An Optimized Speech-to-Text Pipeline for the Whisper Model Supporting Multiple Inference Engine](https://github.com/shashikg/WhisperS2T/tree/main)
2. uv python install python 3.10.12
3. uv pip install -e .
4. uv pip install  --no-cache-dir -U torch==2.1.2
5. uv pip install --no-cache-dir tensorrt_llm==0.8.0.dev2024012301 --extra-index-url https://pypi.nvidia.com
6. uv pip install pynvml==11.5.0 

[TensorRT 9 not available · Issue #35 · NVIDIA/TensorRT-LLM](https://github.com/NVIDIA/TensorRT-LLM/issues/35)
[TensorRT-LLM for Whisper: AttributeError: 'PretrainedConfig' object has no attribute 'n_audio_ctx' · Issue #2449 · NVIDIA/TensorRT-LLM](https://github.com/NVIDIA/TensorRT-LLM/issues/2449)