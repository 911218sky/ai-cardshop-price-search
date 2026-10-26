# AI Cardshop Price Search — Reference

Query recipes, **Product playbook** (living memory), coded-title fans, and failure table.  
**Re-fetch prices live.** Patterns and aliases are what this file is for.

## Product playbook (extend on every new brand)

Copy a blank row when a hunt discovers a new product. Keep aliases + channels; do not freeze day prices.

### Blank row template

```markdown
| Field | Value |
|-------|-------|
| Brand / aliases | |
| Typo / slang / 暗号 | |
| aifka brand= / slugs | |
| Primary channels | 卡网 / 淘宝 / 闲鱼 / 合租 / official |
| Common types seen | A B C … |
| Noise filters | |
| Notes | |
| Last pattern update | YYYY-MM-DD |
```

### Catalog (observed patterns — re-verify prices)

| Brand / aliases | 暗号 / also-search | Primary channels | Common types | Noise / pitfalls |
|-----------------|--------------------|------------------|--------------|------------------|
| **Cursor** | Auto, Composer, 积分卡, 无感换号, cc续杯 | **卡网** (aifka/ldxp) strong for A | A 积分卡; B 插件; C 质保Pro; E 白号; F 接码 | airadar Pro ≠ Auto续杯 |
| **Muse** / Meta Muse | 10亿token, 质保首登, 邀请码, 代刷邀请 | 卡网 + 16688 + PriceAI-class | C 成品号; invite; 代刷 (not always A) | ¥0.01 教程勿拍; 邀请码 ≠ 成品号 |
| **TRAE** / trea / traex | 会议系统, 按时间付费, 切号, 保留上下文, solo, 国际版, CN额度号 | **淘宝+闲鱼**; aifka often **0** | A 换号; C/代充 Pro; H rare | TRAE YOUNG = 球鞋; device 多号风控 |
| **Consensus** | 学生, Claim, 15刀, 合租/独享 | **官方 Claim** + 淘宝/合租; aifka **0** | H 学生%; C 独享; Shared junk | ¥3–5「年卡」多为共享 |
| **ChatGPT** | Plus, 成品号, 直充, 学生 | 卡网 + airadar + 合租 | C/H/I/J mixed | Separate 共享席位 |
| **Claude** | Pro, Max, 成品号 | 卡网 + airadar | C 为主 | |
| **Gemini** / Google AI | Pro, Ultra, 兑换 | 卡网 + airadar | C / CDK | |
| **Grok** | SuperGrok, 成品号 | 卡网 | C | |
| **Windsurf** / **Devin** / **Kiro** / … | (fill when hunted) | (fill) | (fill) | Add row on first deep hunt |

**Rule:** After a hunt, if the brand row is missing or wrong → update this table in the same session (skill write-back).

## Aggregator URL recipes

Replace `{brand}` / `{q}` / `{slug}` / `{TOKEN}` / `{shop_id}`.

```
# aifka discover
https://www.aifka.com/?brand={brand}&q={q}
https://www.aifka.com/?brand=cursor&q=auto
https://www.aifka.com/?brand=cursor&q=%E7%BB%AD%E6%9D%AF
https://www.aifka.com/shops/{shop_id}
https://www.aifka.com/products/{product_id}

# PriceMemo
https://ai.pricememo.cn/products/{slug}
https://ai.pricememo.cn/shops/{TOKEN}
https://ai.pricememo.cn/sources/ldxp/products/{slug}

# OpenPrice card-net dump
https://www.openprice.cc/card-products/{slug}
https://www.openprice.cc/

# airadar (official-tier / 16688 map)
https://airadar.vip/?family={family}
https://airadar.vip/prices/{slug}
https://airadar.vip/prices/{slug}?channel=16688

# Origin
https://wzyp.cn/shop/{TOKEN}
https://www.16688.com.cn/shop/{TOKEN}
https://www.16688.com.cn/goods/{id}
```

If slug 404s, homepage search / family page / WebSearch brand.

## Marketplace coded-title recipes (Taobao · 闲鱼)

```
# Generic (+ brand)
{brand} 会议系统
{brand} 按时间付费
{brand} 保留上下文
{brand} 切号
{brand} 无感换号
{brand} 独享账号
{brand} 积分卡
{brand} 卡密 天卡

# WebSearch wrappers
"闲鱼" {brand} {codeword}
goofish {brand}
site:goofish.com {brand}
```

