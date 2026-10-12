# 內埔裝車助手・電腦版

獨立的靜態網站：不需要 ChatGPT 帳號、付費會員、伺服器或雲端資料庫。網頁與所需程式資源由自己的 GitHub Pages 提供。

## 使用

開網站後新增貨物，使用立體、俯視及側視排列。支援同類網綑、管綑、點焊網堆疊，以及120×120公分棧板方格。配件與大門含門柱單層擺放。

- 「儲存」：儲存在同一網址、同一瀏覽器的本機資料。
- 「出貨單」：開啟這台電腦已儲存的資料。
- 「匯出備份」：下載 JSON，包含所有已儲存出貨單、常用規格，以及目前畫面尚未儲存的排列。
- 「匯入備份」：讀取裝車助手備份；合併資料。同編號不同內容另存新份，保留原有資料。
- 「下載出貨圖」：產生供司機查看的PNG。

請定期匯出備份。清除瀏覽器資料、無痕模式、換瀏覽器、換網址或換電腦，可能無法開啟原本的本機資料。網站沒有帳號保護，使用同一電腦瀏覽器的人能看到已儲存的資料。出貨單不會上傳GitHub。

舊 Sites 網站的雲端出貨单不會自動轉入此版本。需要另外從舊版匯出並轉換資料，或重新建立。

## GitHub Pages 發布

提供其他AI讀取的入口為`ai.html`，說明為`AI_README.md`，完整程式可直接讀取`ai-source.json`與`ai-manifest.json`，每個檔案也提供獨立`.txt`網址，不必解壓ZIP。建置後執行`node scripts/package-desktop.mjs`與`node scripts/package-ai-handoff.mjs`，部署`publish/`。

建立專用儲存庫，例如 `neipu-cargo-desktop`，將本專案上傳至main分支。在 Settings → Pages → Build and deployment 將 Source 設為 GitHub Actions。專案包含自動測試、建置與Pages發布流程。

使用相對資源路徑，支援 `https://你的帳號.github.io/neipu-cargo-desktop/`。只有Pages部署成功回傳的網址才是正式網址。

GitHub Pages 網站一般可由他人開啟，私密程式儲存庫不代表網站私密；依方案另有存取控制限制。備份JSON包含出貨資料，不應提交到公開程式儲存庫。

## 開發

Node.js 22.13以上，執行 `npm ci`、`npm test`、`npm run build`。本機預覽使用 `npm run dev`。建置結果在 `dist/`，可放在其他靜態網站主機。

3D排列依包裝外框估算，請包含墊木並預留綁帶及裝卸間隙；實際承載與固定由負責人和司機確認。
