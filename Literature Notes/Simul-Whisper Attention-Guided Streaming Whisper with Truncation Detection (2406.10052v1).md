---
paper id: 2406.10052v1
title: "Simul-Whisper: Attention-Guided Streaming Whisper with Truncation Detection"
authors: Haoyu Wang, Guoqiang Hu, Guodong Lin, Wei-Qiang Zhang, Jian Li
publication date: 2024-06-14T14:07:26Z
abstract: As a robust and large-scale multilingual speech recognition model, Whisper has demonstrated impressive results in many low-resource and out-of-distribution scenarios. However, its encoder-decoder structure hinders its application to streaming speech recognition. In this paper, we introduce Simul-Whisper, which uses the time alignment embedded in Whisper's cross-attention to guide auto-regressive decoding and achieve chunk-based streaming ASR without any fine-tuning of the pre-trained model. Furthermore, we observe the negative effect of the truncated words at the chunk boundaries on the decoding results and propose an integrate-and-fire-based truncation detection model to address this issue. Experiments on multiple languages and Whisper architectures show that Simul-Whisper achieves an average absolute word error rate degradation of only 1.46% at a chunk size of 1 second, which significantly outperforms the current state-of-the-art baseline.
comments: Accepted by INTERSPEECH 2024
pdf: "[[Assets/Simul-Whisper Attention-Guided Streaming Whisper with Truncation Detection (2406.10052v1).pdf]]"
url: https://arxiv.org/abs/2406.10052v1
tags:
  - asr
  - whisper
  - streaming
---
#研究背景與問題
- **研究背景**:
- **核心問題**: 

#方法與技術 
- **模型/方法**: 
- **實驗設置**: 
#結果與分析 
- **主要結果**: 
- **圖表分析**: 

#自我反思 
- **啟發**: 
- **如何應用到自己工作**: