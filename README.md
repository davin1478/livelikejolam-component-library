# livelikejolam — Slot Reference

呢份文檔 mirror n8n Template Injection node 嘅 hardcoded slot mapping。23 個 component HTML file 嘅 mustache slot 名**必須**完全 match 呢度。

**Slot 命名 convention**：
- `_html` 後綴 → inject 已含 HTML tag（`<br>` / `<span class="underline">`）
- 冇 suffix → inject 純 plain text

**所有 component 通用 slot**（每個 file 都有）：
- `page_number` (string, e.g. `"01"`)
- `total_pages` (string, e.g. `"07"`)

---

## 01 · photo_cover

| Slot | Type | Notes |
|---|---|---|
| `eyebrow` | text | e.g. `A daily reflection` |
| `headline_html` | html | 含 `<br>` + `<span class="underline">` |
| `sub` | text | 1-2 lines, italic Cormorant |
| `image_url` | text | runtime URL 或 fallback gradient string |

**Worst-case headline_html length**：3 行，每行最多 6 字 + 1 個 underline span。
**Worst-case sub length**：2 行，每行 14 字。

⚠️ photo_cover 冇 ivory / deep variant，只有 `photo` variant。

---

## 02 · typography_cover

| Slot | Type | Notes |
|---|---|---|
| `eyebrow` | text | |
| `headline_html` | html | 大字 hero，3 行內 |
| `sub` | text | 1-2 lines |

**Variant**：`ivory` / `deep`

**Worst-case headline_html**：3 行，每行 5 字（最大字 scale），1-2 個 underline word。

---

## 03 · headline_xl

| Slot | Type | Notes |
|---|---|---|
| `eyebrow` | text | |
| `headline_html` | html | 大字，2 行內 |
| `body_text_html` | html | 5-8 lines, `<br>` separated |

**Variant**：`ivory` / `deep`

**Worst-case headline_html**：2 行，每行 7 字。
**Worst-case body_text_html**：8 行，每行 14 字。

---

## 04 · mic_drop_centered

| Slot | Type | Notes |
|---|---|---|
| `eyebrow` | text | |
| `headline_html` | html | 居中，2 行內 |
| `bottom_text_html` | html | 3-4 lines, `<br>` separated |

**Variant**：`ivory` / `deep` (★ deep recommended)

**Fixed ornament**：`⸺ ✦ ⸺`（喺 eyebrow 同 headline 之間，hardcode 入 template）

**Worst-case headline_html**：2 行，每行 6 字。

---

## 05 · scenario_table

| Slot | Type | Notes |
|---|---|---|
| `eyebrow` | text | e.g. `Sound familiar?` |
| `headline_html` | html | 1-2 行 |
| `rows_html` | html | pre-rendered `<li>` sequence |
| `closing_statement_html` | html | 1-2 行 punch |

**rows_html 格式**（n8n transform 後 inject 嘅 string）：
```html
<li><span class="scenario-want">想 quote 加人工</span><span class="scenario-do">→ 唔敢開口</span></li>
<li><span class="scenario-want">想表白</span><span class="scenario-do">→ 等佢主動</span></li>
...
```

**Variant**：`ivory` / `deep`

**Worst-case rows**：6 rows，want 字 12 字內，do 字 8 字內。

---

## 06 · quote_list_with_block

| Slot | Type | Notes |
|---|---|---|
| `eyebrow` | text | |
| `headline_html` | html | 1-2 行 |
| `quote_items_html` | html | pre-rendered `<li>` sequence |
| `punch_quote_html` | html | left-border 嘅大 quote block |

**quote_items_html 格式**：
```html
<li>「XX」</li>
<li>「YY」</li>
...
```

**Variant**：`ivory` / `deep`

**Worst-case quote_items**：5 items，每 item 16 字內。
**Worst-case punch_quote_html**：2-3 行。

---

## 07 · binary_split

| Slot | Type | Notes |
|---|---|---|
| `eyebrow` | text | |
| `headline_html` | html | 1-2 行 |
| `body_lead_in_html` | html | 1-2 行 lead-in |
| `option_a_condition` | text | e.g. `見到老細` |
| `option_a_action` | text | e.g. `→ 仆出去打招呼` |
| `option_b_condition` | text | |
| `option_b_action` | text | |
| `closing` | text | 1 行 punch |

**Variant**：`ivory` / `deep`

**Worst-case condition + action**：condition 8 字內，action 12 字內。

---

## 08 · cta_card

| Slot | Type | Notes |
|---|---|---|
| `cta_eyebrow` | text | e.g. `Three nights. One way.` |
| `cta_mainline_html` | html | 主 CTA line, 2 行內 |
| `cta_action_keyword` | text | 1-2 字，appears in 底部 action line |

