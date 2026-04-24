# Sky Horse Auto — Design Specification

## Overview

Sky Horse 天馬租車是美國自駕租車品牌的落地頁。視覺語言：**Midnight Black × Golden Hour Yellow** — 深邃黑夜襯托金色亮點，傳遞專業可信賴的美式租車體驗。設計乾淨不留多餘裝飾，以內容和圖片為主。

---

## Colors

| Token | Hex | Usage |
|---|---|---|
| `--bg-base` | `#060606` | 頁面底層背景 |
| `--bg-surface` | `#0e0e0e` | Section 背景、卡片底色 |
| `--bg-elevated` | `#161616` | 卡片懸停、疊加層 |
| `--bg-card` | `#111111` | 獨立卡片背景 |
| `--border` | `rgba(255,255,255,0.06)` | 預設邊框（幾乎看不見） |
| `--border-mid` | `rgba(255,255,255,0.12)` | 中等可見邊框 |
| `--border-yellow` | `rgba(255,209,0,0.4)` | 金色邊框（限量供應提示） |
| `--gold` | `#FFDC22` | 主品牌色、CTA 按鈕、重要標籤 |
| `--gold-bright` | `#F5C94E` | 次要金色（hover 狀態） |
| `--gold-dim` | `rgba(232,184,75,0.12)` | 金色背景区块（低對比） |
| `--gold-glow` | `rgba(232,184,75,0.08)` | 金色光暈效果 |
| `--ink-primary` | `#F2F2F2` | 主要文字（近白） |
| `--ink-secondary` | `#8A8A8A` | 次要說明文字 |
| `--ink-tertiary` | `#4A4A4A` | 三級文字（標籤、小字） |
| `--success` | `#34C759` | 成功狀態 |
| `--info` | `#0A84FF` | 資訊提示 |

## Typography

| Token | Font | Size | Weight | Usage |
|---|---|---|---|---|
| `--font-display` | Space Grotesk | — | — | 標題、數字、標籤 |
| `--font-body` | Noto Sans TC | — | — | 內文（繁體中文） |
| `--font-mono` | JetBrains Mono | — | — | 機場代碼、分類標籤 |

### Type Scale（重要數值）

```
Display:  Space Grotesk, 700, 56px, letter-spacing: -2px
H1:       Space Grotesk, 700, 40px, letter-spacing: -1.5px
H2:       Space Grotesk, 600, 32px, letter-spacing: -1px
H3:       Space Grotesk, 600, 24px
Eyebrow:  JetBrains Mono, 500, 11px (全大寫，追蹤 3px)
Body:     Noto Sans TC, 400, 16px, line-height: 1.7
Caption:  Noto Sans TC, 400, 13px
Stat:     Space Grotesk, 700, 40px
Stat-label: Space Grotesk, 400, 12px
```

## Layout

### Container System

```
Container max-width:  1200px
Container padding:    0 40px (水平)
Section padding:     80px 0 (垂直)
Inner pattern:       max-width: 1200px; margin: 0 auto; padding: 0 40px;
```

所有內容區塊（section、inner container）統一使用 1200px 為最大寬度，居中顯示。

### Spacing Scale（4px 為基底）

| Token | Value |
|---|---|
| `--sp-1` | 4px |
| `--sp-2` | 8px |
| `--sp-3` | 12px |
| `--sp-4` | 16px |
| `--sp-5` | 20px |
| `--sp-6` | 24px |
| `--sp-8` | 32px |
| `--sp-10` | 40px |
| `--sp-12` | 48px |
| `--sp-16` | 64px |
| `--sp-20` | 80px |
| `--sp-24` | 96px |

## Elevation & Depth

使用陰影和背景層次建立深度，盡量少用 border：

- **卡片陰影：** `--shadow-card: 0 2px 12px rgba(0,0,0,0.4)`
- **金色光暈：** `--shadow-gold: 0 0 40px rgba(232,184,75,0.15)` — 用於金色按鈕、供應商 logo 牆

## Shapes

| Token | Value | Usage |
|---|---|---|
| `--r-sm` | 6px | 按鈕、晶片、小元件 |
| `--r-md` | 10px | 卡片、輸入框 |
| `--r-lg` | 16px | 大卡片、彈出層 |
| `--r-xl` | 24px | 主視覺區塊 |

## Motion

```css
--ease-out:    cubic-bezier(0.16, 1, 0.3, 1)   /* 自然減速（首選） */
--ease-in-out: cubic-bezier(0.65, 0, 0.35, 1) /* 對稱動畫 */
```

原則：動畫要有意義，針對 transform / opacity，避免用 layout 屬性。

---

## Components

### Button — Primary (Gold)

```
backgroundColor:  --gold (#FFDC22)
textColor:        #000000
fontFamily:       --font-display
fontWeight:       600
fontSize:         15px
padding:          14px 28px
borderRadius:     --r-sm (6px)
cursor:           pointer
hover:
  backgroundColor: --gold-bright (#F5C94E)
  transform: translateY(-1px)
active:
  transform: translateY(0)
```

### Button — Secondary (Outline)

```
backgroundColor:  transparent
textColor:        --ink-primary
border:           1px solid --border-mid
fontFamily:       --font-display
fontWeight:       500
fontSize:         15px
padding:          13px 27px
borderRadius:     --r-sm (6px)
hover:
  border-color: --gold
  color: --gold
```

