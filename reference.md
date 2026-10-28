# AI Cardshop Price Search — Reference

Query recipes, **Product playbook** (living memory), coded-title fans, failure table,  
**discovery channels** (Google / X / personal shops), and **upstream projects** agents should study.  
**Re-fetch prices live.** Patterns, URLs, and channel routing are what this file is for.

## Upstream projects & sites (AI: read these for connectors / UX)

Use as **implementation + indexing references**, not as “buy here” endorsements. Prefer public docs / README / connectors over copying proprietary scrapers wholesale.

### Live aggregators / markets (primary hunt targets)

| Site | URL | What agents use it for |
|------|-----|------------------------|
| aifka | https://www.aifka.com | Multi-brand discover (`brand`/`q`), shop cards → ldxp outbound |
| PriceMemo / AI Price Radar | https://ai.pricememo.cn | Shop directory, SKU memory, stock notes, origin links |
| PriceMemo shops index | https://ai.pricememo.cn/shops | Personal + multi-SKU shops (链动 + **16688**) |
| PriceMemo shop submit | https://ai.pricememo.cn/shops/submit | How personal shops get indexed (`16688.com.cn/shop/...`) |
| OpenPrice | https://www.openprice.cc | Card-net SKU dump (white / plugin / SMS / day cards) |
| airadar | https://airadar.vip | Official-tier / 16688 map; weak for Type A refill |
| ai.nosec.uk | https://ai.nosec.uk | Public AI digital-goods price/stock snapshot (risk-oriented) |
| 16688 | https://www.16688.com.cn | Origin card platform (`/shop/S…`, `/shop/{alias}`, `/goods/G…`) |
| ldxp / 链动 origin | https://wzyp.cn/shop/{TOKEN} | Origin after aggregator points here; CAPTCHA common |

### Open-source projects to study (connectors, monitors, taxonomy)

