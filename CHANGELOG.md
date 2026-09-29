# Changelog

## 0.2.0 — 2026-09-30

### Added
- Event-driven control plane (`system.tick` → queue drain → module subscribers)
- Feature store (online snapshot + history ring)
- Dynamic pair selection with universe filters, cointegration / Hurst / half-life
- Pair candidates + selection metrics on `pairs.selected`
- Portfolio allocator before risk veto
- Module I/O contracts for the 18-bot ecosystem map
- Risk cycle completion topic; execution only on approved `risk.decisions`

### Safety
- Paper mode default; live execution stub always blocked without explicit config + keys + real adapter

## 0.1.0

- Initial paper lab: modules, risk, paper broker, audit, backtest CLI
