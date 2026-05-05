# Web Noter

單一 HTML 檔案的 Markdown 筆記工具，透過 GitHub Contents API 直接讀寫 GitHub repository，無需後端、無需安裝。

## 特色

- **零依賴**：單一 `.html` 檔案，直接瀏覽器開啟即用
- **GitHub 儲存**：筆記存於你的 GitHub repo，版本控制免費附贈
- **YAML Frontmatter**：自動產生 `title`、`source`、`date`、`tags` 欄位
- **附加模式**：保留既有內容，在末尾附加新內容（附時間戳記）
- **深色主題**：Catppuccin Mocha 配色
- **手機支援**：語音輸入、拍照 OCR（iPhone Safari）

---

## 快速開始

### 1. 準備 GitHub Personal Access Token (PAT)

前往 GitHub → Settings → Developer settings → Personal access tokens → **Fine-grained tokens**，建立一個只授權此 repo `Contents: Read and write` 權限的 token。

### 2. 開啟 web-noter.html

**桌機**：用瀏覽器直接開啟 `web-noter.html`（`file://` 協定即可）。

**手機（iPhone）**：需透過 HTTPS 才能使用語音輸入，建議部署至 GitHub Pages（見下方）。

### 3. 填入設定

點右上角 **⚙️ 設定**，填入：

| 欄位 | 說明 | 範例 |
|------|------|------|
| Personal Access Token | 步驟 1 產生的 PAT | `ghp_xxxxxxxxxxxx` |
| GitHub 帳號 (Owner) | 你的 GitHub 使用者名稱 | `your-username` |
| Repository 名稱 | 存放筆記的 repo | `my-notes` |
| Branch | 目標分支 | `main` |
| 路徑前綴（資料夾） | 筆記存放的子目錄 | `notes/` |
| Claude API Key | 用於 OCR 精確辨識（選填） | `sk-ant-api03-...` |

設定儲存於瀏覽器 `localStorage`，重開頁面不需重填。

---

## 部署到 GitHub Pages（手機使用推薦）

語音輸入需要 HTTPS，部署至 GitHub Pages 可取得免費 HTTPS 網址：

1. 將 `web-noter.html` push 到 GitHub repo 的 `main` branch
2. 進入 repo → **Settings** → **Pages**
3. Source 選 **Deploy from a branch**，Branch 選 `main`，資料夾選 `/ (root)` → **Save**
4. 約 1–2 分鐘後網址出現：`https://<帳號>.github.io/<repo名稱>/web-noter.html`
5. 用 iPhone Safari 開啟此網址即可完整使用所有功能

之後每次 `git push` 自動更新，無需額外操作。

---

## 使用說明

### 介面結構

```
┌─────────────────────────────────────────────────────┐
│  Web Noter ●                          [⚙️ 設定]     │  ← Header（● 表示有未儲存變更）
├───────────┬─────────────────────────┬───────────────┤
│  檔名     │   來源 URL              │   標籤        │  ← Meta 欄位
├───────────┴─────────────────────────┴───────────────┤
│  [📋 列表]  [🍳 食譜]  [🎤 語音]  [📷 拍照]        │  ← Toolbar
├─────────────────────────────────────────────────────┤
│  （拍照後顯示）已選擇圖片：xxx.jpg                  │
│  [🔍 快速辨識]  [✨ 精確辨識]  [✕]                 │  ← OCR 面板（選圖後展開）
├─────────────────────────────────────────────────────┤
│                                                     │
│   編輯區（Markdown）                               │
│                                                     │
├─────────────────────────────────────────────────────┤
│  [➕ 附加]  [📂 載入]  [✏️ 覆蓋寫入]  狀態訊息    │  ← Footer
└─────────────────────────────────────────────────────┘
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

> **Ctrl + S** / **Cmd + S** 快捷鍵觸發此操作。

#### 📂 載入
從 GitHub 載入指定檔名的筆記，自動解析 frontmatter 填入 Meta 欄位。

#### ✏️ 覆蓋寫入
以編輯區**現有內容完整取代** GitHub 上的檔案，操作前會跳出確認對話框。

#### 📋 列表
列出設定路徑下所有 `.md` 檔案，顯示檔名、大小、最後修改日期（並行查詢各檔案的最新 commit）。

#### 🍳 食譜
套用做菜筆記範本，快速填入標準格式。

#### 🎤 語音輸入（需 HTTPS）

點按後開始錄音，說完自動停止並將文字插入游標位置。再次點按可手動停止。

- 語言：繁體中文（zh-TW）
- **需要 HTTPS**：透過 `file://` 或 `http://` 開啟時無法使用，請改用 GitHub Pages 網址
- 瀏覽器支援：iOS Safari 14.5+、Chrome for Android、桌面 Chrome / Edge

#### 📷 拍照 OCR

點按後 iOS 會顯示「拍照 / 照片庫 / 瀏覽」選單，選取圖片後出現 OCR 面板：

| 模式 | 技術 | 特點 |
|------|------|------|
| 🔍 快速辨識 | Tesseract.js（純前端） | 免費，首次使用需下載約 20 MB 語言資料，準確度中等 |
| ✨ 精確辨識 | Claude Vision API | 準確度高，需在設定填入 Claude API Key，依使用量計費 |

辨識結果插入編輯器游標位置。

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

所有設定儲存於瀏覽器 `localStorage`，**不會寫入 HTML 檔案或上傳至任何伺服器**：

| Key | 內容 |
|-----|------|
| `wn_pat` | GitHub Personal Access Token |
| `wn_owner` | GitHub 帳號 |
| `wn_repo` | Repository 名稱 |
| `wn_branch` | Branch（預設 `main`） |
| `wn_prefix` | 路徑前綴 |
| `wn_claude_key` | Claude API Key（OCR 精確模式） |

### Token 安全說明

- **GitHub PAT** 只傳送至 `api.github.com`，**Claude API Key** 只傳送至 `api.anthropic.com`，不經過任何第三方
- 部署至 GitHub Pages 後，`localStorage` 受 Same-Origin Policy 保護，其他網站無法讀取
- 建議使用 **Fine-grained token** 並僅授權單一 repo 的 `Contents` 權限，降低外洩影響範圍
- 建議在 Anthropic Console 設定月消費上限，控制 Claude API 費用風險
- 避免在公用或共享帳號的電腦上使用

---

## 注意事項

- 「覆蓋寫入」會完全取代遠端檔案，操作前請確認
- 「列表」功能會對每個 `.md` 檔案各發一次 GitHub API 請求，檔案數量多時速度較慢
- GitHub API 有速率限制（已認證用戶每小時 5,000 次請求）
- Tesseract OCR 首次使用須下載繁中（~16 MB）與英文（~4 MB）語言資料，之後由瀏覽器快取
