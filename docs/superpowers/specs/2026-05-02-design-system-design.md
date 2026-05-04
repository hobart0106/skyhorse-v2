# SkyHorse Design System — 設計規格

**日期：** 2026-05-02  
**狀態：** 待實作  
**範圍：** 方案 B — Token 系統 + 核心元件庫 + 展示頁  
**目標平台：** 純 HTML/CSS（最終轉 WordPress Classic Theme）

---

## 1. 交付物

```
design-system.css            # 全部 token + 元件規則
design-system-preview.html   # 所有元件視覺展示頁
```

頁面只需一個 `<link rel="stylesheet" href="design-system.css">` 即可使用完整設計系統。WordPress 開發時透過 `wp_enqueue_style()` 載入，PHP 模板輸出符合 class 規範的 HTML。

---

## 2. 色彩 Token 架構

### 三層設計

```
Layer 1 — Primitive   原始色值，不直接用於元件
Layer 2 — Semantic    有意義的角色名，theme 切換在這層  ← 元件引用此層
Layer 3 — Component   元件只引用 semantic token
```

### Theme 切換機制

```css
/* 預設：尊重系統設定 */
@media (prefers-color-scheme: dark)  { :root { /* dark tokens */ } }
@media (prefers-color-scheme: light) { :root { /* light tokens */ } }

/* 用戶手動覆蓋 */
[data-theme="dark"]  { /* dark tokens */ }
[data-theme="light"] { /* light tokens */ }
```

### Semantic Token 對照表

| Token | Dark Mode | Light Mode | 用途 |
|-------|-----------|------------|------|
| **背景** | | | |
| `--color-bg-base` | `#060606` | `#F7F5F0` | 頁面底色 |
| `--color-bg-surface` | `#0e0e0e` | `#F0EDE6` | Section 背景 |
| `--color-bg-elevated` | `#161616` | `#E8E4DC` | Input、hover 背景 |
| `--color-bg-card` | `#111111` | `#FFFFFF` | Card 底色 |
| **文字** | | | |
| `--color-ink-primary` | `#F2F2F2` | `#111111` | 主要文字 |
| `--color-ink-secondary` | `#8A8A8A` | `#555555` | 副標、說明文字 |
| `--color-ink-tertiary` | `#4A4A4A` | `#999999` | 時間、meta、placeholder |
| **品牌 / 強調** | | | |
| `--color-accent` | `#FFDC22` | `#D4A800` | CTA、eyebrow（light 加深確保 WCAG 對比） |
| `--color-accent-dim` | `rgba(255,220,34,.12)` | `rgba(212,168,0,.12)` | Badge 背景、chip active |
| `--color-accent-on` | `#060606` | `#FFFFFF` | Accent 按鈕上的文字色 |
| `--color-accent-border` | `rgba(255,220,34,.4)` | `rgba(212,168,0,.4)` | Gold 邊框 |
| **邊框** | | | |
| `--color-border` | `rgba(255,255,255,.06)` | `rgba(0,0,0,.08)` | Card、input 預設邊框 |
| `--color-border-mid` | `rgba(255,255,255,.12)` | `rgba(0,0,0,.15)` | Hover 邊框 |
| **狀態** | | | |
| `--color-success` | `#34C759` | `#248A3D` | 成功狀態 |
| `--color-error` | `#FF453A` | `#C0392B` | 錯誤、警告 |
| `--color-info` | `#0A84FF` | `#0055CC` | 資訊提示 |

> **向後相容：** 現有頁面使用的舊 token 名稱以 CSS alias 保留，例如 `--gold: var(--color-accent)`、`--bg-base: var(--color-bg-base)`。這些 alias 宣告在 `:root` 最後，不影響 theme 切換。新頁面一律使用 `--color-*` 語義名稱，舊 alias 為遷移過渡期用。

---

## 3. 其他 Token（非色彩）

### 字型

