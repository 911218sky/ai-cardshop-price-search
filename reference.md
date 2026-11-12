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
| ldxp / 链动 origin | `https://wzyp.cn/shop/{TOKEN}` · `https://wzyp.cn/item/{id}` · `https://pay.ldxp.cn/item/{id}` | Origin after aggregator; item URLs common on AiCheap/BestAI |

### Secondary aggregators (PriceAI-class — use for outbound links + cross-check)

Primary indexes find *that supply exists*; these often expose **clickable origin goods URLs** (`16688.com.cn/goods/G…`, `wzyp.cn/item/…`, `pay.ldxp.cn/item/…`) that WebFetch alone may strip. **Always re-classify A–J** — many pages mix invite / SMS / finished.

| Site | URL pattern | Strength |
|------|-------------|----------|
| BestAI | https://www.bestai.date/products/{slug} | Tag filters e.g. `?tags=delivery_account`; 「前往购买」→ origin |
| PriceAI | https://priceai.cc/products/{slug} · https://www.priceai.io/products/{slug} | `?mode=finished_account` etc.; large Muse/Cursor tables |
| AiCheap | https://aicheap.pro/products/{slug} | Product-table `<a href>` often **clean origin**; fee notes |
| 水獭 shuita | https://shuita.xyz · `/shop/{TOKEN}` | Fee-inclusive 到手价; shop pages; Muse SKU under「更多」 |
| 一可比价网 | https://vs.yiketools.com/same-source/{id} | Same-source multi-shop floor for one pasted listing |
| AI张大爷 | https://aizhangdaye.com/account/{Brand} | Finished-account boards (was aizhangdama); 「直达购买」→ origin |
| BoAIle | https://boaile.com/deals/ | Daily deals; **Muse often miscategorized under Grok Super** — still useful |
| aiapiprice | https://aiapiprice.com/{brand} | Mixed SMS/invite/C; score columns — filter hard |
| AIGOOD radar | https://aigood.chat/price-radar/products/{slug} | Sometimes embeds raw 16688 goods links in seller notes |
| AI价网 | https://aiprice.club/market | Broad卡网 dump; search noisy — pair with brand playbook |

**OpenPrice caveat (observed 2026-11-12):** Muse finished SKUs may appear under `grok-super` / `other-other` / SMS pages — do not trust slug name alone; filter title for 成品号 vs 接码/邀请码.

### Open-source projects to study (connectors, monitors, taxonomy)

