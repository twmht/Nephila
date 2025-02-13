又稱為 Cross Attention

```python
import torch
import torch.nn.functional as F

class EncoderDecoderAttention(torch.nn.Module):
    def __init__(self, embed_dim):
        super(EncoderDecoderAttention, self).__init__()
        self.embed_dim = embed_dim

        # Q 來自 Decoder，K 和 V 來自 Encoder
        self.W_Q = torch.nn.Linear(embed_dim, embed_dim, bias=False)
        self.W_K = torch.nn.Linear(embed_dim, embed_dim, bias=False)
        self.W_V = torch.nn.Linear(embed_dim, embed_dim, bias=False)

    def forward(self, encoder_output, decoder_input):
        # 產生 Q, K, V
        Q = self.W_Q(decoder_input)  # (batch, tgt_seq_len, embed_dim)
        K = self.W_K(encoder_output)  # (batch, src_seq_len, embed_dim)
        V = self.W_V(encoder_output)  # (batch, src_seq_len, embed_dim)

        # 計算注意力分數
        d_k = self.embed_dim ** 0.5
        scores = torch.matmul(Q, K.transpose(-2, -1)) / d_k

        # 計算注意力權重
        attention_weights = F.softmax(scores, dim=-1)

        # 計算加權輸出
        output = torch.matmul(attention_weights, V)
        
        return output, attention_weights


# 模擬 Encoder 輸出 (batch=1, src_seq_len=5, embed_dim=4)
encoder_output = torch.rand(1, 5, 4)

# 模擬 Decoder 目前輸入 (batch=1, tgt_seq_len=3, embed_dim=4)
decoder_input = torch.rand(1, 3, 4)

# 建立 Encoder-Decoder Attention 模型
attn = EncoderDecoderAttention(embed_dim=4)

# 執行注意力機制
output, attn_weights = attn(encoder_output, decoder_input)

print("Attention Output:\n", output)
print("\nAttention Weights:\n", attn_weights)

```

其中 attention_output 的 shape 是 (batch_size, target_sequence_length, embedding_dimension)

### **Attention Output 的 shape 是 `(1,3,4)`，它的直觀意義是什麼？**

在 `Encoder-Decoder Attention` 中，這個 shape `(1,3,4)` 代表：

```
(batch_size, target_sequence_length, embedding_dimension)
```

- `1` 👉 **batch size**，代表這是處理一個樣本（句子）。
- `3` 👉 **target sequence length**，代表 **Decoder 目前已經生成了 3 個詞**，這是 `Decoder` 目前的輸出長度。
- `4` 👉 **embedding dimension**，代表 **每個詞的向量維度為 4**，這是 Transformer 內部的表示空間。

---

## **具體含義：**

這個 Attention Output `(1,3,4)` **是一組新的詞嵌入表示（contextual embeddings）**，它們已經包含了 **來自 Encoder 的資訊**。

- **Decoder 目前已經生成 3 個詞**（例如 `"Le chat est"`）。
- **這 3 個詞的每個詞向量**，是通過 **Encoder 的輸出詞** 計算 **注意力加權求和** 後獲得的。
- **這些詞向量會被送入 Decoder 的下一層（如前饋神經網絡，Feed Forward Network）來生成最終的輸出詞**。

---

## **舉例說明**

假設我們要翻譯：

```text
The cat sits on the mat.  (來源語言 - English)
```

對應的 Decoder **目前生成的詞** 為：

```text
Le chat est  (目標語言 - French)
```

那麼 `Attention Output` 的 shape `(1,3,4)`：

- `3` 👉 代表 `"Le"`, `"chat"`, `"est"` 這 3 個詞。
- `4` 👉 代表它們的向量，每個詞的隱藏層表示（hidden representation）。

這些詞向量 **已經包含了來自 Encoder 的資訊**，例如：

- `"chat"`（貓） 可能主要關注 `"cat"`。
- `"est"`（是） 可能主要關注 `"sits"`。

