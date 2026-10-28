---
name: ai-cardshop-price-search
description: >
  Broad-then-narrow price hunt for any AI subscription, account, refill, or
  digital good on Chinese card-shop aggregators (aifka, PriceMemo, OpenPrice,
  airadar), origin shops (16688, ldxp/wzyp), personal 小铺 directories, and consumer
  markets (Taobao, 闲鱼/goofish, 合租, X/Twitter discovery). Maps delivery types
  first (Auto pool, plugin, finished account, white account, SMS, student Claim,
  materials), clarifies which type the user wants, deep-dives shops and prices,
  then write-backs new product aliases / codewords / channel notes into this
  skill. reference.md also lists upstream GitHub projects and site recipes for
  agents. Use for Muse, Cursor, ChatGPT, Claude, Gemini, Grok, Windsurf, Devin,
  TRAE/trea, Consensus, or any new brand. Trigger phrases: price compare, find
  shop, refill, unlimited refill, card shop, ldxp, 16688, aifka, pricememo,
  openprice, airadar, cheapest shop, xianyu, goofish, 小铺, 比价, 找店, 续杯,
  无限续杯, 卡网, 链动, 闲鱼, 淘宝, 暗号, 便宜哪家, 学生优惠.
license: AGPL-3.0-or-later
metadata:
  short-description: Generic broad-then-narrow AI cardshop hunt + skill write-back
  version: 1.4.1
---

# AI Cardshop Price Search

**Generic** workflow for any AI digital good: index aggregators → map **delivery types** → lock the type the user wants → deep-dive shops/prices → **write durable learnings back into this skill**.

Never treat marketing phrases ("无限续杯", "学生Pro", "官方") as one SKU. Product names in [examples.md](examples.md) are **worked cases**, not the only supported brands.

## Design principles (generic)

1. **Product-agnostic core** — same A–J taxonomy and expansion order for every brand.
2. **Living product memory** — new brands, slang, and "where supply actually lives" go into [reference.md](reference.md) product playbook + optional new example.
3. **Channels are not equal** — some goods are 卡网-native (Cursor Auto); some are 淘宝/闲鱼-only (TRAE Type A); some need official Claim (Consensus student). Discover, then record.
4. **Prices expire; patterns don't** — re-fetch prices every hunt; keep codewords / channel routing / type maps.

## Index first (agents: open these before guessing shops)

| Role | URL | Strength |
|------|-----|----------|
| Multi-brand discovery + shop outbound | https://www.aifka.com | Title search (`q=`), brand filter, shop pages with `wzyp.cn` outbound |
| Shop / SKU memory | https://ai.pricememo.cn | Stable shop IDs, stock notes, origin links |
| Shop directory (个人小铺) | https://ai.pricememo.cn/shops | 链动 + **16688** personal shops; start here for “小铺” |
| Card-net SKU dump | https://www.openprice.cc/card-products/{slug} | Low-level SKUs: white accounts, plugins, SMS, day cards |
| Official-ish / 16688 channel map | https://airadar.vip | Finished official-tier; weak for refill/plugin |
| Public market snapshot | https://ai.nosec.uk | Extra price/stock observation; risk-oriented |
| Consumer marketplaces | Taobao / Tmall / 闲鱼 (goofish) | Long-tail + Type A when 卡网 empty; titles often **coded** |
| Social discovery | X (Twitter) + Google `site:` | Finds new personal shops / sellers → then open origin |
| 合租 / account shops | PremLogin-class, AccountBoy-class | Shared vs private; normalize per month |
| Official claim / pricing | Brand `/pricing` + help center | List price, student %, region limits |
| Payments materials | https://www.aifka.com/?kind=payment | VCC / visa for Stripe bind |

**Origin shops (after aggregator points you there):**