| Repo | Stars-ish role | Why it matters for this skill |
|------|----------------|-------------------------------|
| [BeterXie/ai_price_radar](https://github.com/BeterXie/ai_price_radar) | PriceMemo backend | **16688 + ldxp shop intake**, public offer model, shop submit flow — mirror URL shapes & platform detection |
| [lincwang123-bot/price-radar](https://github.com/lincwang123-bot/price-radar) | airadar | `platform16688` collectors, Cursor/Claude/GPT families, channel=`16688` |
| [84376834111/gpt-account-market-scanner](https://github.com/84376834111/gpt-account-market-scanner) | Market scanner | Categories (Plus/Claude/Cursor/中转…), stock history, source scheduling |
| [Usagi-org/ai-goofish-monitor](https://github.com/Usagi-org/ai-goofish-monitor) | 闲鱼 AI monitor | Playwright + keyword/AI filter + notify — use when 卡网 miss / Type A on 闲鱼 |
| [ddCat-main/ai-goofish](https://github.com/ddCat-main/ai-goofish) | 闲鱼 monitor | Keyword crawl + AI screen + mail notify |
| [fancyboi999/goofish-cli](https://github.com/fancyboi999/goofish-cli) | 闲鱼 CLI/MCP | Agent-friendly marketplace automation |
| [tristanwqy/GooFish-AIMonitor](https://github.com/tristanwqy/GooFish-AIMonitor) | Local 闲鱼 console | Price-drop / sold-out; local-first |

**Do not** treat Cursor “续杯工具 / 换号 / 绑卡试用” crack repos as hunt indexes — they are client abuse tooling, not shop discovery. Prefer aggregators + markets above.

### Connector notes (from upstream docs — re-check in repo)

- **16688:** public shop detail + goods list APIs; accept `/shop/{alias}` and `/shop/S{digits}`; PriceMemo intake resolves alias → formal shop no.
- **dujiao-next / 独立发卡:** some radars read unauthenticated public REST — treat as another origin class beside 16688/ldxp.
- **链动 / ldxp:** often indexed via aifka shop id → `wzyp.cn/shop/{TOKEN}`; PriceMemo tokens like `WCT9GRNG`, `UELAUHQA`.

## Discovery channels beyond aggregators

When indexes are thin, or user asks “how to find personal shops / chatter”, fan out here — then **classify A–J** and re-check live stock on origin.

### Google / WebSearch recipes

```
# Brand + cardshop
"{brand}" (续杯 OR 小铺 OR 发卡 OR 车位 OR 代充)
"{brand}" ("16688" OR 链动 OR aifka OR pricememo)
site:16688.com.cn/shop {brand}
site:ai.pricememo.cn/shops {brand}
"16688.com.cn/shop" {brand}

# Personal shop discovery
site:16688.com.cn Cursor
"链动小铺" {brand}
```

Use time tools (past week/month) — shop URLs churn. Prefer `"quoted"` phrases for 无限续杯 / 小铺.

### X (Twitter) recipes

Google rarely indexes live X well — agent should use WebSearch **and** tell user to paste advanced search on x.com when needed.

```
{brand} (续杯 OR 车位 OR 小铺 OR 代充) lang:zh
"16688" ({brand} OR Claude OR GPT OR Cursor)
{brand} Pro (小铺 OR 发卡) -is:retweet
since:YYYY-MM-01 {brand} 续杯
```

**Workflow:** X discovers seller → copy bio/pinned link (16688 / wzyp / 独立站 / TG) → open origin → map type → optional PriceMemo `/shops/submit` if public shop missing from index.

### Personal shops (个人小铺)

| How they appear | How to index |
|-----------------|--------------|
| Already on PriceMemo `/shops` | Prefer directory; note platform badge `16688` vs `链动小铺` |
| Raw 16688 URL | `https://www.16688.com.cn/shop/S######` or `/shop/{alias}` |
| ldxp-only | via aifka shop page or `wzyp.cn/shop/{TOKEN}` |
| X / TG / 闲鱼 only | No public shop API → list as `user-verify` + nick; cannot invent stock |

**Rule:** Aggregators only see **public** shop pages. Private TG menus are clues, not priced SKUs until a public URL exists.

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
| **Cursor** | Auto, Composer, 积分卡, 无感换号, cc续杯, 车位, 额度卡, 周期卡, 切号助手 | **Type A:** aifka/ldxp + **独立自助店** (e.g. TXUN); **not** 16688 超市. **Type C:** 16688/airadar strong. Soft titles: X/闲鱼 | A 积分卡/多号池/切号助手; B 不换号插件; C 质保Pro/Ultra; D 额度/周期卡; E 白号; F 接码 | See **Cursor hunt notes** below; airadar Pro ≠ Auto续杯; 16688「换号」常=接码 |
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
https://ai.pricememo.cn/
https://ai.pricememo.cn/shops
https://ai.pricememo.cn/shops/submit
https://ai.pricememo.cn/products/{slug}
https://ai.pricememo.cn/products/cursor-pro
https://ai.pricememo.cn/shops/{TOKEN}
https://ai.pricememo.cn/sources/ldxp/products/{slug}
https://ai.pricememo.cn/sources/ldxp/products/cursor-pro

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
https://www.16688.com.cn/shop/S{digits}
https://www.16688.com.cn/goods/{id}
```

If slug 404s, homepage search / family page / WebSearch brand.

## Seed shop catalog (Cursor / AI card — patterns only)

Observed via PriceMemo directory + Cursor product pages **~2026-10-28**.  
**Re-fetch stock/prices.** Use as hunt seeds (start URLs), not a buy list.

### Cursor-heavy ldxp shops (PriceMemo tokens)

| Shop (label) | PriceMemo / origin | Likely types | Notes |
|--------------|-------------------|--------------|-------|
| cursor续杯会员店 | https://ai.pricememo.cn/shops/WCT9GRNG | **D** 周期/额度卡 | Name says 续杯 — still not A |
| llz-cursor续杯 | https://ai.pricememo.cn/shops/UELAUHQA | **A** 50号/300号 Auto | Cheapest A; often OOS — watch |
| YOUGO科技 | aifka `/shops/1368` → wzyp `PP47XS8K` | **A** 积分卡日/周/月 | High stock on aifka |
| AI工具官方授权店铺 | aifka `/shops/318` → wzyp `xxdlzs` | **A** Auto天/周/月 | High stock; 天卡激活不退 |
| TXUN 自助小店 | https://shop.txun.cc/ | **A** 切号助手日/周/月 | Independent personal; 周月测过不退 |
| 如鱼得水(玩转ai) | harvey / PriceMemo | C Cursor成品 | Not A |
| 壹号铺 / Tora / 小头 / 陆柒 | PriceMemo / aifka | **B** 不换号插件 | Exclude from A deep |
| 壹号铺 / CodeFree (代充 SKUs) | cursor-pro product page | C 代充/成品 | Separate from their B rows |

Cursor aggregate: https://ai.pricememo.cn/products/cursor-pro  
ldxp source slice: https://ai.pricememo.cn/sources/ldxp/products/cursor-pro

### 16688 personal shops (PriceMemo source ids — AI multi-SKU)

These are **personal/small** 16688 shops already indexed; open PriceMemo shop row → “原站” for live `16688.com.cn/shop/S…`. Source ids seen on directory:

| Label (as listed) | PriceMemo source id | Approx quote volume (then) |
|-------------------|---------------------|----------------------------|
| 👑XAI店铺（全品类超市） | `16688-S209643` | high |
| 派大星（4000人群） | `16688-S343514` | mid |
| 珠世AI | `16688-S265669` | mid |
| 梦泽Ai | `16688-S528762` | mid |
| 华润 | `16688-S552918` | mid |
| 奥特曼直销 | `16688-S802470` | mid |
| 欣雨ai | `16688-S405579` | mid |
| 柯炀网络 | `16688-S221796` | mid |
| 霜星的AI小店 | `16688-S248535` | mid |
| 源点数字 | `16688-S396555` | mid |
| ai聚合 | `16688-S981603` | mid |

Origin guess: `https://www.16688.com.cn/shop/S209643` (replace digits from source id). Always confirm via PriceMemo outbound — aliases exist.

### Independent / materials sites (clues only)

| Site | URL | Role |
|------|-----|------|
| TXUN Cursor 自助小店 | https://shop.txun.cc/ | Type A 切号助手卡密；个人独立站；日卡试水 |
| 小文ai小铺 | https://yidachuang.top/ | Cursor 额度/Ultra/Auto — classify per SKU |
| (others) | from X bio / WebSearch | Add to playbook when stable |

**Also note:** 16688 shops above are useful for **C/F** hunts; for **Cursor Type A** prefer seed A rows (YOUGO / 官方授权 / TXUN / llz) instead.

### Cursor Type D vs A reminder (from live titles)

| Title pattern | Default type | Price band memory (re-fetch) |
|---------------|--------------|------------------------------|
| N天/月 pro 独享 N额度（周期卡/额度卡） | D | day~¥8–30; month~¥45–80 class |
| Auto续杯 月卡 N个号+无感换号 | A | month~¥23–100 class |
| 无感切号助手 月卡（独立站卡密） | A | ~¥30 class (TXUN-class) |
| 正规实付 / 质保订阅 Pro 成品 | C | month~¥130–160+ class |
| 旧版 Pro 20刀 独享成品 (16688) | C | ~¥135 class observed |

### Cursor hunt notes (observed 2026-10-28 — patterns, re-fetch prices)

**Where Type A actually lives**

| Channel | Type A Auto 续杯/多号换号 | Notes |
|---------|---------------------------|-------|
| aifka `?brand=cursor&q=auto` / `续杯` / `积分卡` | Strong | YOUGO / 官方授权积分卡·Auto月卡; high stock |
| aifka `?q=换号` alone | **Weak / wrong** | Mostly **B**「不换号-插件」hits — do not treat as A |
| ldxp specialty shop (llz-cursor续杯) | True A titles | Often **cheapest + OOS** — watch, don't recommend as buyable |
| Independent 自助小店 (TXUN `shop.txun.cc`) | True A | Personal-shop class; day/week/month; check no-refund rules |
| 16688 personal 超市 (XAI/派大星/珠世…) | **Almost empty for A** | Cursor rows = **C** Ultra/Pro成品 or **F** 接码; skip for Auto月卡 |
| airadar `cursor-pro?channel=16688` | C-tier | Not A |

**False-friend titles (must classify before pricing)**

| Looks like 续杯 | Real type | Examples |
|-----------------|-----------|----------|
| 店名含「续杯」+「独享N额度/周期卡」 | **D** | cursor续杯会员店 `WCT9GRNG` |
| 「不换号」「插件计费」「每天对话N次」 | **B** | 壹号铺、Tora、小头、陆柒科技 |
| 「接马/接码」+「可换号/自助换号」 | **F** | 16688 Codex/Cursor 接码 — 换号≠号池续杯 |
| 「续杯工具」独立下载站 / GitHub 换号 | Not a shop | Client tooling — exclude from shop table |

**个人小铺 recommendation rule (Cursor A)**

1. Prefer **in-stock** A: independent 自助店 (TXUN-class) or ldxp via aifka (YOUGO / 官方授权).  
2. List **llz-class** cheapest SKU as **OOS watch** when stock=0.  
3. Do **not** send users to 16688 超市 for Auto月卡 just because the shop is「个人小铺」.  
4. If seller requires 切号助手: suggest **day card first** when week/month announce「测过不退」.

**aifka / 16688 agent tips**

- aifka shop outbound: HTML `go?url=` → `wzyp.cn/shop/{TOKEN}` (YOUGO=`PP47XS8K`, 官方授权=`xxdlzs`).  
- 16688: `POST https://www.16688.com.cn/shopApi/shop/detail` `{shop_no}` (alias e.g. `AIDP` → `S209643`); `POST .../shopApi/goods/list` `{shop_no,page,limit}`.  
- Bulk 16688 paging: rate-limit / empty body common; **dedupe `goods_no`** — pages may repeat. Keep scans light (1–2 pages) or sleep between shops.

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
| Need personal 16688 shops | Homepage search weak | PriceMemo `/shops` + airadar channel=16688; Google `site:16688.com.cn/shop` |
| X chatter but no price | Bio/TG only | Extract public shop URL; else `user-verify`; don't invent SKUs |
| Confused by 续杯工具站点 | Client switcher, not 卡网 | Exclude from shop table; hunt aifka/PriceMemo instead |
| User wants 个人小铺 + Cursor Auto | 16688 超市无 A | Route to TXUN-class / ldxp A shops; say 16688=C/F |
| aifka `q=换号` full of 插件 | Query too broad | Use `brand=cursor&q=auto` / `续杯` / `积分卡`; drop 不换号 |
| Cheapest Auto月卡 OOS | Specialty shop sold out | Recommend next in-stock A; keep OOS as watch |
| Switcher 周/月「测过不退」 | Policy on independent shops | Recommend day card trial first |
| 16688 goods list empty/duplicate | Rate limit / bad paging | Sleep; dedupe goods_no; don't trust huge page counts |

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

# Discovery (shops / sellers, then classify)
Google/WebSearch: see "Discovery channels" above
X advanced search: see X recipes above
PriceMemo shop directory: https://ai.pricememo.cn/shops
Submit missing public shop: https://ai.pricememo.cn/shops/submit
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
