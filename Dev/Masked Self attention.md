## **Masked Self-Attention 解釋**

### **1. Masked Self-Attention 是什麼？**

Masked Self-Attention（掩碼自注意力）是 Transformer **Decoder** 內部的一種 **自注意力機制（Self-Attention）**，其核心目標是：

> **在生成下一個詞時，只能關注已經產生的詞，而不能看到未來的詞，以防止資訊洩漏。**

這與一般的 **Self-Attention** 不同，因為普通的 Self-Attention 允許每個詞關注整個輸入序列，而 Masked Self-Attention 會刻意遮蔽未來的資訊。

### **2. Masked Self-Attention 用在哪裡？**

- **GPT / ChatGPT**（純 Decoder-only 模型）
- **Transformer Decoder（如 Whisper、T5、BART）**
- **機器翻譯、語言模型**（需要逐步生成的任務）

當我們在 **Decoder** 內部執行 Masked Self-Attention 時，每個詞 **只能觀察它之前的詞**，而不能看到它之後的詞。

---

## **2. Masked Self-Attention 計算步驟**

Masked Self-Attention 的計算過程與普通 **Self-Attention** 類似，但 **在計算時會使用 Mask 來遮蔽未來的詞**。

### **Step 1: 計算 Q, K, V**

與一般 Self-Attention 相同，每個輸入 token 都會產生 **Query（Q）**、**Key（K）** 和 **Value（V）**：

Q=XWQ,K=XWK,V=XWVQ = X W_Q, \quad K = X W_K, \quad V = X W_V

這裡：

- QQ（Query）：用來查詢關聯詞。
- KK（Key）：用來匹配 Query 以計算權重。
- VV（Value）：包含實際的詞向量資訊。

---

### **Step 2: 計算注意力分數**

與普通 Attention 一樣，我們計算 Query 和 Key 的相似度：

Attention Score=QKTdk\text{Attention Score} = \frac{Q K^T}{\sqrt{d_k}}

這個計算會產生一個 **(seq_len, seq_len)** 的方陣，表示 **序列中每個詞與其他詞的相似度**。

**舉例：** 假設 Decoder 目前處理 `"I love Machine Learning"`，Self-Attention 計算出的關注矩陣可能是：

```
      I    love  Machine Learning
I     ✔️    ✔️    ✔️      ✔️
love  ✔️    ✔️    ✔️      ✔️
Machine ✔️  ✔️    ✔️      ✔️
Learning ✔️ ✔️    ✔️      ✔️
```

在 **一般的 Self-Attention** 中，每個詞可以關注任何其他詞（雙向注意力）。

---

### **Step 3: 加入 Mask，隱藏未來詞**

在 **Masked Self-Attention** 中，我們使用 **下三角 Mask（Lower Triangular Mask）**，遮蔽 **未來詞的注意力分數**：

```
      I    love  Machine Learning
I     ✔️    ❌    ❌      ❌
love  ✔️    ✔️    ❌      ❌
Machine ✔️  ✔️    ✔️      ❌
Learning ✔️ ✔️    ✔️      ✔️
```

這意味著：

- `"I"` 只能關注 `"I"`。
- `"love"` 只能關注 `"I", "love"`。
- `"Machine"` 只能關注 `"I", "love", "Machine"`。
- `"Learning"` 只能關注 `"I", "love", "Machine", "Learning"`。

實際實現時，這是透過 **將 Masked 區域設為 -∞** 來讓 Softmax 變成 0：

Masked Score=[✔−∞−∞−∞✔✔−∞−∞✔✔✔−∞✔✔✔✔]\text{Masked Score} = \begin{bmatrix} ✔ & -\infty & -\infty & -\infty \\ ✔ & ✔ & -\infty & -\infty \\ ✔ & ✔ & ✔ & -\infty \\ ✔ & ✔ & ✔ & ✔ \end{bmatrix}

---

### **Step 4: Softmax 計算注意力權重**

對 **Masked Score** 套用 **Softmax**，將 -∞ 轉換為 0：

Attention Weights=Softmax(Masked Score)\text{Attention Weights} = \text{Softmax}(\text{Masked Score})

這樣，未來的詞就無法影響當前的詞，確保 **Decoder 只能使用過去詞來生成新詞**。

---

### **Step 5: 計算輸出**

最後，我們將 **Attention Weights** 乘上 Value（V）：

Output=Attention Weights×V\text{Output} = \text{Attention Weights} \times V

這樣，每個詞的最終表示 **只來自於它已經看到的詞**，確保語言模型是 **自回歸（Auto-Regressive）** 的。

---

## **3. Masked Self-Attention 的作用**

✅ **防止未來資訊洩露**

- 在訓練過程中，確保模型 **只能使用已生成的詞** 來預測下一個詞，這對於語言模型至關重要。

✅ **適用於自回歸模型（Auto-Regressive Models）**

- **GPT, Whisper, T5, BART** 都使用 Masked Self-Attention，確保模型逐步生成輸出，而不會一次性產生完整序列。

✅ **提升文本生成的流暢性**