| Repo | Stars-ish role | Why it matters for this skill |
|------|----------------|-------------------------------|
| [BeterXie/ai_price_radar](https://github.com/BeterXie/ai_price_radar) | PriceMemo backend | **16688 + ldxp shop intake**, public offer model, shop submit flow — mirror URL shapes & platform detection |
| [lincwang123-bot/price-radar](https://github.com/lincwang123-bot/price-radar) | airadar | `platform16688` collectors, Cursor/Claude/GPT families, channel=`16688` |
| [84376834111/gpt-account-market-scanner](https://github.com/84376834111/gpt-account-market-scanner) | Market scanner | Categories (Plus/Claude/Cursor/中转…), stock history, source scheduling |
| [gm365/shuita-ai-price](https://github.com/gm365/shuita-ai-price) | 水獭 MCP + skill | `ai-price-mcp` live floors; fee-inclusive口径 |
| [AIprice-club/aiprice](https://github.com/AIprice-club/aiprice) | AI价网 dump | Large shop/quote corpus for offline study |
| [dimthink/PriceAI](https://github.com/dimthink/PriceAI) | PriceAI product | Card-net + official + 中转 taxonomy reference |
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
| **Muse** / Meta Muse / mune | 10亿token, 310亿, 质保首登, 邀请码, 代刷/助力, 普号, CDK兑换, 手搓, 耐活王 | **卡网-native** class: 16688 + ldxp + PriceAI-class; expand via discovery loop (not fixed shops) | **C** 成品号 (10B/310B/20B); invite; 代刷; F 接码 | **Muse hunt notes** = how to find more sources; 邀请≠成品≠代刷≠接码 |
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
https://airadar.vip/prices/muse-account-10b
https://airadar.vip/?family=muse

# PriceAI-class (secondary — outbound + cross-check)
https://www.bestai.date/products/{slug}
https://www.bestai.date/products/muse-ai-account?tags=delivery_account
https://priceai.cc/products/{slug}
https://www.priceai.io/products/muse-ai?mode=finished_account
https://aicheap.pro/products/{slug}
https://aicheap.pro/products/muse-account
https://shuita.xyz/
https://aizhangdaye.com/account/{Brand}
https://aizhangdaye.com/account/Muse?sort=price
https://vs.yiketools.com/same-source/{id}
https://boaile.com/deals/
https://aiapiprice.com/{brand}
https://aiprice.club/market

# Origin
https://wzyp.cn/shop/{TOKEN}
https://wzyp.cn/item/{id}
https://pay.ldxp.cn/item/{id}
https://www.16688.com.cn/shop/{TOKEN}
https://www.16688.com.cn/shop/S{digits}
https://www.16688.com.cn/goods/G{digits}
```

If slug 404s, homepage search / family page / WebSearch brand.

## Extracting origin buy links (agent method)

WebFetch markdown often **drops** 「前往购买」hrefs. When user asks for links:

1. Prefer aggregators that keep `<a href>` in product tables (**AiCheap**, aifka product page “原始链接”).
2. Else use **cursor-ide-browser**: navigate → filter type (e.g. BestAI Ready-made / `tags=delivery_account`) → CDP `Runtime.evaluate` collect `a[href]` whose text is 前往购买 / 直达购买 / Go to offer, with nearby title+price.
3. Normalize: keep `16688.com.cn/goods/G…`, `wzyp.cn/item/…`, `pay.ldxp.cn/item/…`, independent shop URLs; strip UTM if desired.
4. **Re-classify** each title before listing — same table mixes invite / SMS / C.
5. 16688 origin pages often WAF/CAPTCHA — give goods URL + note user must open in browser.

## Muse hunt notes (experience / discovery — not a fixed shop list)

**Do not** treat past item ids, same-source ids, or “¥X is the floor” as permanent. Re-search every hunt; follow whatever shops aggregators surface today.

**Channel routing (class, not shop):** Muse finished / invite / 代刷 is usually **卡网-native** (16688 + ldxp). Prefer aggregator → origin. Taobao/闲鱼 are optional long-tail if 卡网 thin. Official Power/Max rarely show as cheap card SKUs.

**Split types before comparing ¥:**

| Bucket | Title cues | Notes |
|--------|------------|-------|
| Invite | 邀请码 | Tiny ¥ often 教程/对接 — **勿拍** as account |
| 代刷 / 助力 | 邀请好友, 邀请助力, Token助力, 自助卡密刷额度, CDK redeem | Buyer’s **own** account; bundle≈30 invites ≠ 拍30次×10B |
| 额度帮刷 | 需要自备成品号 | Refill-only — exclude from finished deep |
| F 接码 | Meta/Muse 接马 | Not finished |
| **C 10亿成品** | 成品号, 普号10亿, 账密+2FA, 邮箱\|取件 | Main C path |
| C 310亿 / 20亿 | 310亿, 300亿, 满额度, 20亿 | Separate bands; never mix into 10B floor |

**Delivery formats (both C):** `账号|密码|2FA` vs `邮箱|邮件取件地址`.

**Noise:** 教程/批发对接; title says 成品但链到邀请码; OpenPrice/BoAIle mis-bucket under Grok; deviant sub-¥1 flags.

### Discovery loop (expand sources every run)

Run until you have **≥2 independent origin item URLs** of the locked type, or indexes exhausted:

1. **Keyword fan-out on 一可 / aifka / PriceAI / AiCheap / 张大爷** — search brand + `成品号` / `10亿` / `310亿` / `邀请` / `接马` separately; open *new* result clusters, don’t reuse last chat’s links.
2. **Same-source as a tool, not a bookmark** — from any live goods hit, open 一可「同货源聚合」to map reseller spread; sort **价格低有库存**. Different title families (账密2FA vs 邮箱取件 vs `新批次`) are different clusters — hunt each.
3. **Follow shops outward** — PriceAI/aifka merchant or shop cards → visit shop → list *all* Muse SKUs there → new item ids. Low-price shops often restock under new ids.
4. **PriceMemo `/shops` + WebSearch** `site:wzyp.cn` / `site:16688.com.cn` + brand — catch personal shops aggregators lag on.
5. **Snapshot sites** (TokenPlus, mozhenzhen, AIGOOD) — use for history / leads only; always re-verify stock on origin or same-source.
6. **Independent 自助站** — when chat/X/bio shows a domain, open it, classify SKUs, add domain pattern to playbook if stable (don’t freeze one SKU).
7. **Stop condition** — quote today’s **有货地板** + **货多次低** (liquid band with many in-stock shops). If user wants lower, say what to watch (same-source 有货 min / new-batch titles), not “wait N weeks for fixed ¥”.

### Listed floor ≠ in-stock floor

Aggregator “最低价” often includes **缺货**. Always prefer 有货/库存少量 rows; re-check origin when PriceAI/AiCheap counters look stale.

### Price-timing patterns (heuristic)

- Thin flash listings below the liquid multi-shop band appear/vanish in **hours–1 day** — watch alerts, don’t bank on a multi-week dump.
- Titles like `MM.DD新批次` often start expensive, then clones pull the liquid band down over **~1–3 days** — re-run discovery, don’t assume last week’s floor.
- Structural collapses need supply shocks; absent that, waiting long rarely beats buying liquid 有货地板 + small buffer.

**Link extraction:** AiCheap/Open shop hrefs; PriceAI「前往购买」/ merchant「前往原站」(CDP); 张大爷「直达购买」; strip UTM; re-classify title beside each href.

**Ops sellers repeat:** fixed residential IP (US/CA/JP/SG); incognito/fingerprint; no FB; rebind email after first login; 质保首登 often 30min–3h; dirty IP → 人脸/waitlist usually **out of warranty**.

## Seed shop catalog (bootstrap only — expand beyond this list)

Observed via PriceMemo + Cursor pages **~2026-10-28**.  
**Seeds = start URLs for a hunt, not the universe.** After opening a seed, crawl that shop’s full catalog, follow same-source neighbors, and WebSearch for shops *not* in this table. Re-fetch stock/prices; drop dead seeds on write-back.

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
| User wants **buy links** but WebFetch has no href | Aggregator SPA / markdown strip | Browser + CDP scrape 「前往购买」; prefer AiCheap table links |
| Muse ¥0.02 “成品” | Invite/tutorial bait | Filter 邀请码/教程; lock Type C 成品号 |
| Muse “额度帮刷” in finished table | Refill SKU | Exclude unless user wants 代刷 |
| OpenPrice Muse under grok-super | Mis-bucket | Title-filter 成品号; also check other-other / SMS pages |
| BoAIle Muse under Super Grok deals | Mis-tag | Still harvest; re-classify before quote |
| Aggregator “最低价” below liquid band | Often 缺货 ghosts | Sort **价格低有库存**; quote 有货地板 separately |
| Reusing last hunt’s item/same-source ids | Stale / OOS / renumbered | Re-run discovery loop; follow shops for new ids |
| Sub-¥1 / deviant Muse C flags | Flash / bait / OOS | Confirm live stock before calling floor |
| aizhangdama.com dead / redirect | Rebrand | Use https://aizhangdaye.com/account/{Brand} |
| Primary aggregators have Muse but no goods URL | Index without outbound | Hit BestAI/PriceAI/AiCheap/张大爷 for origin |

## Optional: browser MCP

Prefer curl + bootstrap for aifka. Use `cursor-ide-browser` for CAPTCHA shop UI, JS-only pages, or **origin-link extraction**.

Sequence: `browser_navigate` → `browser_lock` → filter type → CDP `Runtime.evaluate` on buy `<a href>` → unlock.

CDP snippet idea (adapt selectors per site):

```js
[...document.querySelectorAll('a')]
  .filter(a => /前往购买|直达购买|Go to offer/i.test(a.textContent||''))
  .map(a => ({ href: a.href, row: (a.closest('tr')||a.parentElement)?.innerText?.slice(0,200) }))
```

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
