# Codex Chand | کدکس چند

Codex Chand is a bilingual Codex skill for checking Tehran open-market exchange rates, cryptocurrencies, precious metals, and crude oil. Ask for one asset to get a one-line quote in toman and US dollars, or invoke the skill by itself to see the full watchlist.

## Install

Clone the repository into your Codex skills directory. For this private repository, use GitHub credentials with read access.

```sh
mkdir -p ~/.codex/skills
git clone https://github.com/prp-e/codex-chand.git ~/.codex/skills/codex-chand
```

If you already have a `codex-chand` folder, review or back up your local copy before updating it. Keep the source notes and the `assets` folder beside `SKILL.md` so the skill can resolve its relative links. If Codex does not list the skill immediately, restart the session.

## Usage

| Prompt | Result |
| --- | --- |
| `$codex-chand` | Full bilingual dashboard with Persian and toman selected by default |
| `$codex-chand USD` | Tehran open-market price for one US dollar in toman |
| `$codex-chand BTC` | One Bitcoin quote in toman and US dollars |
| `$codex-chand طلای ۱۸ عیار` | Iranian 18-karat gold price per gram in toman and US dollars |
| `$codex-chand Brent` | Brent spot price per barrel with the observation date when delayed |

Single-asset replies use the language of the request. In the dashboard, switch between Persian and English or between toman and US dollars. The language toggle updates labels, dates, number formatting, and reading direction while preserving the selected category and currency.

## Default watchlist

| Category | Assets |
| --- | --- |
| Fiat | USD, CAD, AUD, EUR, GBP, OMR, TRY, SAR, IQD, KWD, CNY, JPY |
| Crypto | BTC, ETH, USDT, SOL, TRX |
| Metals | World gold and silver per troy ounce, world pure gold per gram, Iranian 18K gold per gram |
| Oil | Brent spot, WTI spot, OPEC basket per barrel. Monthly Dubai crude is available on request. |

## Data and conversions

- **Fiat and local markets.** The skill uses Tehran open-market USD for currency conversion and distinguishes direct quotes from derived cross rates. Local cryptocurrency prices can differ from international prices, including for USDT.
- **Currency units.** One toman equals ten Iranian rials. Quotes published per 100 units are normalized to a single unit. The dashboard uses one verified Tehran USD rate per snapshot for derived equivalents.
- **Metals.** World pure gold per gram is derived using 31.1034768 grams per troy ounce. Iranian 18K gold uses its own local per-gram quote.
- **Freshness.** Codex checks sources when invoked. Delayed benchmarks retain their own observation dates. If a fresh quote cannot be verified, the skill shows a dated fallback or marks the quote unavailable.

The source notes document providers, quote units, timestamps, and alternatives. Availability depends on Codex's network access and the providers. Some optional sources require credentials. Keep credentials out of the dashboard HTML and repository. [Market data sources](sources.md)

## Repository contents

| File | Purpose |
| --- | --- |
| [SKILL.md](SKILL.md) | Invocation behavior, watchlist, display rules, and conversion logic |
| [sources.md](sources.md) | Provider documentation and guidance on selecting and validating observations |
| [assets/market-prices.html](assets/market-prices.html) | Interactive dashboard fragment containing a dated example from November 19, 2026 |

The template shows an embedded snapshot. To refresh it, Codex replaces the observations, dates in both languages, captions, and source links. For a standalone browser export, ask Codex to wrap the fragment in a complete HTML document and supply the hosting styles and theme variables it needs.
