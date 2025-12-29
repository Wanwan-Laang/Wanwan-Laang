# GitHub Stats 自託管設置指南

這個指南將幫助你設置自託管的 GitHub Readme Stats，以支持顯示 private repositories 的統計數據。

## 方案 1：使用 Vercel 自託管（推薦）

### 步驟 1：Fork github-readme-stats 倉庫

1. 訪問 https://github.com/anuraghazra/github-readme-stats
2. 點擊右上角的 "Fork" 按鈕
3. 將倉庫 fork 到你的 GitHub 帳戶

### 步驟 2：在 Vercel 上部署

1. 訪問 https://vercel.com
2. 使用 GitHub 帳戶登錄
3. 點擊 "New Project"
4. 選擇你剛剛 fork 的 `github-readme-stats` 倉庫
5. 點擊 "Deploy"

### 步驟 3：配置環境變數

1. 在 Vercel 項目設置中，進入 "Environment Variables"
2. 添加以下環境變數：
   - **Name**: `PAT_1`
   - **Value**: 你的 GitHub Personal Access Token（見下方如何創建）
   - **Environment**: Production, Preview, Development（全部選中）

### 步驟 4：創建 GitHub Personal Access Token

1. 訪問 https://github.com/settings/tokens
2. 點擊 "Generate new token" → "Generate new token (classic)"
3. 設置以下權限：
   - `repo` (完整控制 private repositories)
   - `read:user` (讀取用戶信息)
4. 生成 token 並複製（只會顯示一次！）

### 步驟 5：更新 GitHub Secrets

1. 在你的 GitHub 倉庫中，進入 Settings → Secrets and variables → Actions
2. 添加以下 secrets：
   - **Name**: `GITHUB_PAT`
   - **Value**: 你的 GitHub Personal Access Token
   - **Name**: `GITHUB_STATS_API`
   - **Value**: 你的 Vercel 部署 URL（例如：`https://your-project.vercel.app`）

### 步驟 6：測試

1. 在 GitHub Actions 中手動觸發 "Update GitHub Stats" workflow
2. 檢查生成的圖片是否包含 private repos 的數據

## 方案 2：使用現有設置（僅公共倉庫）

如果你不需要顯示 private repos，當前的設置已經可以正常工作。workflow 會自動：
- 每天更新統計圖片
- 將圖片保存到 `images/` 目錄
- 自動提交更新

## 注意事項

- **安全**：永遠不要在代碼中直接寫入 token
- **Token 權限**：確保 PAT 有 `repo` 權限才能訪問 private repos
- **Vercel 免費額度**：Vercel 免費計劃有使用限制，但對於個人使用通常足夠

## 故障排除

如果圖片沒有更新：
1. 檢查 GitHub Actions 的運行日誌
2. 確認 secrets 已正確設置
3. 確認 Vercel 部署正常運行
4. 檢查 token 權限是否正確

