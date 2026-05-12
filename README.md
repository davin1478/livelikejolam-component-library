# livelikejolam-component-library

IG carousel 嘅 visual template repo。23 個 self-contained HTML，俾 n8n workflow fetch 落嚟做 template injection，最後 Puppeteer screenshot 出 1080×1350 PNG ship 上 IG。

**Internal repo**：only Davin + n8n developer 接手。

---

## 1 · 呢個 repo 喺成個 pipeline 邊個位

```
┌──────────────────┐
│ Notion form 入   │ 同事填 topic / persona / image
│  request         │
└────────┬─────────┘
         │ webhook POST
         ▼
┌──────────────────┐
│ n8n workflow     │
├──────────────────┤
│ Node 2: fetch    │ 攞最新 brand context（Notion 04 / 01 / 00）
│  live data       │
├──────────────────┤
│ Node 4: Claude   │ 出 3 個方向 carousel JSON
│  API call        │
├──────────────────┤
│ Node 5: Parse    │ keyword guard + HTML sanitize
│  + Guard         │
├──────────────────┤
│ Node 7: Template │ ◀── 呢度 fetch 呢個 repo 嘅 component HTML
│  Injection       │     用 Claude JSON 填 {{slot}}
├──────────────────┤
│ Node 8: Render   │ Puppeteer / Browserless screenshot
│  PNG             │     viewport 1080×1350 deviceScaleFactor 2
└────────┬─────────┘
         ▼
┌──────────────────┐
│ Notion review    │ 同事揀 direction A/B/C
│  page            │
└────────┬─────────┘
         ▼
       ship IG
```

**呢個 repo 嘅角色**：純粹係 visual template layer，唔包 logic、唔包 content。所有動態嘢經 n8n inject 入 mustache slot。

---

## 2 · File map

### Component HTML（23 個，喺 `/components/`）

| Component | Variants | 主要 use case |
|---|---|---|
| `photo_cover` | photo only | Slide 1 when user uploads image |
| `typography_cover` | ivory · deep | Slide 1 when no image (text-driven cover) |
| `headline_xl` | ivory · deep | 大字 hero insight / core takeaway slide |
| `mic_drop_centered` | ivory · deep | 居中 punchline · breathing slide · 倒數第二張固定 |
| `scenario_table` | ivory · deep | 受眾日常情境清單（左想做 / 右實際） |
| `quote_list_with_block` | ivory · deep | 列「你 dictate 自己嘅 N 句話」+ punch quote |
| `binary_split` | ivory · deep | 二分對比（同一情境，兩種人嘅處理） |
| `cta_card` | ivory · deep | 最後一張固定，林作斯多葛 brand mark + CTA |
| `diagram_circles` | ivory · deep | Dichotomy of control 視覺化（左右兩 circle） |
| `flow_arrow` | ivory · deep | 流程 / 因果 / 轉化（A → B → C） |
| `bar_compare` | ivory · deep | 數據對比（控制到 vs 控制唔到 etc.） |
| `confession` | ivory · deep | 林作 first-person 故事 · direction_b 必用至少一張 |

**File naming convention**：`component_{name}_{variant}.html`
（photo_cover 例外，淨係一個 `component_photo_cover_photo.html`）

### Docs（root）

| File | 用途 |
|---|---|
| `README.md` | 呢份文檔 |
| `_design_tokens.md` | Color / typography / spacing / ornament 嘅 source of truth |
| `slot_reference.md` | 23 個 component 嘅 mustache slot table（match n8n inject mapping） |
| `preview.html` | Visual review 入口 · 23 個 component grid · click 任何一個 fullscreen 1:1 view |

---

## 3 · Mustache slot system

每個 component HTML 入面有 `{{slot_name}}` placeholder，n8n Node 7 (Template Injection) 會用 Claude 出嘅 JSON 填曬。

### 兩種 slot type（lock 死）

**Plain text slot**（冇 suffix）— inject 純文字
```
{{eyebrow}}             →  A daily reflection
{{cta_action_keyword}}  →  驚
{{left_label}}          →  控制到
```

**HTML slot**（`_html` suffix）— inject 含 `<br>` / `<span class="underline">` 嘅 string
```
{{headline_html}}  →  「驚」<span class="underline">唔會走</span><br>你做完佢都仲喺度
{{body_text_html}} →  個驚唔會等到「啱嘅時刻」消失<br>你 60 歲都會驚開口要嘢
```

### 共通 slot（所有 23 個 file 都有）
```
{{page_number}}   →  "01" .. "07"
{{total_pages}}   →  "07"
```

### HTML tag allowlist（喺 n8n Node 5 keyword guard 處理）

✅ Allow inside `_html` slot：
- `<br>`
- `<span class="underline">...</span>`

❌ Reject：其他所有 tag（`<script>`、`<img>`、`<a href>`、inline `style=`、event handler 等）

完整 slot list 睇 [`slot_reference.md`](./slot_reference.md)。

---

## 4 · Design tokens（鎖死，唔可以亂改）

詳細喺 [`_design_tokens.md`](./_design_tokens.md)。摘要：

**Color**：
- `--ivory: #F4EFE3` / `--deep: #2D2A22`
- `--ink: #1A1A17` / `--warm-white: #E8DDC7`
- `--brass: #B8923B` / `--brass-bright: #C9A961`
- `--line: #C8C0A8` / `--line-deep: #4A4538`
- `--muted: #7A7468` / `--muted-deep: #8A8474`

**Typography**：
- 中文：Noto Serif TC（400 / 500 / 700 / 900）
- 英文：Cormorant Garamond（300 / 400 / 500 / 600）
- 字體經 Google Fonts CDN load

**Canvas**：1080 × 1350px（IG 4:5）

