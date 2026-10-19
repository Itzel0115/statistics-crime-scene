# Statistics Crime Scene｜統計犯罪現場

繁體中文點擊式偵探遊戲。數據可能是真的，結論卻未必：翻開三宗疑案，找出報告省略的背景，以證據支持結案。

## 啟動

遊戲入口為根目錄 `index.html`，CSS 與 JavaScript 全部內嵌。無 build、npm、後端、API Key、遠端字型或 CDN；手寫字型隨資產自託管。一般瀏覽器可直接開啟 `index.html`；建議透過靜態 HTTP 伺服器遊玩，以取得穩定的瀏覽器存檔來源。

有 Python 時，在專案目錄執行：

```powershell
python -m http.server 8000 --bind 127.0.0.1
```

開啟 `http://127.0.0.1:8000/`。若電腦上的 `python` 指向商店捷徑，請使用真正的 Python 執行檔，或用本專案測試時使用的 Deno：

```powershell
deno run --allow-net=127.0.0.1:8765 --allow-read=. tests/serve.ts
```

開啟 `http://127.0.0.1:8765/`。Deno 只供開發測試，部署後的遊戲不需要它。

## 遊玩方式

1. 接受委託，在木桌上點選任一案卷。三關不必依序進行。
2. 記錄初始判斷，再翻閱第一份證物。初始判斷不計對錯。
3. 檢查備忘錄附件、名冊遮住的欄位、研究摘要的實驗編號，取得後續證物。
4. 使用文件下方的「選取證物，加入分析桌」，再點桌上的分析工作台。
5. 主動切換統計單位、拆分題組與調整權重、比較多重檢定門檻。
6. 點打字機，選結論與支持證據。答錯會提供方向性提示，可繼續修正。
7. 正確結案後取得徽章，閱讀調查前後的理解對照。

筆記本會自動記錄取得的證物及徽章。「重新調查此案」只重設該案的探索紀錄，保留所有已獲徽章。進度保存在目前瀏覽器的 `localStorage`，不會跨裝置同步。損壞或不相容的存檔會安全回退；儲存被停用時會顯示提示，仍可完成本次遊戲。

鍵盤：Tab 移動、Enter／空白鍵啟動按鈕、Esc 關閉文件；滑桿可使用方向鍵。對話框限制焦點於目前文件並鎖定背景操作。手機使用點擊與文件捲動，不依賴拖曳或滑鼠懸停。支援 `prefers-reduced-motion`。

第二版把文字與動態圖表放回原始案卷、分析板和報告紙的淨空範圍。手機以「左頁・證物／右頁・委託」按鈕切換案卷，初步意見留在右頁底部；正文較長時在紙內捲動。結案頁的返回與重查按鈕固定可見。劇情揭露順序及畫面校準記錄見 [第二輪修訂報告](REVISION_V2_REPORT.md)。

第三版補齊三案的數據定義與調查動機：右頁保留短版，點「閱讀完整委託／新聞」展開可捲動前情，Esc 返回閱讀按鈕。CASE 001 是政大校園的虛構教學情境，非真實校務紀錄。大螢幕分析板與 SVG 依可用紙面配置，紅線連接實際圖釘中心。局長批註與私人信件改用本地手寫字；正式表格維持印刷字。三尺寸對照與驗證見 [第三輪修訂報告](REVISION_V3_REPORT.md)。

## 三宗案件

| 案件 | 分析操作與正確數據 | 統計概念 |
| --- | --- | --- |
| 三倍投訴的販賣機 | 客訴 24／72，交易 1,200／7,200；客訴率 2%／1%，每千筆 20／10 | 分母、標準化；變成 300% 是增加 200%，不能據此推論整體品質改善的因果原因 |
| 消失的冠軍學院 | 總榜 A 80%、B 55%；基礎 90%／95%、高階 40%／45%；共同基礎權重 50% 時為 65%／70% | 辛普森悖論、加權平均；不能從觀察性通過率推論教學因果優劣 |
| 被挑選的奇蹟實驗 | A p=0.033；共 12 個模式；單次 α=0.05 一筆顯著，Bonferroni α≈0.00417 零筆顯著 | 多重檢定、選擇性報告；證據不足不等於無效，應用新獨立樣本進行預先指定的確認性實驗 |

正式數據集中於 `index.html` 的 `CASE_DATA`。圖表使用 SVG，按鈕及滑桿實際改變計算與視覺化。第三案的 p 值橫軸為對數刻度，圖內保留原始值，門檻與顯著數量依數值計算。45.96% 的假陽性機率教學僅限 12 次檢定獨立、所有虛無假設皆真等條件，不是產品無效的機率。

## 資產與檔案