| Token | 值 | 用途 |
|-------|---|------|
| `--font-display` | `'Space Grotesk', sans-serif` | 標題、按鈕、數字統計 |
| `--font-body` | `'Noto Sans TC', sans-serif` | 內文、說明、日期 |
| `--font-mono` | `'JetBrains Mono', monospace` | 機場代號、eyebrow、tag |

> **使用規則：** 所有元件的 `font-family` 一律引用 token（`font-family: var(--font-display)`），不得 hardcode 字型名稱。typography utility classes（`.font-display`、`.font-body`、`.font-mono`）提供快捷，但元件內部仍以 token 為準。

### 間距（4px 基底）

`--sp-1: 4px` / `--sp-2: 8px` / `--sp-3: 12px` / `--sp-4: 16px` / `--sp-5: 20px` / `--sp-6: 24px` / `--sp-8: 32px` / `--sp-10: 40px` / `--sp-12: 48px` / `--sp-16: 64px` / `--sp-20: 80px`

### 圓角

| Token | 值 | 用途 |
|-------|---|------|
| `--r-xs` | `4px` | Badge、小元素 |
| `--r-sm` | `6px` | Button、社交 icon |
| `--r-md` | `10px` | Input、小 card |
| `--r-lg` | `16px` | Card、modal |
| `--r-pill` | `9999px` | Chip、tag |

---

## 4. design-system.css 結構

```css
/* ═══════════════════════════════════════
   LAYER 1 — Primitive tokens
═══════════════════════════════════════ */
:root { --primitive-gold-400: #FFDC22; /* ... */ }

/* ═══════════════════════════════════════
   LAYER 2 — Semantic tokens (Dark default)
═══════════════════════════════════════ */
:root,
[data-theme="dark"] { --color-bg-base: #060606; /* ... */ }

[data-theme="light"],
@media (prefers-color-scheme: light) { --color-bg-base: #F7F5F0; /* ... */ }

/* ═══════════════════════════════════════
   LAYER 3 — Reset & base
═══════════════════════════════════════ */

/* ═══════════════════════════════════════
   LAYER 4 — Typography utilities
═══════════════════════════════════════ */

/* ═══════════════════════════════════════
   LAYER 5 — Components (依序)
═══════════════════════════════════════ */
```

---

## 5. 元件清單（18 個）

### 5.1 Navigation `.nav`
- 半透明底色 + `backdrop-filter: blur(20px)`，高度 64px
- Scroll 後顯示底部邊框 `--color-border`
- Logo（文字）+ 導航連結 + CTA 按鈕（`.btn-primary`）
- Mobile：漢堡選單按鈕，展開全寬選單

### 5.2 Button `.btn`
- **Variants：** `.btn-primary`（accent 底）、`.btn-secondary`（透明 + border）、`.btn-ghost`（最低調）
- **Sizes：** 預設（padding 12px 24px）、`.btn-sm`（padding 7px 14px，font-size 12px）
- 全部使用 `var(--font-display)`，`--r-sm` 圓角
- `.btn-primary` 文字色固定用 `--color-accent-on`（dark: #060606，light: #FFFFFF）
- Hover：primary `filter: brightness(1.05)`，secondary/ghost border 變 `--color-border-mid`

### 5.3 Vehicle Card `.car-card`
- 封面圖 + 車型 / 名稱 / 規格 + 價格 + CTA
- Hover：`translateY(-4px)` + gold glow shadow
- 圓角 `--r-lg`，底色 `--color-bg-card`

### 5.4 Chip & Tag `.chip` / `.chip-tag`
- **Chip：** 篩選器用，有 `.active` 狀態（gold border + 背景）
- **Tag：** 車輛/行程屬性標籤，pill 形，只讀
- 字型：`--font-display` 11–12px，`--r-pill`

### 5.5 Form Input `.form-group`
- 子元素：`.form-label` / `.form-input` / `.form-hint`
- Focus：`border-color: var(--color-accent)`（改變 border 顏色，不使用 outline ring；outline ring 僅用於鍵盤導覽的全域 focus style）
- Error：`border-color: var(--color-error)` + `.form-hint.error` 紅色提示
- 底色 `--color-bg-elevated`，圓角 `--r-md`