**闲鱼:** public web is partial — user confirms in app. Record nick + 按天/周/月.

**Noise:** drop brand collisions (footwear, physical merch, unrelated courses).

## aifka page-bootstrap parse (Node)

```js
const fs = require('fs');
const html = fs.readFileSync('aifka.html', 'utf8');
const start = html.indexOf('{', html.indexOf('page-bootstrap'));
let d = 0, end = -1;
for (let k = start; k < html.length; k++) {
  if (html[k] === '{') d++;
  else if (html[k] === '}') { d--; if (d === 0) { end = k + 1; break; } }
}
const data = JSON.parse(html.slice(start, end)).data;
for (const p of data.products || []) {
  console.log({
    title: p.title,
    price: p.price,
    shop: p.shop_name,
    shop_id: p.shop_id,
    stock: p.stock_count,
    status: p.stock_status,
  });
}
```

Shop HTML: grep `go?url=` or `wzyp.cn/shop/`.

## Type-keyword cheat sheet

| Type | Include | Exclude when hunting this type |
|------|---------|--------------------------------|
| A | Auto续杯, 无感换号, 积分卡, 会议系统+换号, 按时间付费换号 | 不换号插件, 质保成品号 |
| B | 不换号, 插件计费, vsix, 激活器 | Auto积分卡 (split if same shop) |
| C | 成品号, 质保, 官方会员, 直充 | 续杯池, 插件, 白号 |
| D | 额度卡, 周期卡 | Do not merge with A/C until verified |
| E | 白号 | Materials only |
| F | 接码, 实卡 | Not membership |
| H | 学生优惠, Claim, SheerID | Materials → I |
| I | EDU, VCC, 绑卡 | Not the membership itself |
| J | 学生成品号 | Verify ≠ H |

## Scout → deep question (copy)

```
我先按交付形态分好了（A 换号池 / B 插件 / C 成品号 / D 额度卡 / E 白号 / F 接码 / H 官方学生 / I 材料 / J 学生成品）。
你要的是哪一种？确认后我只深挖那一类，并把新暗号/渠道写回 skill。
```

## Session capture card

```
- Product / brand:
- Mode: scout | deep | lite | write-back
- Confirmed type:
- Aggregators hit:
- Marketplace codewords that hit:
- Key URLs:
- Top in-stock SKUs (type-matched):
- Playbook write-back needed? Y/N
- Observed at:
```

## Failure table

| Symptom | Likely cause | What to do |
|---------|--------------|------------|
| airadar empty for refill | Wrong class | aifka/OpenPrice; say 16688 weak for refill |
| wzyp CAPTCHA | Bot check | Aggregator prices; user opens origin |
| Thin aifka HTML | SPA | Parse `page-bootstrap` |
| Wild price spread same title | Mixed types | Split by taxonomy |
| Cheapest OOS | Sold out | Next in-stock; OOS watch |
| 淘宝/闲鱼 0 on 无限续杯 | Title ban | Coded synonyms + 闲鱼 parallel |
| Wrong category hits | Brand collision | Add AI/IDE/国际版; drop noise |
| Only 会议系统/助手 | Soft Type A | Confirm 换号/按时间 with seller |
| aifka 0 for whole brand | Not on 卡网 | Playbook: 淘宝/闲鱼/official; write-back |

## Optional: browser MCP

Prefer curl + bootstrap for aifka. Use `cursor-ide-browser` for CAPTCHA shop UI or JS-only pages.

Sequence: `browser_tabs` list → `browser_navigate` → snapshot → extract → unlock.

## Expansion recipes (when 卡网 empty)

```
# Official (any brand)
https://{brand-site}/pricing
# Student help: search "{brand} student discount" / Claim / SheerID

# Materials
https://www.aifka.com/?q=edu
https://www.aifka.com/?kind=payment

# Consumer
Taobao / 闲鱼: {brand} + coded titles
合租: PremLogin / AccountBoy-class + {brand}
```

### Official price capture

CDP `document.body.innerText` on `/pricing` if snapshot thin. Note Monthly vs Annual. Re-check student % live.

### Student product checklist

- [ ] H vs I vs J/C vs Shared separated
- [ ] Official list + student % as anchor
- [ ] Lowest in-stock per confirmed type only
- [ ] Suspicious ¥ floors vs official $ flagged

### Write-back snippet (paste into CHANGELOG)

```
- Product playbook: {brand} — channels={...}; aliases={...}; types={...}
```
