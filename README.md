# Web Noter

單一 HTML 檔案的 Markdown 筆記工具，透過 GitHub Contents API 直接讀寫 GitHub repository，無需後端、無需安裝。

## 特色

- **零依賴**：單一 `.html` 檔案，直接瀏覽器開啟即用
- **GitHub 儲存**：筆記存於你的 GitHub repo，版本控制免費附贈
- **YAML Frontmatter**：自動產生 `title`、`source`、`date`、`tags` 欄位
- **附加模式**：保留既有內容，在末尾附加新內容（附時間戳記）
- **深色主題**：Catppuccin Mocha 配色

---

## 快速開始

### 1. 準備 GitHub Personal Access Token (PAT)

前往 GitHub → Settings → Developer settings → Personal access tokens → **Tokens (classic)**，建立一個勾選 `repo` 權限的 token。

### 2. 開啟 web-noter.html

用瀏覽器直接開啟 `web-noter.html`（`file://` 協定即可）。

### 3. 填入設定

點右上角 **⚙️ 設定**，填入：

| 欄位 | 說明 | 範例 |
|------|------|------|
| Personal Access Token | 步驟 1 產生的 PAT | `ghp_xxxxxxxxxxxx` |
| GitHub 帳號 (Owner) | 你的 GitHub 使用者名稱 | `your-username` |
| Repository 名稱 | 存放筆記的 repo | `my-notes` |
| Branch | 目標分支 | `main` |
| 路徑前綴（資料夾） | 筆記存放的子目錄 | `notes/` |

設定儲存於瀏覽器 `localStorage`，重開頁面不需重填。

---

## 使用說明

### 介面結構

```
┌─────────────────────────────────────────┐
│  Web Noter ●            [⚙️ 設定]       │  ← Header（● 表示有未儲存變更）
├───────────┬─────────────────┬───────────┤
│  檔名     │   來源 URL      │   標籤    │  ← Meta 欄位
├───────────┴─────────────────┴───────────┤
│  [📋 列表]  [🍳 食譜]                   │  ← Toolbar
├─────────────────────────────────────────┤
│                                         │
│   編輯區（Markdown）                    │
│                                         │
├─────────────────────────────────────────┤
│  [➕ 附加]  [📂 載入]  [✏️ 覆蓋寫入]   │  ← Footer
└─────────────────────────────────────────┘
```

### Meta 欄位

| 欄位 | 說明 |
|------|------|
| **檔名** | 儲存的檔案名稱，`.md` 副檔名可省略 |
| **來源 URL** | 筆記的參考來源（會寫入 frontmatter） |
| **標籤** | 逗號分隔，例如 `cooking, recipe` |

### 按鈕功能

#### ➕ 附加至 GitHub
將編輯區內容**附加**到 GitHub 上的既有檔案末尾。若檔案不存在則新建。附加時會插入帶時戳的分隔線：

```markdown
（既有內容）

---
*附加於 2026-03-23 14:30*

（新內容）
```

> **Ctrl + S** 快捷鍵觸發此操作。

#### 📂 載入
從 GitHub 載入指定檔名的筆記，自動解析 frontmatter 填入 Meta 欄位。

#### ✏️ 覆蓋寫入
以編輯區**現有內容完整取代** GitHub 上的檔案，操作前會跳出確認對話框。

#### 📋 列表
列出設定路徑下所有 `.md` 檔案，顯示檔名、大小、最後修改日期（並行查詢各檔案的最新 commit）。

#### 🍳 食譜
套用做菜筆記範本，快速填入標準格式。

---

## 檔案格式

儲存至 GitHub 的筆記包含自動產生的 YAML frontmatter：

```markdown
---
title: "我的筆記"
source: "https://example.com"
date: 2026-03-23
tags: ["cooking", "recipe"]
---

正文內容從這裡開始...
```

---

## 鍵盤快捷鍵

| 快捷鍵 | 功能 |
|--------|------|
| `Ctrl + S` / `Cmd + S` | 附加至 GitHub |
| `Tab` | 在編輯區插入 2 個空格縮排 |

---

## 設定儲存位置

所有設定儲存於瀏覽器 `localStorage`：

| Key | 內容 |
|-----|------|
| `wn_pat` | Personal Access Token |
| `wn_owner` | GitHub 帳號 |
| `wn_repo` | Repository 名稱 |
| `wn_branch` | Branch（預設 `main`） |
| `wn_prefix` | 路徑前綴 |

清除瀏覽器資料會一併清除設定，請妥善保管 PAT。

---

## 注意事項

- PAT 儲存於 `localStorage`，避免在公用電腦使用
- 「覆蓋寫入」會完全取代遠端檔案，操作前請確認
- 「列表」功能會對每個 `.md` 檔案各發一次 GitHub API 請求，檔案數量多時速度較慢
- GitHub API 有速率限制（已認證用戶每小時 5,000 次請求）
