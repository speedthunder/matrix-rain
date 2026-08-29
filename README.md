# ASCII 字元雨（Matrix Rain）

單一 HTML 檔的《駭客任務》風格字元瀑布特效，用 Canvas 2D 繪製，無任何外部依賴。
可即時調整速度、字級、密度、殘影、顏色，並讓自訂文字以「瀑布顯字」的方式浮現在字碼流中。

**線上展示：** https://speedthunder.github.io/matrix-rain/

![預覽](preview.jpg)

## 功能

- **字碼雨**：可列印 ASCII（`! " # … 0-9 … A-Z … a-z … ~`，共 94 字）或純 `0/1` 兩種字元集切換。
- **文字瀑布顯字**：輸入一段文字，畫面會以一道道往下流的字碼把它掃出來——方向、速度、顏色都與背景雨同步，沒有靜止的浮水印。
- **即時控制面板**（右上角「⚙ 設定」）：
  | 控制 | 說明 |
  |------|------|
  | 下落速度 | 每幀往下移動的字數（同時驅動背景雨與顯字掃描） |
  | 字體大小 | 背景字碼的字級 |
  | 欄位密度 | 欄寬 = 字級 × 此值，越小越密 |
  | 殘影長度 | 拖尾長度（值越小尾巴越長） |
  | 亮點色 / 一般色 | 字碼配色，顯字文字會跟著同步變色 |
  | 播放動畫 | 暫停 / 繼續 |
  | ASCII 字元 | 開＝完整 ASCII，關＝純 0/1 |
  | 顯示文字 + 文字大小 | 輸入文字、拉桿調整字級（60–800px），即時重建 |
  | 重置為預設值 | 全部還原 |

- **無障礙**：尊重 `prefers-reduced-motion`（開啟時預設暫停、動畫縮到最短）；面板控制項皆有 `label` 關聯與 `:focus-visible` 樣式。

## 使用方式

### 線上
直接開 https://speedthunder.github.io/matrix-rain/ 。

### 本機
因為是純靜態單檔，任選一種：

```bash
# 方式 A：直接用瀏覽器開啟
#   雙擊 index.html 即可

# 方式 B：起一個本機伺服器（部分瀏覽器對 file:// 較嚴格時）
python -m http.server 8000
#   然後開 http://localhost:8000/
```

## 部署到 GitHub Pages

1. 把 `index.html`（與 `preview.jpg`、`README.md`）推到 repo。
2. repo → **Settings → Pages** → Source 選 **Deploy from a branch**，branch 選 `main` / 根目錄 `/`。
3. 等一兩分鐘，網址即 `https://<帳號>.github.io/<repo 名>/`。

`index.html` 位於根目錄，Pages 會自動當首頁載入。

## 技術重點

- 背景雨：每欄一個下落位置，每幀畫一個隨機字元，靠每幀鋪一層半透明黑（`trailAlpha`）產生拖尾。
- 顯字：離屏 canvas 把文字畫大 → 讀 alpha 通道 → 依格線取樣成遮罩座標集合；繪製時用一道相位錯開（`col × 2.4`）的對角掃描，只在掃描頭經過的格子畫字，字級／掃描寬度隨文字高度自適應。
- 動畫節流交給 `requestAnimationFrame`，速度以「每幀字數」表示。

## 授權

MIT