```text
index.html                 單一遊戲入口（內嵌 CSS / JavaScript / SVG）
asset/
  background/              原始桌面背景
  props/                   原始案卷、筆記本、放大鏡、打字機等
  ui/                      原始標題、印章、徽章
  templates/               原始空白紙張與分析板
  case001/ ～ case003/      原始附加案例圖；保留但不作遊戲證據
  optimized/               26 張另外產生的 WebP，約 7.30 MB
  fonts/                   自託管手寫字型、OFL 授權及來源雜湊
screenshots/               實際瀏覽器截圖，非遊戲執行所需
tests/                     資產盤點、瀏覽器測試、測試結果
ASSET_INVENTORY.md         素材使用與排除說明
TEST_REPORT.md             實際驗證項目及限制
submission_notes.md        NTU COOL 繳交說明草稿
CHANGELOG.md               本次實際開發與修正紀錄
```

原始 38 張 PNG 完整保留。WebP 為獨立衍生版本：最長寬度限制為 1,400px、品質 86；未覆寫原始圖片。`<img>` 若優化版本失敗，會依正確資料夾回退原始 PNG；CSS 紙張與背景使用優化資產，因此部署時應保留整個 `asset/`。

手寫字型取自 [芫荽 Iansui 官方專案](https://github.com/ButTaiwan/iansui)，依 SIL OFL 1.1 製作必要字集並改名為 Crime Scene Notes。檔案 `asset/fonts/CrimeSceneNotes.woff2` 為 271,396 bytes，使用 `font-display: swap`；完整授權在 [OFL.txt](asset/fonts/OFL.txt)，來源與雜湊在 [SOURCE.json](asset/fonts/SOURCE.json)。正式遊戲不連線下載字型。新增手寫文字時應重新建立字集；開發用來源下載及子集腳本為 `tests/v3-download-font.ts`、`tests/subset-font.py`（Python fontTools 與 Brotli），不是執行遊戲的相依套件。

## GitHub Pages 部署

1. 建立 GitHub 儲存庫（一般免費帳號使用公開儲存庫），將 `index.html`、完整 `asset/` 與 `.nojekyll` 放在儲存庫根目錄。其他 Markdown 與截圖可一起提交；不要上傳測試瀏覽器的設定檔。
2. 推送至 `main` 分支。
3. 在儲存庫的 **Settings → Pages → Build and deployment**，Source 選 **Deploy from a branch**，分支選 **main**，資料夾選 **/(root)**，按 **Save**。
4. 等 Pages 部署完成，使用設定頁顯示的實際網址，例如 `https://你的帳號.github.io/statistics-crime-scene/`。網址只是格式範例；本次未代為發布儲存庫。
5. 用該網址確認三案入口與圖片，將實際網址填入 `submission_notes.md`。

此流程依 [GitHub 官方：設定 Pages 發布來源](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site) 整理。所有遊戲資產皆使用 `./asset/...`，已在本機 `/statistics-crime-scene/` 子路徑驗證。

## 開發測試

歷次紀錄見 `TEST_REPORT.md`；目前第三版驗證見 `REVISION_V3_REPORT.md`。自動化採 Deno 原生 WebSocket 連接 Edge DevTools Protocol，透過實際滑鼠事件走流程，不需安裝 Playwright；權重滑桿使用原生鍵盤事件操作。

```powershell
deno run --allow-read=index.html,asset,tests/asset-inventory.json --allow-write=tests/static-results.json --allow-net=127.0.0.1:8765 tests/static-check.ts
deno run --allow-read=index.html,asset/fonts,tests/baselines --allow-write=tests/v3-static-results.json --allow-net=127.0.0.1:8765 tests/v3-static-check.ts
deno run --allow-net=127.0.0.1:9223 --allow-read=tests/v2-baseline.json --allow-write=screenshots,tests/v3-browser-results.json tests/v3-browser-test.ts
deno run --allow-net=127.0.0.1:9223 --allow-write=tests/v3-resilience-results.json tests/v3-resilience-test.ts
deno run --allow-net=127.0.0.1:9223 --allow-write=screenshots,tests/v3-visual-results.json tests/v3-visual-test.ts
```

後三個指令需先用獨立測試設定檔啟動 Chromium／Edge，並開啟 `--remote-debugging-port=9223` 及遊戲頁，同時執行上述 8765 靜態伺服器。測試會清除獨立設定檔中本機測試來源的遊戲存檔，請勿在自己的遊玩設定檔執行。測試與截圖不會在正式遊戲載入；舊版測試及結果保留作歷史紀錄。修訂前完整 V2 原始碼保存在 `tests/baselines/index-v2.html`，須放回專案根目錄才能以原相對路徑執行。