### 5.6 Badge `.badge`
- **Variants：** `.badge-gold`、`.badge-success`、`.badge-info`
- 狀態標記，可嵌入 Card 內，pill 形
- 對應 token：`--color-accent-dim` / `--color-success` / `--color-info`

### 5.7 Promotion Section `.promo-section`
- **完整版：** 兩欄 grid（1:1），左：eyebrow + headline + body + actions + 箭頭導覽；右：媒體 slot（4:3 比例，badge overlay 固定左下）
- **精簡版：** `.promo-compact`，橫幅，左文字右 CTA，gold border 提示重要性
- 媒體 slot 可放圖片、地圖、輪播

### 5.8 Article Card `.article-card`
- **Variants：**
  - 標準垂直（16:9 封面 + category / title / excerpt / date）
  - 橫向 `.article-card-h`（列表頁）
  - 精選大圖 `.article-card-featured`（2:1 封面，首頁主打）
- Hover：封面圖 `scale(1.04)` + card `translateY(-4px)` + gold glow

### 5.9 Section Header `.section-header`
- 結構：`.section-eyebrow` → `.section-title` → `.section-sub`（可選）
- **Eyebrow：** JetBrains Mono 11px，3.5px letter-spacing，`--color-accent`，前置 28px 金線
- **Title：** Space Grotesk，`clamp(30px, 4vw, 48px)`，-1.5px tracking
- **Sub：** Noto Sans TC 15px 300，max-width 480px
- **Modifiers：** `.centered`（置中）、`.compact`（margin 縮小、title 較小）
- `margin-bottom: 56px`（mobile: 36px）

### 5.10 Review Card `.review-card`
- **標準版：** 300px，quote mark（gold-dim 64px）+ text（4行截斷）+ 頭像 + 姓名 + 行程·日期
- **精選大版：** `.review-card-lg`，取代現有 `.celeb-quote-panel`，text 不截斷
- **Stars 變體：** 可用 5 顆星替換 quote mark
- Hover：`translateY(-3px)`，無 gold glow（評論卡比車輛卡低調）

### 5.11 Tour Card `.tour-card`（現有 `.ft-card` 重命名）
- 封面圖（16:9）+ badge overlay + body（tag / title / desc / highlights / footer）
- **Badge overlay：** `.tour-card__badge--hot`（gold，對應全域 `.badge-gold` 的樣式）、`.tour-card__badge--new`（white）。這些是 position:absolute 的 overlay badge，與全域 `.badge` 視覺相同但需定位，故保持獨立 class。
- **Highlights：** 使用 `.chip-tag`，限 4 個
- **Footer：** 時鐘 icon + 天數（左）、"From" + 金色價格（右）
- Hover：`translateY(-4px)` + gold glow

### 5.12 Footer `.footer`
- 4 欄 grid（`2fr 1fr 1fr 1fr`）：品牌欄 + 美國租車 + 阿拉斯加 + 聯絡我們
- 品牌欄：logo + desc + 社交 icon（36px 方形，hover gold border）
- 連結：`--color-ink-tertiary`，hover `--color-ink-primary`
- 底部版權列：左側版權，右側隱私 / 條款
- Mobile：單欄堆疊，底部置中

### 5.13 Stats Bar `.stats-bar`
- 橫排數字統計，每個 `.stat-item` 包含 `.stat-num`（大字）+ `.stat-label`
- `--font-display` 700，數字使用 `clamp`
- 可置於 hero 下方或獨立 section

### 5.14 FAQ Accordion `.faq-item`
- 點擊 `.faq-question` 展開 / 收合 `.faq-answer`
- 展開動畫：`max-height` transition
- 展開時 question 顏色變 `--color-accent`，右側箭頭旋轉 180°
- 多個頁面共用（index.html、faq.html、ak_index.html）