### Card (Car)

```
backgroundColor: --bg-card (#111111)
borderRadius:    --r-lg (16px)
padding:         0
border:          1px solid --border
overflow:        hidden
Shadow:          --shadow-card
hover:
  border-color: --border-mid
  transform: translateY(-4px)
  box-shadow: --shadow-gold
```

### Section Eyebrow Label

```
fontFamily:    --font-mono (JetBrains Mono)
fontSize:      11px
fontWeight:    500
letterSpacing: 3px
textTransform: uppercase
color:         --gold (#FFDC22)
display:       inline-flex
alignItems:    center
gap:           10px
```

### Nav

```
backgroundColor: rgba(6,6,6,0.92)
backdropFilter:  blur(20px)
height:          64px
borderBottom:    1px solid --border
inner max-width: 1200px
inner padding:   0 40px
```

### Form Input

```
backgroundColor: --bg-elevated
border:          1px solid --border-mid
borderRadius:    --r-md (10px)
padding:         12px 16px
color:           --ink-primary
fontFamily:      --font-body
fontSize:        15px
focus:
  border-color: --gold
  outline: none
```

### Filter Tag / Chip

用於部落格/新聞分類篩選按鈕（如：全部、名人推薦、自駕攻略、租車指南）。

**預設狀態：**
```
fontFamily:    --font-display (Space Grotesk)
fontSize:      11px
fontWeight:    600
letterSpacing: 1px
textTransform: uppercase
color:         rgba(255,255,255,0.4)
background:    rgba(255,255,255,0.05)
border:        1px solid rgba(255,255,255,0.08)
borderRadius:  20px
padding:       6px 14px
cursor:        pointer
transition:    all 0.2s
```

**Active / Hover 狀態：**
```
background: rgba(255,255,255,0.06)
border-color: rgba(255,255,255,0.92)
color: #ffffff
```

**容器 (.blog-tags)：**
```
display: flex
gap: 8px
flex-wrap: wrap
justify-content: flex-start
margin-bottom: 32px
```

### Special Tag (e.g. 阿拉斯加專區)

與 Filter Tag 樣式相同，但用於專案專區標示，語意上為永久性標籤。

### Category Label + Popular Badge

用於車款分類頁的分類名稱（如：休旅車 SUV）旁邊的可點擊熱門/新到標示。

**分類名稱 (.cat-card-type)：**
```
display: flex
align-items: center
gap: 10px
flex-wrap: wrap
fontSize: 16px
fontWeight: 800
color: --ink-primary
```

**熱門徽章 (.cat-card-badge)：**
```
display: inline-flex
align-items: center
gap: 4px
background: rgba(255,220,34,0.12)
border: 1px solid rgba(255,220,34,0.3)
color: --gold (#FFDC22)
fontSize: 9px
fontWeight: 700
letterSpacing: 1px
lineHeight: 1
padding: 4px 8px
borderRadius: 4px
```

**熱門徽章 SVG icon：**
```
width: 10px
height: 10px
color: --gold
```

**新到徽章 (.cat-card-badge.is-new)：**
```
background: rgba(255,255,255,0.08)
border: 1px solid rgba(255,255,255,0.15)
color: --ink-secondary
```

### Car Model Tag

車款卡上列出具體車款的小型標籤（如：Honda CR-V、Subaru Outback 四驅）。

**預設狀態：**
```
fontFamily:    --font-display (Space Grotesk)
fontSize:       12px
fontWeight:     500
color:          --ink-secondary
background:     rgba(255,255,255,0.04)
border:         1px solid rgba(255,255,255,0.12)
borderRadius:   20px
padding:        3px 12px
lineHeight:     1.5
whiteSpace:     nowrap
transition:     color 0.15s, border-color 0.15s, background 0.15s
```

**Hover 狀態：**
```
color:          --ink-primary
border-color:   rgba(255,255,255,0.25)
background:     rgba(255,255,255,0.08)
```

**容器 (.cat-card-models)：**
```
display: flex
flex-wrap: wrap
gap: 4px
```

---

## Do's and Don'ts

- **DO** 使用金色（`--gold`）只出現在最重要的 CTA、eyebrow 標籤、hover 狀態
- **DO** 所有內容區塊最大寬度不超過 1200px
- **DO** Section 垂直 padding 統一 80px（mobile 改 60px）
- **DO** 圖片滿版不裁切，保持 16:9 或 4:3 比例
- **DON'T** 不要在同個 view 混用超過兩種 border-radius 等級
- **DON'T** 不要用 `--ink-primary` 以外的白色作為主要文字
- **DON'T** 不要在深色背景上用純白（`#ffffff`），應用 `--ink-primary`（`#F2F2F2`）
- **DON'T** 不要在非金色元素上使用 `--gold` 色系

---

## File Structure Conventions

```
index.html        — 首頁（所有 CSS 內嵌）
fleet.html        — 車款頁
```

新增頁面時：
1. 引入 Google Fonts：Space Grotesk、Noto Sans TC、JetBrains Mono
2. 引用 `DESIGN.md` 中的 tokens，勿自行定義新的顏色/間距值
3. Section 結構：`<section> → <div class="inner"> → 內容`
4. Inner 容器：`max-width: 1200px; margin: 0 auto; padding: 0 40px;`