- 讓模型能夠學習「逐步生成」的策略，而不是一次性預測所有詞，這樣可以更靈活地調整詞序。

---

## **4. PyTorch 實現 Masked Self-Attention**

```python
import torch
import torch.nn.functional as F

class MaskedSelfAttention(torch.nn.Module):
    def __init__(self, embed_dim):
        super(MaskedSelfAttention, self).__init__()
        self.embed_dim = embed_dim

        # Q, K, V 的線性變換
        self.W_Q = torch.nn.Linear(embed_dim, embed_dim, bias=False)
        self.W_K = torch.nn.Linear(embed_dim, embed_dim, bias=False)
        self.W_V = torch.nn.Linear(embed_dim, embed_dim, bias=False)

    def forward(self, x):
        seq_len = x.shape[1]

        # 計算 Q, K, V
        Q = self.W_Q(x)  # (batch, seq_len, embed_dim)
        K = self.W_K(x)  # (batch, seq_len, embed_dim)
        V = self.W_V(x)  # (batch, seq_len, embed_dim)

        # 計算注意力分數
        scores = torch.matmul(Q, K.transpose(-2, -1)) / (self.embed_dim ** 0.5)  # (batch, seq_len, seq_len)

        # 創建 Mask（下三角矩陣）
        mask = torch.tril(torch.ones(seq_len, seq_len))  # (seq_len, seq_len)
        scores = scores.masked_fill(mask == 0, float('-inf'))  # 掩蓋未來詞

        # Softmax 計算權重
        attention_weights = F.softmax(scores, dim=-1)

        # 計算加權輸出
        output = torch.matmul(attention_weights, V)  # (batch, seq_len, embed_dim)

        return output, attention_weights

# 測試 Masked Self-Attention
embed_dim = 4
seq_len = 5
x = torch.rand(1, seq_len, embed_dim)  # 隨機初始化輸入

masked_attention = MaskedSelfAttention(embed_dim)
output, attn_weights = masked_attention(x)

print("Masked Attention Weights:\n", attn_weights)
```

---

## **5. 結論**

**Masked Self-Attention**：

1. **與一般 Self-Attention 相同，但會遮蔽未來詞**。
2. **確保模型逐步生成詞，不會提前「偷看」未來的詞**。
3. **廣泛應用於 GPT、Whisper、BART、T5 等模型**，適用於翻譯、語音轉錄、文本生成等任務。

🚀 **Masked Self-Attention 是語言模型中「自回歸生成」的關鍵技術，確保了 Transformer 能夠逐步產生合理的輸出！** 🎯


## **Masked Self-Attention vs. Auto-Regressive Model（自回歸模型）**

這兩個概念在 Transformer 模型中息息相關，尤其是在 **GPT、Whisper、機器翻譯（如 T5, BART）、語音識別（ASR）** 等模型中廣泛使用。讓我們用一個具體的範例來理解它們的區別與聯繫。

---

## **1. Masked Self-Attention：確保序列逐步生成**

### **🔹 概念**

**Masked Self-Attention** 是 Transformer **Decoder** 內部的一種 **自注意力機制（Self-Attention）**，它的作用是：

> **讓每個 token 只能關注它「之前」的 token，而不能看到「未來」的 token，防止資訊洩漏。**

這與普通的 **Self-Attention（全局關注）** 不同，因為普通的 Self-Attention 允許每個詞關注整個輸入，而 **Masked Self-Attention 會刻意遮蔽未來的詞**。

### **🔹 範例**

假設我們使用 Transformer 來生成句子：

```
"I love NLP"
```

當我們使用 **Masked Self-Attention** 時：

- **"I"** 只能看到自己，不能看到 "love" 和 "NLP"。
- **"love"** 可以看到 "I" 但不能看到 "NLP"。
- **"NLP"** 可以看到 "I" 和 "love"。

這用一個矩陣來表示：

```
      I    love   NLP
I     ✔️    ❌    ❌
love  ✔️    ✔️    ❌
NLP   ✔️    ✔️    ✔️
```

🔹 **數學表示（使用 Mask）**：

Masked Score=[✔−∞−∞✔✔−∞✔✔✔]\text{Masked Score} = \begin{bmatrix} ✔ & -\infty & -\infty \\ ✔ & ✔ & -\infty \\ ✔ & ✔ & ✔ \end{bmatrix}

這樣，模型在 **計算注意力分數（Softmax 前）** 時：

- **被 Mask 遮蔽的部分會變成 -∞，Softmax 後機率變為 0**，確保該詞 **無法關注未來的詞**。

---

## **2. Auto-Regressive Model（自回歸模型）：逐步生成序列**

### **🔹 概念**

**Auto-Regressive Model（自回歸模型）** 是一種逐步生成序列的方法，它的主要特點是：

> **當前的詞只能依賴「過去」已生成的詞，逐步產生下一個詞。**

這與 **Masked Self-Attention 的概念完全一致**，但不同的是：

- **Masked Self-Attention 是 Transformer Decoder 內部的一種計算方法**。
- **Auto-Regressive Model 是一種生成方式，適用於 RNN、LSTM、Transformer 等模型**。

