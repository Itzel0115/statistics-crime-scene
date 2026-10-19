# Statistics Crime Scene 第三輪修訂與驗證

日期：2026-10-19（Asia/Taipei）。本輪依 `CODEX_REVISION_V3.md` 修改現有 V2；先保存 [V2 原始碼](tests/baselines/index-v2.html) 與實際瀏覽器截圖，再完成 CASE 001 樣板、通關及三尺寸 34 項檢查，才擴展另外兩案。V2 的故事與測試以既有程式、[V2 報告](REVISION_V2_REPORT.md)、測試 JSON 及先前已讀的故事內容對照；本輪磁碟上的 `STORY_COPY_V2.md`、`CODEX_REVISION_V2.md` 已不存在，未擅自重建。

## 故事節點

| 案件 | 接案與數據定義 | 揭露順序與具體處置 |
| --- | --- | --- |
| CASE 001 | 政大校園多處販賣機的同一套營運服務，兩個完整七日週的購物客訴 24／72 件。詳細委託解釋付款未出貨、重複扣款、退款爭議；件數不是故障機台或學生人數；每筆交易的重複申訴已合併。右頁與完整文件皆標示「情節與數據均屬虛構，非政大真實校務紀錄。」 | 新聞引發撤機與合約協調，匿名稽核信封帶出交易明細；前情不透露 1,200／7,200。數據週期統一為第 1 週／第 2 週。局長、初步建議、證物用語與結案配合改寫；結案暫緩單憑總件數撤機，要求標準化資料再議合約，沒有把比例下降寫成所有問題消失。 |
| CASE 002 | 初次出現即定義能力測驗通過率：達所應試題組及格標準的人數／實際應試人數。長版交代每人只考一組，同題組跨校同試卷、同及格標準；不是錄取率、畢業率或升學率。 | 星橋 A 列入尚未核定的補助優先推薦草案；晨星 B 異議與匿名信促成調查。前情不透露兩校題組人數或分組答案。仍須展開名冊、取得配置表、比較共同權重；結案暫緩核定，併列總榜、分組與組成後再議政策。 |
| CASE 003 | NeuroWave 上市前委託核對 Focus-X 廣告。第一次看到 +5.3 分與 p=0.033 時即知道是記憶測驗平均分數比較；長版補充兩組獨立受試者。 | FX-A 仍在摘要右上角，必須追查後才展開完整日誌；前情不透露 12 項研究。後續通訊、門檻比較、退回過度宣稱與新的確認性研究保留。 |

短版敘事分別為 115、112、128 字元（含標點及拉丁文字）；固定「閱讀完整委託／新聞」入口開啟長版紙張文件。手機短版區域仍可在紙內捲動，初步意見與閱讀入口不隨正文捲走；長版用滑鼠滾輪／觸控捲動，Esc 返回原閱讀按鈕。沒有新增關卡或更動第一印象的存檔三態。

## CSS 圖層與 SVG

- `.case-brief`、`.case-copy` 保留 V2 的透明紙面與原案卷比例；新增 `.case-reading` 與 `.fiction-note`，把長版前情移入 `commission()`，沒有重新蓋上不透明 HTML 底板。
- `.modal-layer` 以固定視窗 grid 置中；`.document.analysis` 依視窗寬、高與原分析板比例決定尺寸，上限 1500px。`.document-body` 的可用紙面高度微調為 60%，主圖區 flex 配置，旁欄 grid 配置。
- `.analysis-main` 的圖表填滿可用高度，數值輸出留在圖下；CASE 002 的 `#weight-controls` 移至旁欄，CASE 003 的軸說明移至旁欄。手機延續原紙張邊框與紙內捲動，滑桿、返回按鈕仍可到達。
- `resizeChart()` 依 SVG 實際 CSS 尺寸重算 viewBox、柱寬、格線、座標與標籤。`resizePChart()` 重算 12 列、對數軸、門檻線與數值欄；`pX()` 使用相同繪圖範圍。文字沒有隨圖片任意拉伸；模式與權重仍使用原統計函式計算。
- `.evidence-pin` 是可量測的實際錨點。`drawConnections()` 取圖釘與 SVG 的 `getBoundingClientRect()`，將中心換算為 SVG 座標。SVG 設 `pointer-events:none`，不攔截操作；單一證物不畫線。ResizeObserver、MutationObserver、視窗 resize、字型載入完成及動畫完成均排程更新；關閉文件時解除觀察器。

### 實測尺寸

原始量測：[V2](tests/v3-before-metrics.json)、[V3](tests/v3-visual-results.json)。以下為 CSS px，尺寸四捨五入。

