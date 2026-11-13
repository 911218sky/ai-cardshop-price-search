# Changelog

## 1.5.4 - 2026-11-13

- **Cheapest-in-type playbook:** when user asks 最便宜, do not stop at AiCheap/水獭 “卡网最低” — ladder AI价网 md(成品号) → shop outbound → BestAI Ready-made CDP → independent domains; rank 闪货 vs 有货深度 vs liquid.
- SKILL Lowest-price section + examples case 7 rewritten from live Muse C hunt method (subset-floor trap, multi-quota split).
- Version 1.5.4.

## 1.5.3 - 2026-11-13

- **Machine-readable discovery layer:** AI价网 `/md/*.md` + `sitemap-shops.xml` (CC BY cite), 水獭 `ai-price-mcp`/`llms.txt`, BiWu `/products/{slug}`, 链动/云猫 `goodsList` + ESA caution — teach *how to expand shops*, not build a local mirror.
- License red lines: CardNav/PriceAI discovery-only; stock/dedupe 口径; `catfk.com` origin patterns.
- Muse discovery loop step 0 = AI价网 md/sitemap; checklist 2a/2e; failure-table rows for ESA / fee-vs-shelf / sitemap fan-out.
- Version 1.5.3.

## 1.5.2 - 2026-11-13

- **Recipes over frozen shops:** design principle + write-back rules — keep search/discovery loops, not permanent item/same-source ids or “cheapest shop is X”.
- Muse notes rewritten as **discovery loop** (fan-out → same-source tool → shop crawl → PriceMemo/WebSearch → snapshots → independent); seeds = bootstrap only.
- Version 1.5.2.

## 1.5.1 - 2026-11-13

- **Muse hunt notes:** listed floor ≠ in-stock floor; hunt/timing experience (later generalized in 1.5.2 away from sticky IDs).
- Version 1.5.1.

## 1.5.0 - 2026-11-12

- **PriceAI-class secondary aggregators:** BestAI, PriceAI, AiCheap, shuita, 一可比价, AI张大爷 (aizhangdaye), BoAIle, aiapiprice, AIGOOD, AI价网 — for outbound buy links + cross-check.
- **Extracting origin buy links:** AiCheap hrefs + browser CDP on 前往购买/直达购买; re-classify titles; 16688 WAF note.
- **Muse hunt notes** + playbook refresh (mune typo; invite≠代刷≠成品≠接码; 10B vs 310B; OpenPrice/BoAIle mis-bucket).
- Upstream OSS: `gm365/shuita-ai-price`, `AIprice-club/aiprice`, `dimthink/PriceAI`.
- SKILL: index rows, expansion-when-need-links, Step 2 filters, checklist 2d.
- `examples.md` case 6 — Muse 10B Type C deep.
- Version 1.5.0.


## 1.4.1 - 2026-10-28

- **Cursor hunt notes** (live deep hunt): Type A lives on aifka/ldxp + independent 自助店 (TXUN); **16688 个人超市 ≈ empty for A** (C/F instead).
- **False friends:** 续杯店名+额度卡=D; 不换号插件=B; 接码「换号」=F; aifka `q=换号` alone misleading.
- **Buy rules:** day-card-first when 周月测过不退; cheapest A often OOS → watch + next in-stock.
- Seed catalog: YOUGO / 官方授权 / TXUN / B-shop excludes; 16688 API tips (dedupe, rate-limit).
- `examples.md` case 5 — Cursor Auto 月卡 deep; SKILL deep-step filters.
- Version 1.4.1.

## 1.4.0 - 2026-10-28

- **Upstream projects & sites** in `reference.md`: PriceMemo/aifka/airadar/OpenPrice/16688/nosec + GitHub (`ai_price_radar`, `price-radar`, market-scanner, `ai-goofish*`) for agent study.
- **Discovery channels:** Google / X (Twitter) query recipes; 个人小铺 via `/shops` + 16688 `S*` seeds.
- **Seed shop catalog:** Cursor ldxp tokens (`WCT9GRNG`, `UELAUHQA`, …) and 16688 personal shop source ids (observed 2026-10-28; re-fetch).
- `SKILL.md`: index/expansion for shops directory, X, 16688 personal; ban crack “续杯工具” as indexes.
- `examples.md` case 4 — discovery / reference stack.
- Version 1.4.0.

## 1.3.0 - 2026-10-26

- **Generic skill:** product-agnostic core; examples are optional case studies, not the only brands.
- **Knowledge write-back:** after meaningful hunts, update Product playbook / examples / CHANGELOG (required).
- **Product playbook** in `reference.md`: living catalog (Cursor, Muse, TRAE, Consensus, …) with channels, 暗号, noise, types.
- Modes: add `write-back`; checklist step 9; output template Write-back section.
- README: portable install + write-back expectation; version 1.3.0.

## 1.2.0 - 2026-10-26

- **Coded-title / synonym fan-out** for Taobao/闲鱼 title bans.
- **闲鱼 first-class** for Type A when 卡网 empty.
- **examples.md:** TRAE Type A case (aifka 0 → 淘宝/闲鱼暗号).
- reference: marketplace coded recipes + failure rows.

## 1.1.0 - 2026-10-26

- Expansion protocol when aggregators miss; lowest-price-in-type + official anchor.
- Taxonomy H/I/J; Consensus 学生 Pro example.

## 1.0.0 - 2026-10-26

- Initial broad-then-narrow cardshop skill; Cursor Type A example; AGPL-3.0-or-later.
