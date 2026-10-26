---
name: ai-cardshop-price-search
description: >
  Broad-then-narrow price hunt for AI subscriptions, accounts, refills, and
  tools on Chinese card-shop aggregators (aifka, PriceMemo, OpenPrice, airadar)
  and origin shops (16688, ldxp/wzyp). Maps delivery types first (Auto huanhao,
  plugin, finished account, white account, SMS), clarifies which type the user
  wants, then deep-dives shops and prices. Use when the user wants to buy or
  compare Muse, Cursor, ChatGPT, Claude, Gemini, Grok, Windsurf, Devin, refill,
  unlimited refill, plugin, finished account, top-up, CDK, white account, SMS,
  or asks which shops sell something on 16688 / ldxp / card shops. Trigger
  phrases: price compare, find shop, refill, unlimited refill, card shop, ldxp,
  16688, aifka, pricememo, openprice, airadar, cheapest shop,
  比价, 找店, 续杯, 无限续杯, 卡网, 链动, 便宜哪家.
license: AGPL-3.0-or-later
metadata:
  short-description: Broad-then-narrow AI cardshop price hunt via aggregators
  version: 1.0.0
---

# AI Cardshop Price Search

Find AI membership / account / refill offers by **indexing aggregators first**, mapping **delivery types broadly**, then **locking the type the user wants** and deep-diving shops + prices. Never treat "无限续杯" / unlimited refill as one SKU.

## Index first (agents: open these before guessing shops)

| Role | URL | Strength |
|------|-----|----------|
| Multi-brand discovery + shop outbound | https://www.aifka.com | Title search (`q=`), brand filter, shop pages with `wzyp.cn` outbound |
| Shop / SKU memory | https://ai.pricememo.cn | Stable shop IDs, stock notes, origin links |
| Card-net SKU dump | https://www.openprice.cc/card-products/{slug} | Low-level SKUs: white accounts, plugins, SMS, day cards |
| Official-ish / 16688 channel map | https://airadar.vip | Good for finished official-tier accounts; weak for refill/plugin |

**Origin shops (after aggregator points you there):**

| Platform | Origin pattern | Notes |
|----------|----------------|-------|
| ldxp (链动) | `https://wzyp.cn/shop/{TOKEN}` | SPA; often CAPTCHA — prefer aggregator scrape, user verifies origin |
| 16688 | `https://www.16688.com.cn/shop/{TOKEN}` · `/goods/{id}` | SPA; public search weak — use airadar/aifka as index |
| Other ldxp systems | via aifka shop card `website_type` | Follow outbound on shop page |

**Do not** start on 16688 homepage search alone. **Do not** assume airadar `cursor-pro` lists refill SKUs.

## When to use

- User wants to buy / compare AI tools or accounts on card shops
- User says: 续杯, 无限续杯, Auto换号, 插件, 成品号, 代充, 白号, 接码, Muse, Cursor, ...
- User asks: which shop is cheap, who has stock, does 16688 have it, ldxp sellers

## Non-negotiables

1. **Broad → classify → confirm → deep** — never jump to one shop from a vague "续杯".
2. **Never price-compare across delivery types** (Auto pool ≠ plugin ≠ warranty finished account).
3. **Aggregators index; origin shops sell** — quote aggregator price + link; warn stock/price may lag.
4. **Honest risk scope** — third-party refill/tools are unofficial; note Admin/inject/account-pool risks briefly.
5. **If CAPTCHA blocks origin** — use aifka/PriceMemo numbers; tell user to re-check origin before pay.
6. **Domain examples are optional** — Cursor refill case in [examples.md](examples.md) is a worked study.
7. **Degrade honestly** if aggregators fail — no invented shops or prices.

## Modes

- **`scout`** (default when target type unclear): map taxonomy + 2-4 candidate types + ask user which type.
- **`deep`** (type confirmed, or user already named it): multi-aggregator hunt for that type only.
- **`lite`**: one aggregator + top 3 links for a quick peek.

If unclear, run **`scout`** first, then **`deep`**.

## Workflow checklist

Copy and track:

