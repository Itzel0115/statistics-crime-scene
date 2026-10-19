# 資產盤點

開發開始時，根目錄只有 `asset/`，沒有既有 HTML、套件或應用程式。

共發現 **38 張 PNG**：背景 1、道具 14、介面 6、模板 5、附加案例圖 12。詳細實際路徑、尺寸、檔案大小與原始 SHA-256 記於 `tests/asset-inventory.json`；接觸表預覽為 `tests/asset-contact-sheet.jpg`。

## 正式採用

- `background/desk_main_bg.png`：主場景背景。
- `props/casefile_001_closed.png` 至 `casefile_003_closed.png`：可點擊案件袋。
- `props/casefile_001_open.png` 至 `casefile_003_open.png`：開啟的案卷框架。
- `props/notebook_closed.png`、`notebook_open.png`：筆記入口與收藏畫面。
- `props/magnifier.png`：桌面調查指南的實體道具；名冊另有可操作的放大鏡檢查按鈕。
- `props/typewriter_idle.png`、`typewriter_with_paper.png`：結案入口與判決畫面。
- `ui/title_statistics_crime_scene.png`：開場標題。
- `ui/stamp_evidence_found.png`、`stamp_case_closed.png`：發現證物與結案蓋章。
- `ui/badge_case001.png`、`badge_case002.png`、`badge_case003.png`：三案徽章。
- `templates/template_newspaper.png`、`template_report.png`、`template_memo.png`、`template_data_sheet.png`、`template_analysis_board.png`：公開報導、文件材質及分析桌。

主程式載入上述素材的 `asset/optimized/同名.webp` 衍生版。共用素材均產生對應的 WebP，包含未在主場景放置的咖啡杯、證物標籤及紅線束；沒有假互動熱區。分析桌的紅線由 SVG 依證物 DOM 位置繪製。

第二輪調整：分析畫面統一使用 `template_analysis_board.webp`；公開報導、筆記收藏與結案以 `template_report.webp` 呈現，短便條使用 `template_memo.webp`。`notebook_open`、`template_newspaper`、`template_data_sheet` 保留資產，但目前畫面不使用；較早 CSS 的宣告由 V2 樣式覆蓋。手機分析板以同一原圖的 CSS 九宮格邊框延展，沒有新增點陣圖片或覆蓋用紙板。三張案卷仍完整等比例顯示，手機在局部視窗切換左右頁。

## 保留但不作正式證據

附加案例圖片含有既有固定文字與數字，遊戲完全不載入 `asset/case001/`、`asset/case002/`、`asset/case003/`。

- 案例一實際檔名為 `case001_newspape.png`，不是推測的 `case001_newspaper.png`。交易紀錄圖片可見「18」等不符指定資料的內容。
- 案例二配置圖片呈現的分組結構與本案兩個難度組不同，不作數據來源。
- 案例三研究摘要圖片帶有不符規格的 p 值，內部備忘錄也有與本案不符的日期與文字。
- 上述圖片只保留在素材目錄，沒有裁剪或將部分文字混入正式證據。

所有可採信的文字、表格、數字和圖表都來自 HTML／SVG 及單一 `CASE_DATA`，不從圖片辨識或擷取。

## 原檔保存與最佳化

以 FFmpeg 將 26 張共用 PNG 另存成 WebP，寬度上限 1,400px、品質 86、保留透明度。優化後合計 7,295,598 bytes。原始 PNG 無覆寫、無刪除；靜態檢查會用開發前的 SHA-256 逐一比對。
