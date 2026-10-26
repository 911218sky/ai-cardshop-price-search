# AI Cardshop Price Search

A portable Markdown skill for **broad-then-narrow** price hunts on Chinese AI card-shop aggregators (aifka, PriceMemo, OpenPrice, airadar) and origin shops (16688, ldxp/wzyp).

**Version:** 1.0.0

## 30-second start

1. Give the agent the **product** (e.g. Cursor 无限续杯, Muse 账号) and any constraint (只要 Auto / 要质保 / 只要 16688).
2. Agent runs **`scout`**: harvest ≥2 aggregators, map **delivery types** (A Auto池 / B 插件 / C 成品号 / …).
3. You pick the type (or you already named it) → agent runs **`deep`** on that type only.
4. You get a shop table + trial/month suggestions + source URLs. Re-check origin before paying (CAPTCHA/stock lag).

## What it does

- Treats vague marketing words ("无限续杯") as **multi-type**, not one SKU.
- Indexes **aggregators first**, then jumps to origin shop tokens.
- Forces a **taxonomy map** before recommendations.
- Separates Auto换号池 vs 插件 vs 质保成品号 vs 白号/接码 so prices are comparable.
- Degrades honestly on CAPTCHA / empty channels (no fake shops).

## When to use

- Buying or comparing Muse / Cursor / ChatGPT / Claude / … on 卡网
- Trigger phrases: 比价, 找店, 续杯, 无限续杯, 卡网, 链动, 16688, aifka, pricememo, openprice, airadar

## Requirements

- Network access to aggregators (and optionally origin shops)
- Optional: `cursor-ide-browser` MCP if SPA/CAPTCHA needs a real browser
- No package manager required for the skill files themselves

## Installation

### Cursor personal skill

Copy this folder to:

```text
~/.cursor/skills/ai-cardshop-price-search/
```

### Layout

```text
ai-cardshop-price-search/
├── SKILL.md
├── reference.md
├── examples.md
├── README.md
├── CHANGELOG.md
└── LICENSE
```

Then ask the agent to find shops/prices for a product (e.g. Cursor Auto续杯, Muse 成品号).

## Files

| File | Purpose |
|------|---------|
| `SKILL.md` | Main workflow: scout → taxonomy → confirm → deep → recommend |
| `reference.md` | URL recipes, aifka JSON parse, failure table |
| `examples.md` | Cursor 无限续杯 → type A case study |
| `README.md` | This file |
| `CHANGELOG.md` | Version history |
| `LICENSE` | AGPL-3.0-or-later |

## Scope

General-purpose cardshop hunt. The Cursor refill case in `examples.md` is a worked example, not a mandatory product protocol.

## Related

- Format inspired by [consensus-lit-search](https://github.com/911218sky/consensus-lit-search)

## License

GNU Affero General Public License v3.0 or later. See [`LICENSE`](LICENSE).