### 5.15 Advantage Card `.adv-card`
- Icon wrap（`--color-accent-dim` 背景）+ 標題 + 描述
- 用於「為什麼選我們」等 feature highlight 區塊
- Hover：背景微亮，`translateY(-2px)`

### 5.16 Airport Card `.airport-card`
- 機場代號（JetBrains Mono 大字，`--color-accent`）+ 城市名 + 機場全名
- 網格排列，底色 `--color-bg-card`，border `--color-border`
- Hover：border 變 `--color-border-mid`

### 5.17 Hero Booking Form `.hero-booking-card`
- 浮卡式預約表單，overlay 於 hero 圖片右側或底部
- 欄位：取車地點 / 取車日期 / 還車日期（可選異地還車）
- 送出按鈕：全寬 `.btn-primary`
- 底色 `--color-bg-card`，圓角 `--r-lg`，輕微 shadow

### 5.18 Newsletter Section `.newsletter`
- Email 訂閱橫幅：左側標題 + 副標，右側 input + 送出按鈕
- 底色 `--color-bg-surface` 或 `--color-accent-dim`
- Mobile：垂直堆疊

---

## 6. 通用互動規範

### Shadow & Easing Tokens

```css
--shadow-sm:   0 4px 12px rgba(0,0,0,.3);
--shadow-md:   0 8px 24px rgba(0,0,0,.4);
--shadow-gold: 0 20px 48px rgba(0,0,0,.5), 0 0 0 1px var(--color-accent-dim);
--ease-out:    cubic-bezier(0.16, 1, 0.3, 1);
--ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);
```

### Z-index Scale

```css
--z-base:    0;
--z-sticky:  100;   /* stats bar, sticky elements */
--z-nav:     200;   /* navigation */
--z-overlay: 300;   /* modal backdrop */
--z-modal:   400;   /* modal content */
--z-toast:   500;   /* notifications */
```


| 行為 | 規格 |
|------|------|
| Card hover | `translateY(-4px)` + `box-shadow: var(--shadow-gold)`（定義見下方） |
| 圖片 hover | `transform: scale(1.04)` + `transition: 0.4s ease` |
| Transition 預設 | `0.2s ease` |
| Focus ring | `outline: 2px solid var(--color-accent)` + `outline-offset: 2px` |
| Disabled | `opacity: 0.4`，`pointer-events: none` |

---

## 7. 響應式斷點

| 名稱 | 寬度 | 行為 |
|------|------|------|
| Mobile | < 768px | 單欄，間距縮小，隱藏部分元素 |
| Tablet | 768–1024px | 2 欄，footer 2 欄 |
| Desktop | > 1024px | 完整版面，max-width 1200px |

---

## 8. WordPress Classic Theme 對應

| 設計系統 | WordPress |
|---------|-----------|
| `design-system.css` | `wp_enqueue_style()` 在 `functions.php` 載入 |
| 元件 HTML 結構 | PHP template parts（`template-parts/components/`） |
| `.car-card` | 車輛 CPT 的 loop template |
| `.tour-card` | 行程 CPT 的 loop template |
| `.article-card` | Post loop template |
| Dark/Light token | `body` 加 `data-theme` attribute，可綁定用戶偏好或手動切換 |
| WordPress CPT schema | **不在本規格範圍內**。CPT 欄位定義（ACF schema）屬於 WP 開發規格，需另立文件。 |

---

## 9. 實作順序建議

1. `design-system.css` — token 層（Layer 1–2）
2. Reset + Typography utilities
3. 基礎元件：Button、Badge、Chip、Form Input
4. 結構元件：Section Header、Navigation、Footer
5. 卡片元件：Vehicle Card、Article Card、Tour Card、Review Card
6. Section 元件：Stats Bar、Advantage Card、Airport Card、Hero Booking Form、Promotion Section、Newsletter、FAQ Accordion
7. `design-system-preview.html` — 展示頁

---

## 10. 尚未定義（實作時補充）

- **Icon library 選型**：目前各頁面混用 SVG inline，建議統一為單一 sprite 或 icon font，選型留待開發階段決定。
