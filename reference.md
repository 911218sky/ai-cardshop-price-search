# AI Cardshop Price Search — Reference

Query recipes, aggregator parsing tips, and URL patterns. Product-specific numbers go stale; **re-fetch live**.

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
https://www.openprice.cc/card-products/cursor
https://www.openprice.cc/card-products/chatgpt
https://www.openprice.cc/

# airadar (official-tier / 16688 map)
https://airadar.vip/?family=cursor
https://airadar.vip/prices/cursor-pro
https://airadar.vip/prices/cursor-pro?channel=16688
https://airadar.vip/prices/muse-account

# Origin
https://wzyp.cn/shop/{TOKEN}
https://www.16688.com.cn/shop/{TOKEN}
https://www.16688.com.cn/goods/{id}
```

### Brand / slug hints (not exhaustive)

| Product | aifka brand= | OpenPrice / airadar slug ideas |
|---------|--------------|--------------------------------|
| Cursor | cursor | cursor, cursor-pro, cursor-ultra |
| Muse | muse | muse-account |
| ChatGPT | chatgpt | chatgpt-plus |
| Claude | claude | claude-pro |
| Gemini | gemini | gemini |
| Grok | grok | grok |

If slug 404s, start from aggregator homepage search / family page.

## aifka page-bootstrap parse (Node)

```js
// Save HTML then:
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

Shop HTML: grep `go?url=` or `wzyp.cn/shop/` for origin token.

## Type-keyword cheat sheet

| Type | Include | Exclude when hunting this type |
|------|---------|--------------------------------|
| A Auto pool | Auto续杯, 无感换号, 积分卡, N个号 | 不换号插件, 质保成品号 |
| B Plugin | 不换号, 插件计费 | Auto续杯积分卡 (unless same shop sells both — split rows) |
| C Finished | 成品号, 质保, 官方会员, 直充 | 续杯, 插件, 白号 |
| D Quota card | 额度卡, 周期卡, 独享N额度 | Do not merge with A or C until title verified |
| E White acct | 白号, Free Auto 白号 | Treat as materials |
| F SMS | 接码, 实卡 | Not a membership |

## Scout → deep question (copy)

```
我先按交付形态分好了（A Auto换号池 / B 插件不换号 / C 质保成品号 / D 额度卡 / E 白号 / F 接码）。
你要的是哪一种？确认后我只深挖那一类的店和价。
```

## Session capture card (per product hunt)

```
- Product / brand:
- Mode: scout | deep | lite
- Confirmed type:
- Aggregators hit: aifka | PriceMemo | OpenPrice | airadar
- Key URLs:
- Top in-stock SKUs (type-matched):
- OOS watch:
- CAPTCHA / degradation notes:
- Observed at (local time):
```

## Failure table

| Symptom | Likely cause | What to do |
|---------|--------------|------------|
| airadar channel=16688 empty for refill | Wrong product class | Switch to aifka/OpenPrice; say 16688 weak for this SKU |
| wzyp CAPTCHA | Bot check | Use aifka/PriceMemo prices; user opens origin |
| Thin aifka HTML | SPA / bootstrap | Parse `page-bootstrap` JSON |
| Same title, wildly different prices | Different delivery type | Re-read title keywords; split types |
| Cheapest is OOS | Sold out | Prefer next in-stock; list OOS as watch |

## Optional: browser MCP

Prefer `curl` + bootstrap JSON for aifka. Use `cursor-ide-browser` when:

- Need to click through shop UI after CAPTCHA (user may need to solve)
- airadar family page is JS-only

Sequence: `browser_tabs` list → `browser_navigate` → snapshot → extract links → unlock.