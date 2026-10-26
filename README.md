# AI Cardshop Price Search

Portable Markdown skill for **generic** broad-then-narrow price hunts on Chinese AI card shops and consumer markets — plus **write-back** of new brand / 暗号 / channel patterns into the skill itself.

**Version:** 1.3.0  
**Repo:** https://github.com/911218sky/ai-cardshop-price-search

## 30-second start

1. Name any **product** (Cursor 无限续杯, Muse 账号, TRAE 换号, Consensus 学生Pro, …) and constraints.
2. Agent runs **`scout`**: ≥2 aggregators (or expansion), maps **delivery types** A–J.
3. You pick a type → **`deep`** on that type only.
4. Agent **write-backs** new aliases/channels into `reference.md` / `examples.md` when the hunt teaches something new.
5. Re-check origin / 闲鱼 App before paying.

## What it does

- Product-agnostic A–J taxonomy (换号池 / 插件 / 成品号 / 学生Claim / …).
- Aggregators first; expand to 淘宝 / 闲鱼 / 合租 / official / materials when empty.
- Coded-title fan-out when `无限续杯` is banned.
- **Living Product playbook** — each new brand’s channel routing and slang stay in the skill.
- Lowest **in-stock** price within the confirmed type + official anchor.
- Honest degradation (no fake shops).

## When to use

Any AI digital good on 卡网 / 淘宝 / 闲鱼 / 合租.  
Triggers: 比价, 找店, 续杯, 无限续杯, 卡网, 链动, 16688, 闲鱼, 淘宝, 暗号, 学生优惠, aifka, …

## Requirements

- Network to aggregators / marketplaces
- Optional: `cursor-ide-browser` for SPA/CAPTCHA
- No package install required for the skill files

## Installation

### Portable (Downloads / clone)

```text
git clone https://github.com/911218sky/ai-cardshop-price-search.git
```

Point the agent at this folder’s `SKILL.md`, or copy into Cursor skills:

```text
~/.cursor/skills/ai-cardshop-price-search/
```

### Layout

```text
ai-cardshop-price-search/
├── SKILL.md       # workflow + write-back rules
├── reference.md   # URL recipes + Product playbook
├── examples.md    # worked cases (structure only)
├── README.md
├── CHANGELOG.md
└── LICENSE
```

## Files

| File | Purpose |
|------|---------|
| `SKILL.md` | Scout → taxonomy → deep → recommend → write-back |
| `reference.md` | Recipes + **Product playbook** (extend per brand) |
| `examples.md` | Cursor / Consensus / TRAE cases |
| `CHANGELOG.md` | Versions |
| `LICENSE` | AGPL-3.0-or-later |

## After each new product hunt

Agents should update the Product playbook (and optionally `examples.md`) so the next session starts smarter. See **Knowledge write-back** in `SKILL.md`.

## License

GNU Affero General Public License v3.0 or later. See [`LICENSE`](LICENSE).