這些詞向量會進一步用於生成 `"assis"`、`"sur"`、`"le"`、`"tapis"` 等詞，直到完整生成 `"Le chat est assis sur le tapis."`。

---

## **總結**

`Attention Output` 的形狀 `(1,3,4)`：

1. **3** 👉 代表 **Decoder 目前生成了 3 個詞**。
2. **4** 👉 代表 **每個詞的嵌入維度**，這些向量用來學習語言的語義關係。
3. **這些詞的向量已經考慮了 Encoder 的資訊**，是最終產生正確輸出的關鍵。

🚀 **這就是 Encoder-Decoder Attention 如何幫助 Transformer 模型理解並翻譯句子的方式！**

### **Attention Weights 的 shape 是 `(1,3,5)`，它的直觀意義是什麼？**

這個 shape `(1,3,5)` 代表：

```
(batch_size, target_sequence_length, source_sequence_length)
```

- **`1`** 👉 **batch size**，表示這是一個樣本（句子）。
- **`3`** 👉 **target sequence length**，表示 **Decoder 目前已經生成了 3 個詞**（即 `"Le"`, `"chat"`, `"est"`）。
- **`5`** 👉 **source sequence length**，表示 **Encoder 輸入句子有 5 個詞**（即 `"The"`, `"cat"`, `"sits"`, `"on"`, `"mat"`）。

---

## **具體含義：**

這個 `Attention Weights (1,3,5)` **是一個注意力矩陣**，表示 **Decoder 在生成每個詞時，它對 Encoder 輸出的關注程度**。

換句話說：

- 每一個 `target token`（Decoder 目前已生成的詞）**會關注 Encoder 的輸入詞**，產生一組機率分布（加總為 1）。
- 這個 `5` 表示 **Decoder 生成的每個詞都會查看 Encoder 輸出的 5 個詞，並分配權重**。
- 這些 **注意力權重（attention scores）** 會告訴模型應該主要關注哪些輸入詞來生成正確的翻譯。

---

## **舉例說明**

假設我們要翻譯：

```text
Source Sentence (Encoder 輸入) : "The cat sits on the mat."
Target Sentence (Decoder 目前輸出) : "Le chat est"
```

那麼 **Attention Weights (1,3,5) 的每一行代表一個 Decoder 生成詞的權重**：

|Decoder 生成詞|`"The"`|`"cat"`|`"sits"`|`"on"`|`"mat"`|
|---|---|---|---|---|---|
|`"Le"`|0.1|0.5|0.2|0.1|0.1|
|`"chat"`|0.05|0.7|0.2|0.03|0.02|
|`"est"`|0.05|0.1|0.75|0.05|0.05|

- `"Le"` 主要關注 `"cat"`（因為 `"Le chat"` 是對 `"The cat"` 的翻譯）。
- `"chat"` 也主要關注 `"cat"`，但可能稍微看了一下 `"sits"`。
- `"est"` 主要關注 `"sits"`（因為 `"est"` 是 `"sits"` 的對應詞）。

這些權重確保了 **每個輸出詞在翻譯時，只關注對應的重要輸入詞**。

---

## **為什麼 Attention Weights 是 `(target_seq_len, source_seq_len)`？**

這是因為：

1. **每個 Decoder 生成的詞都需要計算與 Encoder 輸入詞的相似度**。
2. **Softmax 保證每個 Decoder 生成詞的權重總和為 1**（確保是一個有效的機率分布）。
3. **這些權重決定了輸出的詞應該如何組合 Encoder 的資訊**。

---

## **總結**

📌 `Attention Weights` 的 shape `(1,3,5)` 代表：

- **3** 👉 **Decoder 目前生成了 3 個詞**。
- **5** 👉 **Encoder 輸入包含 5 個詞，每個詞都有不同的重要程度**。
- **這些權重決定 Decoder 生成每個詞時應該關注 Encoder 的哪些詞**，從而提高翻譯準確性。

🚀 **這就是 Transformer 模型如何利用 Encoder-Decoder Attention 來進行高品質翻譯的關鍵！**