### **🔹 範例**

當我們使用 **自回歸方式** 來生成 `"I love NLP"` 時：

1. **第一步：生成 "I"**
    
    ```
    [START] -> "I"
    ```
    
    - 這時候 **模型沒有歷史資訊**，完全依賴 `<START>` 來預測 "I"。
2. **第二步：根據 "I" 來生成 "love"**
    
    ```
    [START] -> "I" -> "love"
    ```
    
    - 這時候模型只能看到 `"I"`，根據 `"I"` 來選擇最可能的下一個詞。
3. **第三步：根據 "I love" 來生成 "NLP"**
    
    ```
    [START] -> "I" -> "love" -> "NLP"
    ```
    
    - 這時候模型只能看到 `"I love"`，根據它來選擇 `"NLP"`。

這個過程稱為 **自回歸（Auto-Regressive）生成**，因為： ✅ **每一步的輸入來自於「過去已經生成的詞」**  
✅ **無法一次性看到整個序列，必須逐步生成**

這與 Masked Self-Attention 相匹配，因為 Mask 確保了模型在 Decoder 訓練時 **只能看到已生成的詞，而不能看到未來的詞**。

---

## **3. Masked Self-Attention vs. Auto-Regressive Model**

|**概念**|**Masked Self-Attention**|**Auto-Regressive Model**|
|---|---|---|
|**定義**|Transformer Decoder 內部的一種機制，用於 Mask 未來詞|一種逐步生成的方式，確保模型只能依賴已生成的詞|
|**目的**|防止資訊洩露，確保生成過程是逐步的|避免一次性生成完整序列，讓模型學習合理的語言結構|
|**應用**|Transformer（GPT, Whisper, T5, BART）|RNN, LSTM, Transformer（GPT, Whisper）|
|**是否 Mask 未來詞？**|是|是（因為它是逐步生成的）|
|**是否需要過去輸出？**|不需要（可以平行計算）|需要（因為下一步的輸入來自前一步的輸出）|
|**運行方式**|訓練時 Mask 所有未來詞，讓 Decoder 只能關注過去詞|推理時逐步輸入前一步輸出，直到生成完整序列|

🚀 **結論：**

- **Masked Self-Attention 是 Auto-Regressive Model 在 Transformer 內部的一種計算方式！**
- **它確保 Transformer Decoder 只能看過去的詞，而不能看未來的詞，從而滿足 Auto-Regressive 生成的要求。**

---

## **4. PyTorch 範例**

### **🔹 Masked Self-Attention（內部計算方式）**

```python
import torch
import torch.nn.functional as F

def masked_self_attention(x):
    seq_len = x.shape[1]

    # 創建 Q, K, V
    Q = x  
    K = x  
    V = x  

    # 計算注意力分數
    scores = torch.matmul(Q, K.transpose(-2, -1)) / (x.shape[-1] ** 0.5)

    # 創建 Mask，遮蔽未來詞
    mask = torch.tril(torch.ones(seq_len, seq_len))  # (seq_len, seq_len)
    scores = scores.masked_fill(mask == 0, float('-inf'))  

    # Softmax 計算權重
    attention_weights = F.softmax(scores, dim=-1)

    # 計算加權輸出
    output = torch.matmul(attention_weights, V)  

    return output, attention_weights

# 測試範例
x = torch.rand(1, 5, 4)  # (batch, seq_len, embed_dim)
output, attn_weights = masked_self_attention(x)
print("Masked Attention Weights:\n", attn_weights)
```

---

### **🔹 Auto-Regressive（逐步生成）**

```python
import torch
import torch.nn.functional as F

# 假設 vocab_size = 10000, embed_dim = 4
class AutoRegressiveModel(torch.nn.Module):
    def __init__(self, embed_dim, vocab_size):
        super(AutoRegressiveModel, self).__init__()
        self.embed_dim = embed_dim
        self.output_layer = torch.nn.Linear(embed_dim, vocab_size)

    def forward(self, hidden_state):
        logits = self.output_layer(hidden_state)  # (batch, seq_len, vocab_size)
        probabilities = F.softmax(logits, dim=-1)
        next_word_index = torch.argmax(probabilities, dim=-1)
        return next_word_index

# 測試逐步生成
model = AutoRegressiveModel(embed_dim=4, vocab_size=10000)
hidden_state = torch.rand(1, 1, 4)  # 只給 1 個詞
next_word_index = model(hidden_state)
print("Next Word Index:", next_word_index)
```

---

## **5. 總結**

📌 **Masked Self-Attention**：在 **Transformer Decoder 內部** 使用 Mask 來避免未來資訊洩露，確保自回歸特性。  
📌 **Auto-Regressive Model**：一種 **逐步生成的方式**，保證下一個詞 **只能依賴過去詞**，應用於 GPT、Whisper 等模型。

🚀 **Masked Self-Attention 是 Auto-Regressive 的關鍵技術，讓 Transformer 能夠正確地逐步生成序列！** 🎯