**Hardcoded（非 slot）**：
- brand_mark: `林作斯多葛`
- brand_tag: `生活哲學 · LIVELIKEJOLAM`
- brand_detail line 1: `三天課程`
- brand_detail line 2: `教你用理性 overcome 情緒`
- brand_detail line 3: `唔係教你冇情緒`

**Variant**：`ivory` / `deep`

**底部 action line template**（hardcoded）：
```html
<div class="cta-action">
  跟住 <span class="accent">{{cta_action_keyword}}</span> 落 livelikejolam.com
</div>
```

---

## 09 · diagram_circles

| Slot | Type | Notes |
|---|---|---|
| `eyebrow` | text | |
| `headline_html` | html | 1-2 行 |
| `left_label` | text | circle 標題, e.g. `控制到` |
| `left_item_1` | text | |
| `left_item_2` | text | |
| `left_item_3` | text | |
| `right_label` | text | e.g. `控制唔到` |
| `right_item_1` | text | |
| `right_item_2` | text | |
| `right_item_3` | text | |
| `verdict_html` | html | 底部 punch quote, 1-2 行 |

**Variant**：`ivory` / `deep`

**Pattern**: dichotomy of control，左右並排兩個 circle，唔 overlap。

**Worst-case label**：5 字。
**Worst-case item**：6 字。

---

## 10 · flow_arrow

| Slot | Type | Notes |
|---|---|---|
| `eyebrow` | text | |
| `headline_html` | html | 1-2 行 |
| `flow_steps_html` | html | pre-rendered `<div class="flow-step">` sequence |
| `closing` | text | 1 行 |

**flow_steps_html 格式**：
```html
<div class="flow-step">
  <span class="flow-label">感受到驚</span>
  <span class="flow-sub">step 1</span>
</div>
<div class="flow-arrow-icon">↓</div>
<div class="flow-step">...</div>
...
```

⚠️ Arrow icon `↓` n8n 喺 transform 時自己 inject 入 step 之間，唔需要 Claude 出。

**Variant**：`ivory` / `deep`

**Worst-case steps**：4 steps，每 step label 12 字內。

---

## 11 · bar_compare

| Slot | Type | Notes |
|---|---|---|
| `eyebrow` | text | |
| `headline_html` | html | 1-2 行 |
| `bars_html` | html | pre-rendered `<div class="bar-row">` sequence |
| `verdict_html` | html | 底部 punch quote |

**bars_html 格式**：
```html
<div class="bar-row">
  <div class="bar-label">
    <span>你控制到嘅</span>
    <span class="bar-pct">~10%</span>
  </div>
  <div class="bar-track">
    <div class="bar-fill" style="width: 10%;"></div>
  </div>
</div>
...
```

**Variant**：`ivory` / `deep`

**Worst-case bars**：4 bars，label 10 字內。

---

## 12 · confession

| Slot | Type | Notes |
|---|---|---|
| `eyebrow` | text | 建議「First-person from Jo Lam」 |
| `setup_html` | html | 場景 setup, 含 `<br>`, 2-4 行 |
| `quote` | text | 純 plain text，**冇 HTML**（已喺 `.confession-quote` 入面） |
| `closing` | text | 1-2 行 |

**Variant**：`ivory` / `deep` (★ deep recommended)

**Worst-case setup_html**：4 行，每行 14 字內。
**Worst-case quote**：3 行（用 `\n` 或 `<br>` 由 quote 本身 inject？）

⚠️ **quote slot 我 propose 改做 `quote_html`** —— 因為林作親口金句經常 2-3 行（e.g.「我冇撚事報名做乜」係 1 行，但好多 case 想 break 多行 emphasis）。如果保留 plain text，3 行金句就要靠 CSS auto-wrap，但 wrap 位置控制唔到（中文無 word boundary）。

**Davin call**：keep plain text `quote`，定 upgrade 做 `quote_html` 容許 `<br>`？我會喺 polish 嗰陣**暫時用 `quote_html`**（template 內可以 inject `<br>`，但 plain text 都 work），等你 confirm 後決定 n8n side 點 inject。

---

## 共通 sanitization rule（n8n Parse Guard 已處理）

- ✅ Allow inside `*_html` slot：`<br>`, `<span class="underline">…</span>`
- ❌ Reject 其他 tag：`<script>`, `<img>`, `<a href>`, `<style>`, `<iframe>`, inline `style="…"`, `on*=` event handler

呢個 sanitization 喺 n8n side 處理，HTML template 唔需要任何 escape logic。
