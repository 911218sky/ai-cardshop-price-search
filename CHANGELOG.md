# Changelog

## 1.1.0 - 2026-10-26

- **Expansion protocol:** if aifka/PriceMemo/OpenPrice/airadar miss, continue through Taobao/闲鱼/合租/official/materials before declaring not found.
- **Lowest-price rule:** report floor within confirmed type + official anchor; flag junk shared floors.
- Taxonomy **H/I/J** for student discount / materials / student-channel finished accounts.
- Index rows: consumer marketplaces, 合租 shops, official Claim, `aifka ?kind=payment`.
- **examples.md:** Consensus 学生 Pro worked case (indexes empty → expand → typed floors).
- **reference.md:** expansion URL recipes + student checklist.
- Version bump to 1.1.0.

## 1.0.0 - 2026-10-26

- Initial release: broad-then-narrow AI cardshop price hunt skill.
- Aggregator index table: aifka, PriceMemo, OpenPrice, airadar + ldxp/16688 origin patterns.
- Delivery-type taxonomy A–G; scout / deep / lite modes; degradation path.
- reference.md: URL recipes, aifka bootstrap parse, failure table.
- examples.md: Cursor 无限续杯 → type A worked case (2026-10-26).
- GNU Affero General Public License v3.0 or later.