# AI Cardshop Price Search — Worked Example (Cursor 无限续杯)

> **Case study only.** Copy the *structure* (scout map → confirm type → deep table).  
> Do **not** treat these 2026-10-26 prices as forever-current — re-fetch.

## Task definition

**Goal:** Find Cursor "无限续杯" shops and prices.  
**User later clarified:** type **A** (Auto account-pool / 无感换号), not plugin, not warranty Pro.  
**Mode:** scout → deep.

## Mistake to avoid (what went wrong first)

Searching only `airadar.vip/prices/cursor-pro?channel=16688` returned empty / official Ultra-tier noise.  
**Lesson:** 无限续杯 lives mainly on **ldxp / aifka / OpenPrice**, not 16688 official Pro listings.

## Scout — delivery-type map (broad)

| Type | Sample titles seen | Price band (then) | Notes |
|------|--------------------|-------------------|-------|
| A | Cursor Auto续杯 积分卡; Auto月卡 50号+无感换号 | day ~¥2.6–3.7; month ~¥22.9–36.8 | Target after user said A |
| B | cursor日卡-不换号-插件 | day ~¥9–11; month ~¥69–78 | Excluded for A |
| C | 正规实付 Cursor Pro 成品号 质保 | ~¥115–160+ | Excluded |
| D | cursor续杯会员店 周期卡/额度卡 | day ¥8.8; month ¥46.8–75.8 | Title claims Pro quota — not clear A |
| E | Cursor Free Auto 白号 | ~¥0.65–1.0 | Materials only |
| F | Cursor 接码 美区 | ~¥1–3 | Registration aid |

**Ask user:** A/B/C/D… → user picked **A**.

## Deep — type A only (observed ~2026-10-26)

| Shop | Platform | SKU | Price | Stock | Origin / index |
|------|----------|-----|-------|-------|----------------|
| YOUGO科技 | ldxp via aifka | auto续杯 日/周/月 积分卡 | ¥3.68 / ¥16.80 / ¥36.80 | in | https://wzyp.cn/shop/PP47XS8K · https://www.aifka.com/shops/1368 |
| AI工具官方授权店铺 | ldxp | Auto 天/周/月卡 | ¥2.60 / ¥15.80 / ¥36.80 | in | https://wzyp.cn/shop/xxdlzs · https://www.aifka.com/shops/318 |
| llz-cursor续杯 | ldxp | Auto月卡 50号 / 300号 | ¥22.90 / ¥99.90 | **OOS** | https://wzyp.cn/shop/UELAUHQA · https://ai.pricememo.cn/shops/UELAUHQA |

Tool pages (not shops): YOUGO https://cc.yougoaigc.com/ ; other shop notice https://www.skhxd.top/

## Suggest (as of that run)

1. **Trial:** AI工具官方授权 天卡 ¥2.60 (or YOUGO 日卡 ¥3.68)
2. **Month in-stock:** YOUGO / 官方授权 ¥36.80
3. **Watch:** llz ¥22.90 when restocked
4. **Excluded:** B plugins, C warranty Pro, D 额度卡店 unless user changes type

## Sources used

- https://www.aifka.com/?brand=cursor&q=auto
- https://ai.pricememo.cn/shops/UELAUHQA
- https://ai.pricememo.cn/shops/WCT9GRNG (classified as D, not A)
- https://www.openprice.cc/card-products/cursor (E/F/B noise; little pure A)
- https://airadar.vip/prices/cursor-pro?channel=16688 (empty for A)

## Stop rules

- User said type A → stop recommending 插件 / 质保成品号
- Origin CAPTCHA → keep aggregator prices + origin links for user check
- OOS cheapest (llz) → do not pretend it is buyable; list as watch