**Ornament hierarchy**（3 層，lock）：
1. Master ornament `⸺ ✦ ⸺` — 只 `mic_drop_centered` 用
2. Section divider（110px brass 線）— `typography_cover` / `cta_card`
3. Soft divider（1px line full-width）— footer / list border

**cta_card hardcoded text**（唔係 slot，唔可以動）：
```
brand_mark:   林作斯多葛
brand_tag:    生活哲學 · LIVELIKEJOLAM
brand_detail: 三天課程
              教你用理性 overcome 情緒
              唔係教你冇情緒
```

---

## 5 · 出新版本嘅 workflow

### Case A：想 fine-tune typography / spacing / SVG proportion（visual polish）

⚠️ **唔好直接手改 23 個 file**。改 1 個 token 要改 46 處（23 file × 2 variant 共用 code），出錯率高。

**正確做法**：

1. 入返 generator script（content creator AI 嗰邊有 `generate_components.py`）
2. 改 `BASE_STYLES` 入面嘅 design token，或者 component-specific styles section
3. Re-run script → 出 23 個新 HTML file
4. 開 `preview.html` 視覺 verify
5. 跑 1-2 個 dry run carousel 對比 quality
6. Push commit 取代舊版

### Case B：想加新 component（13th component）

1. 同 content creator AI 同 n8n developer 三邊 align（會影響 n8n inject mapping）
2. 喺 `_design_tokens.md` 加新 component section
3. 喺 `slot_reference.md` 加新 slot list
4. 喺 generator script 加新 component spec
5. Re-run script → 出 2 個新 HTML file（ivory + deep）
6. n8n side 同步加 inject mapping
7. Push 取代

### Case C：想改 cta_card brand wording

`cta_card` 嘅 brand_mark / brand_tag / brand_detail 係 hardcoded text，**唔經 Claude inject**。

1. 改 generator script 入面 `CTA_BODY` lambda 嘅 hardcoded string
2. Re-run → 出新 cta_card_ivory + cta_card_deep
3. Push

⚠️ 任何 brand-facing wording change 一定要 Davin sign-off 至改，因為呢啲嘢一改成 30 張 carousel 都會 follow。

### Case D：想改 mustache slot 名

⚠️ **呢個會 break n8n**。改 slot 名要兩邊 sync：

1. n8n developer 改 Template Injection node 嘅 inject mapping
2. Content creator AI 改 generator script
3. 兩邊一齊 deploy
4. 改 `slot_reference.md`

唔可以單邊改。

---

## 6 · 常見伏

**伏 1：直接 browser 開單獨 component HTML**
會見到 raw `{{slot_name}}` 字符，呢個係 by design——template 未 inject 前本來就咁。Visual review 永遠開 `preview.html`，唔好開單獨 file。

**伏 2：Puppeteer / Browserless 唔等 font load**
Google Fonts 係 async resource。Render 之前必須 `waitUntil: 'networkidle0'`，否則 PNG 出嚟 fallback 系統字體（serif default）。

**伏 3：Underline span 跨行**
`<span class="underline">` 用 `background-image: linear-gradient(...)` 畫底線，browser 會自動每行 repaint。但 typography size 如果改大／改細，要 re-test 6px gap 同 6px stroke 仲啱唔啱比例（喺 `_design_tokens.md` 第 5 section）。

**伏 4：scenario_table / quote_list / flow_arrow / bar_compare 嘅 array slot**
呢幾個係 `rows_html` / `quote_items_html` / `flow_steps_html` / `bars_html` — n8n 必須先將 Claude 出嘅 array transform 做 pre-rendered HTML string 先 inject。直接塞 array 入 mustache 唔 work。Format 喺 `slot_reference.md` 每個 component section 有 example。

**伏 5：diagram_circles 嘅 SVG item 字超過 6 字**
SVG `<text>` 坐標係 fix 死。每個 circle 入面 3 個 item，每個字位中文 6 字以內。Claude system prompt 要 reinforce 呢個 constraint，唔係靠 template 處理。

**伏 6：photo_cover 嘅 image_url fallback**
冇圖嘅 case，n8n inject `image_url = "linear-gradient(135deg, #3a3530 0%, #1a1814 100%)"`（一條 gradient string）。CSS `background-image` 同 accept URL 同 gradient，所以 template 結構唔變。

---

## 7 · n8n side 必須 sync 嘅嘢

呢啲 commit 之前要同 n8n developer confirm 對齊：

- [ ] Slot name 100% match `slot_reference.md`（特別係 `confession` 嘅 `quote_html`，v1 用 `quote` plain text，v2 改咗）
- [ ] Node 5 keyword guard 識 reject `_html` slot 入面非 allowlist 嘅 tag
- [ ] Node 8 Puppeteer config：`viewport: { width: 1080, height: 1350, deviceScaleFactor: 2 }`, `waitUntil: 'networkidle0'`
- [ ] Component template URL 用 GitHub raw URL prefix（`https://raw.githubusercontent.com/davin1478/livelikejolam-component-library/main/components/`）

---

## 8 · Version history

- **v2** (2026-05-11)：visual creator polish · scale up 至 real 1080×1350 · typography hierarchy refine · diagram_circles 用 inline SVG (dichotomy pattern) · ornament hierarchy 收斂 3 層 · `confession` slot 由 `quote` upgrade 做 `quote_html` · cta_card brand_tag 改 `生活哲學 · LIVELIKEJOLAM`
- **v1** (initial)：reference design from content creator · 480×600 preview scale · baseline structure for SVG components

---

## Contact

- Brand / wording call：Davin
- Visual polish：Content Creator AI（呢個 repo 嘅 maintainer）
- n8n integration：n8n developer
