# livelikejolam Component Library

IG carousel component HTML templates for livelikejolam Stoic content generation.

## Overview

呢個 repo host 緊 23 個 self-contained component HTML file，用嚟生成林作斯多葛 IG carousel post。

每個 file 係一張 IG slide 嘅 template（1080×1350px）。n8n workflow 喺 runtime fetch 個 HTML，將 Claude 生嘅 content inject 入去 `{{slot}}` placeholder，再透過 Browserless render 做 PNG。

## File Naming Convention

```
component_[component_name]_[variant].html
```

- `[component_name]` — 例：`headline_xl`, `scenario_table`, `cta_card`
- `[variant]` — `ivory`（米黃底，default）/ `deep`（深色底，contrast）/ `photo`（photo_cover 專用）

## Components (23 files total)

| Component | Variants | Purpose |
|---|---|---|
| photo_cover | photo | Slide 1 配相 cover |
| typography_cover | ivory, deep | Slide 1 純文字 cover |
| headline_xl | ivory, deep | 大字 hero insight |
| mic_drop_centered | ivory, deep | 居中 punchline（mic-drop） |
| scenario_table | ivory, deep | 受眾日常情境清單 |
| quote_list_with_block | ivory, deep | 自言自語 list + punch quote |
| binary_split | ivory, deep | 二元選擇對比 |
| cta_card | ivory, deep | Slide 7 brand statement（locked product positioning） |
| diagram_circles | ivory, deep | 控制二分法 / 二元 SVG |
| flow_arrow | ivory, deep | 流程 / 因果 |
| bar_compare | ivory, deep | 數據對比 |
| confession | ivory, deep | 林作 first-person 故事 |

## Design Tokens (locked)

```
--ivory:        #F4EFE3   主背景米黃
--deep:         #2D2A22   深色 contrast
--ink:          #1A1A17   主文字（米黃底用）
--warm-white:  #E8DDC7   主文字（深底用）
--brass:        #B8923B   accent / underline
--brass-bright: #C9A961   highlight 用
--line:         #C8C0A8   divider（米黃底用）
--line-deep:   #4A4538   divider（深底用）
--muted:        #7A7468   副文字
```

## Slot Syntax

呢啲 file 用 Mustache-style placeholder：

```html
<div class="headline">{{headline_html}}</div>
<div class="eyebrow">{{eyebrow}}</div>
```

n8n template injection 將 `{{slot_name}}` 替換成 Claude JSON 入面對應 field 嘅 value。

## Versioning

呢個 repo 係 v1 reference implementation。Visual creator polish 之後會直接 push 上嚟 update。
n8n workflow 用 raw URL fetch，唔需要 redeploy。

## Raw URL Pattern

```
https://raw.githubusercontent.com/[USERNAME]/livelikejolam-component-library/main/component_[name]_[variant].html
```

## License

Private — Lin Stoic project use only.