```
Progress:
- [ ] 1. Scope: product family, user phrase, budget/horizon, mode (scout|deep|lite)
- [ ] 2. Broad harvest: >=2 aggregators (prefer aifka + PriceMemo or OpenPrice)
- [ ] 3. Taxonomy map: cluster hits into delivery types
- [ ] 4. Confirm target type with user UNLESS already named clearly
- [ ] 5. Deep pass: only that type — shops, SKUs, price, stock, outbound URLs
- [ ] 6. Cross-check: second aggregator or shop page; flag OOS / stale
- [ ] 7. Recommend: cheapest trial + best monthly in-stock; exclude wrong types
- [ ] 8. Cite sources: aggregator URL / shop token / when observed
```

## Step 1 — Scope

Ask (or infer) only what is missing:

| Field | Example |
|-------|---------|
| Brand / product | Cursor, Muse, ChatGPT Plus |
| User words | "无限续杯" / "只要 Auto" / "要质保 Pro" |
| Horizon | try 1 day / need one month |
| Channel preference | 16688 only / ldxp OK / any |

Do **not** wait for perfect scope before scout harvest — gather types first, then confirm.

## Step 2 — Broad harvest (always multi-source)

1. **aifka:** `https://www.aifka.com/?brand={brand}&q={keyword}`
   - Parse `page-bootstrap` JSON (`products[]`: title, price, shop_name, stock_count, shop_id).
   - Shop outbound: `https://www.aifka.com/shops/{shop_id}` → `/go?url=` → `wzyp.cn/shop/...`
2. **PriceMemo:** `https://ai.pricememo.cn/products/{slug}` or `/shops/{TOKEN}`
3. **OpenPrice:** `https://www.openprice.cc/card-products/{slug}`
4. **airadar:** `https://airadar.vip/prices/{slug}` and `?channel=16688` — mainly official-tier; if empty for refill, say so and move on

Keyword seeds (mix):

| Intent hint | Try q= |
|-------------|--------|
| refill / 续杯 | 续杯, auto, 换号, 插件 |
| official membership | 成品号, 质保, 官方, 直充 |
| materials | 白号, 接码 |

WebSearch may discover shop names; **prices must come from aggregator or origin**.

## Step 3 — Taxonomy map (required)

| Type | Title keywords | Usually means |
|------|----------------|---------------|
| A | Auto续杯, 无感换号, N个号, 积分卡 | Free/trial account pool + switcher; often Auto/Composer only |
| B | 不换号, 插件计费, 插件 | Client plugin / inject quota |
| C | 成品号, 质保订阅, 官方会员, 直充 | Real paid sub / ready account |
| D | 额度卡, 周期卡, 独享N额度 | Quota/time card — verify; do not assume = A or C |
| E | 白号, Free Auto 白号 | Raw accounts (DIY), not a refill service |
| F | 接码, 实卡 | SMS / phone for registration |
| G | Other / unclear | Do not recommend until classified |

In **scout**, show the map with sample SKUs + price bands, then ask which type. Then Step 4.

## Step 4 — Deep dive (single type)

1. Re-query with type-specific keywords.
2. Shop table: name | platform | SKU | price | stock | origin | index page | notes
3. Prefer in-stock over cheapest OOS.
4. Separate trial (day/week) vs month.
5. List tool URLs from shop notices under notes — do not install tools unless asked.

## Step 5 — Recommend

1. Trial: cheapest in-stock day/week of confirmed type
2. Regular: cheapest in-stock month of that type
3. Backup: next shop or OOS watch
4. One line: which types were excluded

## Degradation path

1. State what failed.
2. Give exact URLs for the user to open.
3. Label partial data `aggregator-only` / `user-verify-origin`.
4. Forbidden: fake shop tokens, fake stock, made-up prices.

## Output template

```markdown
## Scope
- Product: ...
- Mode: scout | deep | lite
- Confirmed type: ... (or pending)

## Delivery-type map (broad)
| Type | Sample titles | Price band | In-stock shops |

## Shops after lock (deep)
| Shop | SKU | Price | Stock | Links |

## Suggest
1. Trial: ...
2. Month: ...
3. Excluded: ...

## Sources
- aifka / PriceMemo / OpenPrice / airadar + observed time
```

## Additional resources

- Query recipes + aifka JSON tips: [reference.md](reference.md)
- Worked case (Cursor unlimited refill -> type A): [examples.md](examples.md)