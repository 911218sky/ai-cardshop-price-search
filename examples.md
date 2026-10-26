# AI Cardshop Price Search — Worked Examples

> **Structure only.** Copy scout → confirm type → deep → **write-back**.  
> Prices are snapshots — **re-fetch**. Add a new section when a hunt teaches a new channel/slang pattern.

| Case | Product | Lesson |
|------|---------|--------|
| 1 | Cursor 无限续杯 → A | 卡网-native Type A; airadar Pro ≠ 续杯 |
| 2 | Consensus 学生 Pro | aifka 0 → official Claim + 合租; H vs Shared |
| 3 | TRAE Type A | aifka 0 → 淘宝/闲鱼 + 暗号 (会议系统) |

---

# Worked Example 1 — Cursor 无限续杯

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

---

# Worked Example 3 — TRAE Type A 换号/无限续杯 (2026-10-26)

> Copy when **卡网 0 hits** and supply lives on **淘宝 + 闲鱼** with **coded titles**.

## Task

**Goal:** TRAE (user typed "trea") **无限续杯** → user locked **Type A** (换号池).  
**Mode:** scout → expand (synonyms) → deep A.

## Scout — primary indexes

| Source | Query | Result |
|--------|-------|--------|
| aifka `?q=trae` / `?q=trea` | TRAE | **0 hits** |
| PriceMemo / airadar | trae | No usable Type A SKU |
| Official docs | 积分 / Dollar Usage | No official "无限续杯"; CN Pro ~¥89–99; intl Pro ~$20 |

**Do not stop.** Expand + **synonym fan-out**.

## Synonym fan-out (what actually hit)

| Query / fragment | Where | Notes |
|------------------|-------|-------|
| `无限续杯` + TRAE | 淘宝 | Works for some shops; others soften |
| `会议系统` | 淘宝 | Soft Type A title — same sellers |
| `按时间付费` + 换号 + 保留上下文 | 淘宝 | Clear A signal |
| `traex` / `solo` / `切号保留上下文` | 淘宝 | Soft / feature-coded |
| `闲鱼` + trae 续杯 / 国际版 | Web → 闲鱼 | Partial index; **open 闲鱼 app** for live nicks |
| Literal `TRAE` alone | 淘宝 | **Noise:** TRAE YOUNG 球鞋 — filter out |

## Deep — Type A only (marketplace; ~2026-10-26)

| Shop / signal | Platform | SKU signal | Price | Volume | Notes |
|---------------|----------|------------|-------|--------|-------|
| ai小王的店铺 | 淘宝 | 独享国际版 · 无限token · 换号 · 保留上下文 · **按时间付费** | **¥5** | **2000+** | Best volume Type A try |
| ai小王的店铺 | 淘宝 | 无限token · **会议系统** | ¥5 | 400+ | Same shop, soft title |
| 小晨Ai服务 | 淘宝 | 不限量 · 续杯会议 · 兼 CN | **¥3.9** | low | Ask intl vs CN |
| (list) | 淘宝 | 不限量 · 切上下文 · **Win only** | ¥3.9 / ¥3.5 | — | Confirm OS |
| (list) | 淘宝 | 额度号 + 管理工具 · **CN** | ¥29.9 | — | Tool+account bundle |
| (list) | 淘宝 | 国际版 **换号插件** | ¥39.99 | 66 | Often closer to B — split |
| 闲鱼 | 闲鱼 | 同类暗号：切号 / 国际版 / 会议 / 按天 | ~¥3.5–5 band (re-check live) | varies | Public web thin; user verify in app |

卡网：**no** ldxp/16688 Auto 积分卡 equivalent found that day.

## Suggest

1. **Trial A:** 淘宝 **ai小王 ¥5** (highest 付款量) or ¥3.9 soft-title shops after confirming 国际版+换号.  
2. **闲鱼:** search same codewords in-app; prefer sellers with recent 评价; expect same ¥3.5–5 band.  
3. **Excluded:** AccountBoy Pro ¥110 (Type C); 淘宝 ¥24.8「代充」(suspicious vs official); official Pro (not Type A).  
4. **Risk:** TRAE device multi-account limits (~24h freeze); 切号后上下文官方不跟账号走.

## Sources

- https://www.aifka.com/?q=trae (0)
- https://www.taobao.com/list/product/trae.htm
- https://guangtao.taobao.com/product-9de4ae803e19a05328af641cbeeac04764ed93e78734948f45cc22ff6416d5c6.html
- https://docs.trae.cn/ide_plans-and-billing
- WebSearch: 闲鱼 TRAE 续杯 / 会议系统 / 切号

## Skill lesson captured

- Empty aifka ≠ no Type A — check **淘宝+闲鱼+暗号**.  
- Soft titles (`会议系统`) can be the real listing after bans.  
- Always list **which codewords hit** in Sources.

## Write-back done (pattern)

- Product playbook: TRAE — channels=淘宝+闲鱼; aifka often 0; aliases=trea/traex/会议系统/按时间付费  
- Generic coded table: 会议系统, 按时间付费  
- Noise: TRAE YOUNG sneakers