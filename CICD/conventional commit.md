以下是按照 **Conventional Commit** 的規範，將 `type` 和 `scope` 以及相關關鍵字整理成樹狀結構的 **Markdown**，適合用於生成心智圖：

````markdown
# Conventional Commit 規範

## 1. Type（提交類型）
描述提交的主要意圖，以下是常見的 `type` 類型：

### feat
- 表示新增功能。
- 與 **MINOR** 更新（語義化版本號）相關。

### fix
- 表示修復問題或缺陷。
- 與 **PATCH** 更新相關。

### BREAKING CHANGE
- 表示有重大變更。
- 必須包含 `BREAKING CHANGE:` 註腳，或者在 `type!` 中添加 `!`。

### ci
- 表示與持續集成相關的改變，例如工作流、流水線、GitHub Actions 等。

### build
- 表示構建系統的改動，例如修改 `npm`、`webpack`、`maven` 配置。

### chore
- 表示非代碼更改的雜項改動，例如更新依賴、修改腳本。

### docs
- 表示僅修改文檔的提交，例如 README 文件的更改。

### style
- 表示代碼格式的修改（不影響功能或邏輯），例如空格、縮排、分號。

### refactor
- 表示代碼重構（既不新增功能也不修復缺陷）。

### perf
- 表示提升性能的提交，例如優化代碼。

### test
- 表示新增或修改測試代碼。

---

## 2. Scope（影響範圍）
`scope` 是可選的，提供具體的影響上下文，以下是常見的關鍵字建議：

### 功能模塊
- `auth`: 認證相關功能。
- `user`: 用戶管理模塊。
- `payment`: 支付功能。
- `profile`: 個人資料。

### 技術層
- `api`: 後端 API。
- `ui`: 用戶界面。
- `db`: 數據庫。
- `middleware`: 中間件。
- `tests`: 測試相關。

### 配置和工具
- `ci`: 持續集成相關改動。
- `actions`: GitHub Actions。
- `pipeline`: CI/CD 流水線。
- `deps`: 依賴管理。
- `config`: 配置文件改動。

### 文檔和代碼
- `readme`: 修改 README 文件。
- `docs`: 其他文檔相關改動。
- `style`: 代碼格式化改動。

---

## 3. Commit 格式範例

### 新增功能
```plaintext
feat(auth): add JWT token authentication

Added support for JWT tokens to enhance user authentication security.
````

### 修復問題

```plaintext
fix(ui): resolve button alignment issue in the login page

Aligned the login button properly to match the new design specifications.
```

### 重大變更

```plaintext
feat(api)!: deprecate v1 endpoints

BREAKING CHANGE: All v1 API endpoints have been removed. Use v2 endpoints instead.
```

### 持續集成

```plaintext
ci(actions): update cache key for workflows

Optimized workflow cache configuration to reduce build times.
```

### 文檔修改

```plaintext
docs(readme): add installation instructions for contributors

Added a step-by-step guide for setting up the project locally.
```

---

## 4. 規範注意事項

- **描述簡潔明確**：主標題應清楚說明變更內容。
- **正文補充細節**：若需要，正文解釋變更原因或影響。
- **行數限制**：正文每行不超過 100 字符。
- **一致性**：團隊應統一 `type` 和 `scope` 的使用，提升可讀性。

---

這樣的 Markdown 結構既包含了規範，又列出了常見的 `type` 和 `scope` 關鍵字，非常適合轉換為心智圖，便於視覺化和學習。需要進一步擴展或自定義的部分，您可以根據團隊需求添加更多關鍵字或範例！


![[Conventional Commit 規範-Conventional Commit 規範.jpeg]]