| 1920×1080 | V2 分析板寬 | V3 分析板寬 | V2 SVG 元素 | V3 SVG／viewBox | V3 有效繪圖高度 |
| --- | ---: | ---: | --- | --- | ---: |
| CASE 001 | 1360 | 1500 | 695×235 | 755×428 | 354 |
| CASE 002 | 1360 | 1500 | 695×235 | 755×428 | 354 |
| CASE 003 | 1360 | 1500 | 695×310 | 755×428 | 356 |

V2 長條圖 viewBox 固定 510×280，元素較寬時實際圖形仍縮在有限區域；V3 使用容器對應的繪圖座標，利用紙面剩餘高度。**本機實測 V2 分析板 DOM 外框原本已水平置中**，因此沒有宣稱修正一個量不到的外框偏移。V3 明確由視窗置中，三尺寸中心誤差小於 2px，並改善內容分配造成的視覺失衡。

1440×900 時板寬約 1295px，SVG 約 617×327–351px；所有桌面圖表、輸出、旁欄返回與權重控制均位於紙內。390×844 時板寬 378px，長條 SVG 約 306×240px、p 值 SVG 約 306×340px；旁欄接在圖表下方，以紙內垂直捲動操作，沒有水平溢出。

紅線共量測 14 組：三尺寸各三案、1920→1440→390→1920 的連續 resize、證物多行換行及字型完成事件。以 SVG 路徑兩端轉換到螢幕座標，比對實際圖釘中心，最大誤差 **0.000 CSS px**；同時驗證路徑數為圖釘數減一（單一圖釘為零）。

## 手寫字型與授權