| Platform | Origin pattern | Notes |
|----------|----------------|-------|
| ldxp (链动) | `https://wzyp.cn/shop/{TOKEN}` | SPA; CAPTCHA common — aggregator first, user verifies |
| 16688 | `https://www.16688.com.cn/shop/S{digits}` · `/shop/{alias}` · `/goods/{id}` | Homepage search weak — PriceMemo `/shops` + airadar as index |
| Other ldxp systems | via aifka shop card `website_type` | Follow outbound on shop page |

**Do not** start on 16688 homepage search alone. **Do not** assume airadar `{brand}-pro` lists refill SKUs.  
**Do not** treat GitHub “Cursor 续杯工具” crack repos as shop indexes.  
**Do not** equate「个人小铺」with Type A — 16688 超市 often sells C/F while Auto续杯 lives on ldxp + independent 自助店.

Before harvesting a known brand, skim **Product playbook**, **Upstream projects & sites**, **Discovery channels**, **Seed shop catalog**, and **Cursor hunt notes** in [reference.md](reference.md).

## When to use

- Buy / compare **any** AI tool, membership, account, or materials on 卡网 / 淘宝 / 闲鱼 / 合租
- User says: 续杯, 无限续杯, Auto换号, 插件, 成品号, 代充, 白号, 接码, 学生优惠, 暗号, …
- User asks: which shop is cheap, who has stock, 16688/ldxp/淘宝/闲鱼 where?

## When primary indexes miss (required expansion)

If aifka / PriceMemo / OpenPrice / airadar return **0 relevant hits**, do **not** stop:

1. **WebSearch** brand + intent + 淘宝 / 闲鱼 / goofish / 合租 / 学生 / EDU / 代充 + **coded synonyms** + `16688` / 小铺
2. **PriceMemo `/shops`** + Google `site:16688.com.cn/shop` for **个人小铺**
3. **Taobao / Tmall** (browser); note 共享 vs 独享 vs 代充 — list price often ≠ real SKU
4. **闲鱼 (goofish)** — **required** for Type A / banned titles; parallel to Taobao, not optional
5. **X (Twitter)** advanced search (recipes in reference) — seller discovery only; still need origin URL
6. **合租 / account marketplaces** (PremLogin, AccountBoy-class, …)
7. **Official path**: `/pricing` + student/faculty help docs
8. **Materials path** (own-account official): EDU mail + VCC + bind services
9. Community (linux.do, V2EX, brand forum) for **slang/clues only** — prices need live listings

**Stop rule:** Steps 1–8 exhausted with nothing buyable → say not found; give cheapest verified official/materials path — never invent shops.

