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

---

# Worked Example 2 — Consensus 学生 Pro (2026-10-26)

> Copy the *expansion* pattern: primary indexes miss → keep going → type map → lowest within type.

## Task

**Goal:** Buy Consensus **学生 Pro** (student-priced Pro), report lowest options.  
**Mode:** scout → expand → deep.

## Scout — primary indexes

| Source | Query | Result |
|--------|-------|--------|
| aifka `?q=consensus` | Consensus | **0 hits** |
| PriceMemo / OpenPrice / airadar | consensus | No usable Consensus SKU |
| aifka `?q=学生` | student SKUs | ChatGPT/Kiro/Codex students — **not** Consensus |

**Do not stop.** Run expansion list from SKILL.md.

## Expansion hits

| Channel | What appeared | Notes |
|---------|---------------|-------|
| Official `consensus.app/pricing` | Pro **$12/mo billed annually ($144/yr)**; Monthly toggle separate; banner **Student/Faculty/Clinician up to 40% off** | Claim via academic email / NPI |
| Official help | Students/faculty **40% off**; clinicians 25% | https://help.consensus.app/en/articles/10064359-how-to-get-a-student-or-clinician-discount |
| Taobao | "consensus ai" 独享/共享/代充 — list from ~¥2.8–5 (often 共享年卡); 独享成品/代充 tiers labeled **15刀** | List price ≠ 独享价 |
| AccountBoy | Private Pro 1mo **$8.59** / 3mo **$22.06** / 1yr **$42.78** | Not official student; third-party account |
| PremLogin | ~**$3.32/mo** (12mo) but page also shows 多人共享 | Classify carefully 共享 vs 独享 |
| aifka edu materials | EDU mail ~¥33–50; Gmail EDU short-rent ~¥0.83 | Materials for type H, not Pro itself |
| aifka `?kind=payment` | visa 5刀卡 ~¥70 ($10.50) etc. | Stripe bind materials |

## Taxonomy for this product

| Type | Meaning here | Floor observed |
|------|--------------|----------------|
| H | Official student 40% on **your** account | Anchor: Pro annual $144 → **~$86.40/yr** after 40%; or use Claim on pricing page for live quote |
| I | EDU mail + VCC + bind stack | Mail ¥33+ + VCC ¥70+ **plus** H checkout — only if user lacks edu/card |
| C | Third-party 独享成品 / 代充 | AccountBoy year **$42.78**; Taobao 15刀/mo tiers |
| Shared | 共享/合租 | Taobao ~¥3–5/yr-looking SKUs; PremLogin ~$3.32/mo — **not** student Pro |

## Lowest-price answer (by type)

1. **True student Pro (H):** Official Claim 40% — verify live checkout; expect ~**$86.4/yr** or ~**$12/mo** student monthly if 40% off $20 (confirm on Claim page). Cheapest **legitimate** path if user is eligible.
2. **Just need Pro access (C 独享):** AccountBoy private year **$42.78** was lowest clear private annual found that day (re-fetch).
3. **Do not call 共享 ¥3–5 "学生Pro"** — wrong type.

## Suggest template used

- If user has school email → **H** official Claim first.  
- If user only wants cheap Pro seat → quote **C** floor + warn not student.  
- Excluded: shared junk marketed as 年卡.

## Sources

- https://consensus.app/pricing/
- https://help.consensus.app/en/articles/10064359-how-to-get-a-student-or-clinician-discount
- https://www.aifka.com/?q=consensus (0)
- https://www.aifka.com/?kind=payment
- https://www.accountboy.com/en-us-usd/buy-Consensus
- https://premlogin.com/zh-TW/product/161
- Taobao listings titled consensus ai 代充/独享