取自 [芫荽 Iansui 官方專案](https://github.com/ButTaiwan/iansui)，採 SIL Open Font License 1.1；保留原字形輪廓，建立必要字集並將衍生家族改名為 Crime Scene Notes。自託管檔為 [CrimeSceneNotes.woff2](asset/fonts/CrimeSceneNotes.woff2)，271,396 bytes、979 字元；完整 [OFL.txt](asset/fonts/OFL.txt) 與 [來源／SHA-256](asset/fonts/SOURCE.json) 一併保留。

`@font-face` 使用相對路徑及 `font-display:swap`。局長批註為 20–22px，匿名信、私人稽核來函、局長提示與分析旁註使用手寫字；正式公文、資料表、按鈕、SVG 標籤及研究部正式電子通訊維持印刷字。瀏覽器 `CSS.getPlatformFontsForNode` 的 15 筆證據確認實際出字使用 custom font Crime Scene Notes，不只檢查 CSS 字型名稱；結果保存在視覺測試 JSON 的 `fonts`。

## 測試結果

環境為 Windows、Microsoft Edge 155.0.4283.62 無頭瀏覽器、Deno 透過原生 CDP；獨立測試設定檔。遊戲於本機 `/statistics-crime-scene/` 子路徑執行。尺寸為 **1440×900、1920×1080、390×844**。

| 測試 | 結果 | 證據 |
| --- | --- | --- |
| 先行 CASE 001 樣板 | 34／34 | [腳本](tests/v3-case001.ts)、[結果](tests/v3-stage1-results.json) |
| 三尺寸、每尺寸完整三案流程 | 264／264 | [腳本](tests/v3-browser-test.ts)、[結果](tests/v3-browser-results.json) |
| localStorage 被拒、調查門檻、Tab／Shift+Tab／Esc、桌面工具 | 11／11 | [腳本](tests/v3-resilience-test.ts)、[結果](tests/v3-resilience-results.json) |
| 故事定義、揭露、紙內滾輪、焦點返回、SVG 邊界、紅線、實際字型 | 119／119 | [腳本](tests/v3-visual-test.ts)、[結果與量測](tests/v3-visual-results.json) |
| 原數值／依賴／正解鍵／徽章、字型格式／雜湊／授權／子路徑 | 7／7 | [腳本](tests/v3-static-check.ts)、[結果](tests/v3-static-results.json) |
| 原圖與靜態相容性 | 38 張 PNG 雜湊不變；26 張 WebP 可載入；內嵌 JS 可解析；無遠端執行相依 | [結果](tests/static-results.json) |

全流程以瀏覽器原生滑鼠事件點擊，權重滑桿用原生鍵盤事件操作，涵蓋原始／校正模式切換、0／50／100 權重、錯誤結論與錯誤證據不能通關、三案徽章、重整保存、舊版進行中存檔恢復、重查單案、損壞存檔與減少動態效果。視覺專項使用指定進度 fixture 快速展開各狀態，與完整通關測試分開記錄；容錯測試以程式點擊模擬異常狀態。最後的短註文字與焦點返回修正，另重跑 119 項專項、11 項容錯及靜態檢查。

CASE_DATA 的顯示標籤與文案有修改，**不能稱整個物件逐字未變**；數值輸入、證物 ID／依賴、結案正解鍵、徽章及三案數量均與 V2 備份比對相同。原統計結果維持 2%／1%、每千筆 20／10、增加 200%；A／B 總榜 80%／55%、共同權重 65%／70%；p=0.033、12 次 Bonferroni 門檻約 0.00417。全流程與專項沒有捕捉到未處理 JavaScript 例外或遊戲資產 HTTP 錯誤。

## 同尺寸 V2／V3 截圖

每個連結均為原尺寸瀏覽器 PNG；修改前與修改後使用同一視窗、同一案件、同一分析模式。三案 × 三尺寸 × 案卷／分析，共 **18 組對照**。不是設計稿或重新生成的素材。

| 尺寸／案件 | 案卷 V2 | 案卷 V3 | 分析 V2 | 分析 V3 |
| --- | --- | --- | --- | --- |
| 1440×900／001 | [V2](screenshots/v3-before-1440-case001-file.png) | [V3](screenshots/v3-after-1440-case001-file.png) | [V2](screenshots/v3-before-1440-case001-analysis.png) | [V3](screenshots/v3-after-1440-case001-analysis.png) |
| 1440×900／002 | [V2](screenshots/v3-before-1440-case002-file.png) | [V3](screenshots/v3-after-1440-case002-file.png) | [V2](screenshots/v3-before-1440-case002-analysis.png) | [V3](screenshots/v3-after-1440-case002-analysis.png) |
| 1440×900／003 | [V2](screenshots/v3-before-1440-case003-file.png) | [V3](screenshots/v3-after-1440-case003-file.png) | [V2](screenshots/v3-before-1440-case003-analysis.png) | [V3](screenshots/v3-after-1440-case003-analysis.png) |
| 1920×1080／001 | [V2](screenshots/v3-before-1920-case001-file.png) | [V3](screenshots/v3-after-1920-case001-file.png) | [V2](screenshots/v3-before-1920-case001-analysis.png) | [V3](screenshots/v3-after-1920-case001-analysis.png) |
| 1920×1080／002 | [V2](screenshots/v3-before-1920-case002-file.png) | [V3](screenshots/v3-after-1920-case002-file.png) | [V2](screenshots/v3-before-1920-case002-analysis.png) | [V3](screenshots/v3-after-1920-case002-analysis.png) |
| 1920×1080／003 | [V2](screenshots/v3-before-1920-case003-file.png) | [V3](screenshots/v3-after-1920-case003-file.png) | [V2](screenshots/v3-before-1920-case003-analysis.png) | [V3](screenshots/v3-after-1920-case003-analysis.png) |
| 390×844／001 | [V2](screenshots/v3-before-mobile-case001-file.png) | [V3](screenshots/v3-after-mobile-case001-file.png) | [V2](screenshots/v3-before-mobile-case001-analysis.png) | [V3](screenshots/v3-after-mobile-case001-analysis.png) |
| 390×844／002 | [V2](screenshots/v3-before-mobile-case002-file.png) | [V3](screenshots/v3-after-mobile-case002-file.png) | [V2](screenshots/v3-before-mobile-case002-analysis.png) | [V3](screenshots/v3-after-mobile-case002-analysis.png) |
| 390×844／003 | [V2](screenshots/v3-before-mobile-case003-file.png) | [V3](screenshots/v3-after-mobile-case003-file.png) | [V2](screenshots/v3-before-mobile-case003-analysis.png) | [V3](screenshots/v3-after-mobile-case003-analysis.png) |

補充狀態：[完整委託](screenshots/v3-1920-case001-commission.png)、[局長批註](screenshots/v3-1440-case001-director.png)、[私人來函](screenshots/v3-1920-case001-letter.png)、[手機匿名信](screenshots/v3-mobile-case002-letter.png)、[完整日誌](screenshots/v3-1920-case003-log.png)、[換行後紅線](screenshots/v3-redline-reflow.png)。完整通關截圖另以 `v3-`、`v3-1920-`、`v3-mobile-` 開頭；樣板截圖以 `v3-stage1-` 開頭。V2 截圖與歷史測試結果保留。

## 驗收範圍與限制

已實際檢視瀏覽器截圖並執行上述互動與幾何量測。390×844 為 Edge 手機尺寸模擬，尚未在實體 iOS／Android、Safari、Firefox 或螢幕閱讀器人工驗收；不同系統後備印刷字可能有微小排版差異。GitHub Pages 相容性驗證為本機專案子路徑，沒有發布遠端網站。首次離線造訪、瀏覽器清除快取後的離線啟動不在測試範圍；遊戲本身無遠端 API／CDN 相依。

沒有新增第四關、重新生成圖像或更動原 PNG；未改變統計結論、三關通關規則、徽章及 localStorage 存檔格式。