**Optional tooling (study, don't require install):** 闲鱼 monitors / price-radar repos listed under Upstream projects in [reference.md](reference.md) — useful when building automation; for a one-off hunt, WebFetch aggregators + browser is enough.

## Coded titles / synonym fan-out (Taobao · 闲鱼)

Platforms ban or bury literal `无限续杯` / brand+续杯. **Fan out** — one failed keyword ≠ no supply.

1. Literal brand + intent first.
2. If thin / 0 / wrong category (shoes, etc.) / 违规下架 → use codeword tables in [reference.md](reference.md).
3. Search **each** synonym on Taobao **and** 闲鱼; WebSearch `"闲鱼" + synonym`.
4. Re-classify every hit into A–J before pricing.
5. Prefer 付款量/评价 over cryptic lowest price.
6. In Sources: list **which codewords hit** (for write-back).

### Cross-brand soft titles (mix with `{brand}`)

| Plain intent | Try |
|--------------|-----|
| Type A / 续杯 | 续杯, 无感换号, 自动换号, 切号, 号池, 积分卡, 卡密, 天卡/周卡/月卡, 按时间付费 |
| Soften bans | 会议系统, 助手, 工具, 稳定token, 量大管饱, 独享账号, 独立账号, 双系统 |
| Context | 保留上下文, 不掉记录, 切号保留, solo无感 |
| Plugin (often B) | 插件, vsix, 激活器, 无感插件 |

Brand-specific seeds live in the **Product playbook** — extend there, not only in chat.

## Knowledge write-back (required after meaningful hunts)

This skill is a **shared memory**. After a hunt that teaches something new (new brand, new channel, new 暗号, empty-卡网 routing), **update the skill files in this folder** before ending — do not leave learnings only in chat.

### When to write back

| Trigger | Update |
|---------|--------|
| New brand / product family | Add row to Product playbook in [reference.md](reference.md) |
| New seller slang / 暗号 that worked | Add to that brand's aliases + generic coded table if reusable |
| Channel routing surprise (e.g. "only 闲鱼, aifka=0") | Playbook `Primary channels` + failure table if general |
| New delivery subtype for this brand | Note under playbook; keep A–J IDs stable |
| Full worked hunt worth reusing | Append short case to [examples.md](examples.md) (structure, not stale prices as gospel) |
| User asks to refresh skill / upload | Write-back + bump [CHANGELOG.md](CHANGELOG.md) version |

### Write-back checklist

```
Write-back:
- [ ] reference.md Product playbook row (aliases, channels, type notes, noise filters)
- [ ] New reusable codewords → generic coded table if cross-brand
- [ ] examples.md case if pattern is non-obvious (optional but preferred)
- [ ] CHANGELOG.md entry + version bump in SKILL.md + README.md metadata
- [ ] Do NOT paste ephemeral day prices as forever facts — label "observed YYYY-MM-DD, re-fetch"
```

### What not to hardcode forever

- Exact ¥/$ floors (re-fetch every run)
- Shop stock counts
- Official promo % without "verify live"

### What to keep forever

- Which channel class carries the SKU (卡网 vs 淘宝/闲鱼 vs official Claim)
- Alias / typo / 暗号 lists
- Brand-specific noise (e.g. TRAE YOUNG sneakers)
- Type A vs B vs C pitfalls for that brand

If the skill lives only under Downloads (not `~/.cursor/skills`), still edit **this repo folder**; tell the user where files changed.

## Lowest price (within confirmed type)

1. All **in-stock** SKUs of that type across expanded platforms.
2. Normalize unit when possible (per day / month / year); keep currency.
3. Floor + 1–2 backups.
4. Official list (and student % if any) as anchor.
5. Flag suspicious floors vs official (likely 共享/junk).

## Non-negotiables

1. **Broad → classify → confirm → deep** — never one shop from vague "续杯".
2. **Never price-compare across delivery types.**
3. **Aggregators index; origin/marketplaces sell** — warn lag/CAPTCHA.
4. **Honest risk** — third-party refill/tools unofficial; brief pool/plugin risks.
5. **CAPTCHA on origin** — aggregator numbers + user re-check.
6. **Examples are optional domain studies** — replace product freely.
7. **Degrade honestly** — no invented shops/prices.
8. **Expand until found or list exhausted.**
9. **State lowest in-stock for confirmed type** + official anchor.
10. **Taobao/闲鱼 synonym fan-out** — one banned keyword ≠ empty market.
11. **闲鱼 first-class** when 卡网 miss or titles soft.
12. **Write back** durable product/channel/slang learnings into this skill.

## Modes

- **`scout`** — taxonomy map + ask type (default when unclear).
- **`deep`** — confirmed type only; multi-source.
- **`lite`** — one aggregator + top 3 links.
- **`write-back`** — after hunt (or alone): update playbook/examples/CHANGELOG.

Unclear → `scout` then `deep`; end with `write-back` when something new was learned.

## Workflow checklist

```
Progress:
- [ ] 1. Scope: product family, user phrase, budget/horizon, mode
- [ ] 1b. Skim reference.md Product playbook for this brand (if row exists)
- [ ] 2. Broad harvest: >=2 aggregators; if 0 → expand (PriceMemo/shops + 淘宝+闲鱼+X +合租/official/materials)
- [ ] 2b. Synonym fan-out on 淘宝/闲鱼 when literal thin/banned
- [ ] 2c. If user wants 个人小铺: seed catalog + `/shops` + 16688 source ids in reference.md
- [ ] 3. Taxonomy map A–J
- [ ] 4. Confirm type (unless user already named it)
- [ ] 5. Deep: shops, SKUs, price, stock, links (incl. 闲鱼)
- [ ] 6. Cross-check; flag OOS/stale
- [ ] 7. Recommend: floor in-type + official anchor; exclude wrong types
- [ ] 8. Cite sources + codewords that hit + observed time
- [ ] 9. Write-back: playbook / examples / CHANGELOG if new learnings
```

## Step 1 — Scope

| Field | Example |
|-------|---------|
| Brand / product | any AI digital good (Cursor, Muse, TRAE, Consensus, …) |
| User words | "无限续杯" / "只要 Auto" / "学生Pro" / "质保成品" |
| Horizon | try 1 day / one month |
| Channel preference | 16688 only / ldxp OK / 淘宝闲鱼 OK / any |

Infer missing fields; do not block scout on perfect scope.

## Step 2 — Broad harvest

1. **aifka** `?brand={brand}&q={keyword}` — parse `page-bootstrap` `products[]`
2. **PriceMemo** `/products/{slug}` or `/shops/{TOKEN}`
3. **OpenPrice** `/card-products/{slug}`
4. **airadar** `/prices/{slug}` — official-tier; empty for refill → say so and move on

Then expansion + synonym fan-out as needed. Prices from aggregator, origin, or live marketplace only.

## Step 3 — Taxonomy map (required)

| Type | Title keywords | Usually means |
|------|----------------|---------------|
| A | Auto续杯, 无感换号, N个号, 积分卡, 会议系统+换号, 按时间付费 | Account pool + switcher |
| B | 不换号, 插件计费, vsix, 激活器 | Client plugin / inject |
| C | 成品号, 质保订阅, 官方会员, 直充 | Paid sub / ready account |
| D | 额度卡, 周期卡, 独享N额度 | Quota/time — verify ≠ A/C |
| E | 白号 | Raw DIY accounts |
| F | 接码, 实卡 | SMS / phone |
| G | Unclear | Do not recommend until classified |
| H | 学生优惠, Claim, SheerID, 教育优惠 | Official % off on *your* account |
| I | EDU邮箱, VCC, 3DS, 绑卡 | Materials for H |
| J | 学生成品号 | Finished account via student channel |

Scout: show map + bands → ask type → Step 4.

## Step 4 — Deep (single type)

Shop table: name | platform | SKU | price | stock | origin/index | notes. Prefer in-stock. Split day/week vs month. Tool URLs under notes only.

**False-friend filter (before the table):** drop D「独享额度/周期卡」misnamed 续杯, B「不换号/插件」, F「接码+换号」, and non-shop 续杯工具站.  
**Switcher / 切号助手 shops:** if week/month say「测过不退」, recommend day-card trial before month.  
**OOS cheapest:** list as watch; recommend next **in-stock** same type.

## Step 5 — Recommend

Trial floor → regular floor → backup → excluded types one-liner.

## Step 6 — Write-back

Apply [Knowledge write-back](#knowledge-write-back-required-after-meaningful-hunts). Tell user what files changed.

## Degradation path

1. State failure. 2. Exact URLs. 3. Label `aggregator-only` / `user-verify-origin` / `xianyu-app-verify`. 4. No fake tokens/stock/prices.

## Output template

```markdown
## Scope
- Product: ...
- Mode: scout | deep | lite
- Confirmed type: ... (or pending)

## Delivery-type map (broad)
| Type | Sample titles | Price band | In-stock shops |

## Shops after lock (deep)
| Shop | Platform | SKU | Price | Stock | Links |

## Suggest
1. Trial: ...
2. Month: ...
3. Excluded: ...

## Sources
- indexes + marketplaces + codewords that hit + observed time

## Write-back
- [ ] playbook / examples / changelog updated (or: nothing new)
```

## Additional resources

- URL recipes, Product playbook, discovery (Google/X/小铺), upstream GitHub/sites, seed shops: [reference.md](reference.md)
- Worked cases (Cursor A, Consensus H, TRAE A, discovery seeds): [examples.md](examples.md)
