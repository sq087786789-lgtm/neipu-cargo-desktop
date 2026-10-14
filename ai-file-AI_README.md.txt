# 內埔裝車助手：給其他 AI 的重建說明

這是公開的靜態網站與完整程式交接資料。用途是让另一個具備網頁讀取、檔案寫入及程式建置能力的 AI，下載同一套程式並重建工具。純聊天或不支援瀏覽、程式執行的 AI，仍需要人協助下載或建置；網站不會自動取得任何 AI 的控制權。

正式網站：https://sq087786789-lgtm.github.io/neipu-cargo-desktop/

人閱讀的移交頁：https://sq087786789-lgtm.github.io/neipu-cargo-desktop/ai.html

本說明的純文字網址：https://sq087786789-lgtm.github.io/neipu-cargo-desktop/AI_README.md

完整原始程式清單：https://sq087786789-lgtm.github.io/neipu-cargo-desktop/ai-manifest.json

所有原始程式的 JSON 檔：https://sq087786789-lgtm.github.io/neipu-cargo-desktop/ai-source.json

最新版原始程式 ZIP：請在 https://sq087786789-lgtm.github.io/neipu-cargo-desktop/ai.html 點「下載原始程式ZIP」。重建以 ai-manifest.json／ai-source.json 為準。

GitHub：https://github.com/sq087786789-lgtm/neipu-cargo-desktop

## 直接重建相同工具

1. 讀取 ai-manifest.json，其中每筆包含原始相對路徑、文字讀取網址、SHA-256及UTF-8位元組數。按每筆的path還原檔案階層；text_url 指向可直接閱讀的純文字檔，不需解ZIP。也可讀取 ai-source.json：files陣列每筆含path及完整content，按路徑寫入檔案。
2. 使用 Node.js 22.13以上（建議22或24），執行 `npm ci` 安裝package-lock.json鎖定的套件。
3. 執行 `npm test`，確認排列、碰撞、支撐與備份測試通過。
4. 執行 `npm run build`。`dist/` 是可部署的靜態網站，無伺服器與資料庫需求。
5. 若要一起提供本交接頁，執行 `node scripts/package-desktop.mjs`，再執行 `node scripts/package-ai-handoff.mjs`；把 `publish/` 放到靜態主機。這會產生內嵌JS/CSS的index.html、AI文件與原始程式讀取檔。source.zip是另外提供的便捷下載檔，重建不依賴它。
6. 建置結果使用相對路徑，能放在網站子目錄；使用HTTPS主機開啟。下載程式不代表取得正式GitHub網站的修改權；只更新自己管理的新副本，原正式網站更新需要擁有者授權。

## 必須保留的真實用途與規則

- 主要在電腦使用的內埔菱形網、管材装車工具。是真正的3D排列，可拖曳、轉向、切換立體／俯視／側視，並顯示尺寸、重量待填與超出空間提示。
- 台尺：1尺＝30.3公分。菱形網單捲直徑30公分；標準10捲綑寬120、高90公分，長度依台尺設定；零散2–9捲、長3–12台尺，寬高使用實際打包量測。
- 鍍鋅管長200／300／600公分，可自訂。整綑常見寬60–80、高40–60公分。
- 配件預設30×20×40公分；大門含門柱一起包，尺寸以整組量測。
- 網綑、管綑與點焊網同類堆疊；完整支撐、碰撞與移動整個堆疊由lib/cargo.ts處理。配件、大門只單層。上層不能懸空或穿過其他貨物。
- 地板方格預設120×120公分棧板占地，可切換50×50或自訂長寬。拖曳位置仍使用5公分吸附，方格不代表新增實體棧板。
- 本機儲存出貨單、常用規格。下載PNG供司機看；JSON備份包含目前未儲存排列、已儲存出貨單與常用規格，支援合併匯入。

## 關鍵檔案

- app/cargo-app.tsx：貨物表單、出貨單、備份、圖像匯出與操作介面。
- app/cargo-scene.tsx：Three.js、OrbitControls、3D畫面與拖曳。
- lib/cargo.ts：幾何、碰撞、支撐、堆疊與尺寸統計。
- lib/local-records.ts：資料驗證、本機儲存、備份匯出及合併匯入。
- scripts/check-cargo.mjs、scripts/check-local-storage.mjs：關鍵行為測試。
- index.html、main.tsx、vite.config.ts：靜態網站入口與建置設定。

## 出貨資料與程式分開

公開資料只含程式與示範貨物，沒有使用者的實際出貨單、密碼、API金鑰。出貨單存在原電腦瀏覽器的localStorage；換帳號、電腦、瀏覽器或網址不會自動同步。要轉移實際排列，使用者需自行匯出JSON備份，再在新副本匯入；備份可能含取貨／送貨資訊，不要提交到公開程式儲存庫。

匯出格式 `neipu-cargo-backup`，version 1；匯入需保持既有验证，錯誤資料不能覆寫原有資料。清除瀏覽器資料前先匯出備份。

保留第三方套件授權及vendor內授權檔。裝載依包裝外框估算，實際載重、固定、裝卸方式需由業主和司機確認。

## 貨物尺寸標示

立體圖預設在每件貨物上方標示編號、種類與打包後外框長×寬×高（cm），支援全部、僅選取或關閉。尺寸標籤採透明背景，不畫底框，只保留文字、細文字描邊與貨物指引線，避免遮住貨物和車廂。尺寸是貨物原始包裝尺寸，轉向不交換顯示數字；空間佔位仍依旋轉後尺寸計算。修改尺寸、移動或堆疊後標示同步，PNG 截圖包含當前標示。實際資料及儲存格式不變。
