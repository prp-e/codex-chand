---
name: codex-chand
description: Show historical price charts with technical indicators, return a one-line quote for a named asset, or show the full bilingual Codex Chand dashboard when invoked without arguments for fiat currencies, crypto, Iranian gold coins, metals, and energy in toman or US dollars. Use for نرخ ارز، دلار آزاد، طلا، رمزارز و نفت when the user wants a price snapshot or chart.
---

# Codex Chand

When invoked without arguments (for example, `$codex-chand` by itself), show the full bilingual dashboard for the entire watchlist. Verify the latest available prices, display the source observation times, and use a dated fallback when necessary. Proceed directly without asking which asset to show.

For an asset request with a date range, historical chart, candles, or technical indicators, use the Historical charts section below. This takes precedence over the single-price mode. For an invocation with no arguments, show the full dashboard. When the inline visualization host exposes `window.openai.sendFollowUpMessage`, render asset names as accessible buttons. On click, request a 90-day historical chart for the exact asset in the selected language and currency, with range and indicator controls. Use plain asset names in standalone contexts without that host API.

For a single named asset after an invocation such as `@codex chand دلار آمریکا` or `$codex-chand USD`, fetch only the requested quote and the open-market USD conversion rate if needed. Reply in one plain-text line with the asset, its quoted quantity, and its price in both toman and US dollars. Use the user's language and the number formatting below. For USD, show the toman price of one dollar and its face value of one US dollar. Example with placeholder price: «۱ دلار آمریکا = {نرخ} تومان | ۱ دلار آمریکا». Attach the source link at the end of the same line when citing fetched data. Give an observation date on that line for delayed quotes or dated fallbacks, or say the quote is unavailable if none is verified. Skip dashboards, tables, headings, and other assets unless the user explicitly requests them.

Use [source notes](sources.md) to collect dated observations. For a dashboard, adapt [the HTML template](assets/market-prices.html). Its embedded observations are a dated example checked on 2026-11-21, with the source dates shown in the dashboard. Replace them and both language versions of the date, source links, and relevant captions for a refresh. Use the old snapshot only when explicitly showing the example or a dated fallback. If a fresh quote is unavailable, label it as unavailable or show the last verified price with its own date.

Keep the established watchlist unless the user narrows it:

- Fiat: USD, CAD, AUD, EUR, GBP, OMR, TRY, SAR, IQD, KWD, CNY, JPY, AED, CHF.
- Crypto: BTC, ETH, USDT, SOL, TRX, XRP, BNB, DOGE, PAXG.
- Metals: world gold and silver per troy ounce, world pure gold per gram, Iranian 18K and 24K gold per gram, melted gold per mesghal, platinum and palladium per troy ounce, monthly copper and aluminum per metric ton.
- Iranian gold coins: Emami, Bahar Azadi, half, quarter, and one-gram coin, each per piece.
- Energy: Brent spot, WTI spot, OPEC basket per barrel, and Henry Hub spot natural gas per million BTU. Dubai monthly is an alternative when requested.

For the chart, keep Persian and toman as defaults. The English toggle translates all labels and dates, switches RTL to LTR and changes digits and separators. Use Persian digits with U+066B (٫) for grouping and ASCII period for decimals per the user's preference. Use standard US formatting in English. Changing language preserves unit and asset category. Include the USD option and label it as an equivalent when converted from a local quote.

Normalize data to toman internally. Divide IRR by 10, and divide any quote for 100 units by 100 before using it as a unit price. Use a single verified Tehran open-market USD rate for the display snapshot. Multiply worldwide USD prices by that rate for toman; divide local toman prices by that rate for a USD equivalent. Derive pure gold per gram from the troy ounce using 31.1034768 grams. Use the local 18K quote directly. Distinguish local crypto quotes, including USDT, from international USD equivalents. Keep the actual observation date of delayed benchmarks and the observation month of monthly averages. Retain the quoted quantity when switching currencies.

For inline Codex presentation, use the visualization surface when available and keep the HTML fragment self-contained with an embedded snapshot. For a standalone HTML deliverable, wrap the fragment in a full document and supply equivalents for host-only styles and theme variables. Keep credentials server-side. Before delivery, verify all requested rows, one local and one global conversion, both number formats, both languages, and the unit switch.

## Historical charts

Default to 90 days. Offer 7, 14, 30, 60, 90, and 180 days, 1, 2, and 5 years, year to date, all available observations, and explicit start and end dates. Label actual coverage and partial observations.

- For local rates, use [NerkhAra history](https://nerkhara.com/en/api/) at `history/{id}.json` or `history/{id}.csv`. The standard daily series contains UTC-day averages. Use the asset page's separately published OHLC when available, or documented exchange OHLC for a matching crypto pair. Use line charts for sampled prices or averages.
- Use [source notes](sources.md) for EIA spot prices, benchmark histories, and World Bank monthly averages. Maintain a consistent provider, benchmark, sampling frequency, and quote basis within each series. Preserve gaps and original timestamps. If data is unavailable, report the available coverage or show a dated fallback.
- Convert with verified Tehran open-market USD rates for matching historical days or aligned monthly averages. Label the FX source and method. Show candles in a converted series only when open, high, low, and close can be calculated from aligned intraperiod data. For USDT-denominated observations, verify the corresponding USDT conversion.
- Show period return, high and low, drawdown from the selected period's peak, maximum drawdown, SMA 20 and 50, and Wilder RSI 14 with 30 and 70 reference levels. Count periods at the series' actual frequency and retain earlier observations for indicator warm-up.
- Build a self-contained interactive chart with language, currency where available, range, indicator, and chart-type controls. Use actual OHLC to enable candles. Verify that controls update the chart while preserving the chosen quote basis.
