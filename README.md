# Aether

**Paper-first, event-driven multi-module trading lab** for the 18-bot ecosystem architecture.

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> Not a live prop desk. Not investment advice. Live orders are **blocked by default**.

## Features

- **Event-driven control plane** — `system.tick` seeds a FIFO bus; modules react as subscribers
- **Hard risk veto** — execution only consumes approved `risk.decisions`
- **Dynamic pair selection** — filters, multi-factor scores, cointegration / Hurst / half-life candidates
- **Regime as central context** — label + probability distribution stamped on events
- **Feature store** — shared online features for paper / backtest parity path
- **Audit trail** — JSONL risk and rotation log
- **CLI** — paper ticks, backtest, commander text commands

## Quick start

```bash
git clone https://github.com/g-pstar7/aether.git
cd aether
python -m venv .venv

# Windows
.venv\Scripts\activate

# macOS / Linux
source .venv/bin/activate

pip install -r requirements.txt
pip install -e .
pytest -q

python -m aether.cli paper --ticks 50
python -m aether.cli backtest --symbol BTC-USD --days 90
python -m aether.cli cmd "status"
python -m aether.cli live-check
```

## Control-plane cascade

```text
system.tick
  → data.quality
  → regime.updates
  → pairs.selected (+ pair_candidates, metrics)
  → signals.ensemble
  → risk.decisions  →  execution.fills (approved only)
  → risk.cycle_done
  → performance / swarm / heartbeat
```

## Layout

```text
src/aether/
  bus/           queue-backed event bus
  core/          engine, types, feature store, portfolio, config
  modules/       pair selection, regime, signals, …
  risk/          hard veto
  execution/     paper broker + live stub
  backtest/      historical runner
  audit/         JSONL logger
  commander/     text meta-commands
config/          settings.yaml
tests/
```

## Config

Copy `config/settings.example.yaml` → `config/settings.yaml`. Keep:

```yaml
mode: paper
allow_live: false
```

## Metrics ownership

| Owner | Responsibility |
|--------|----------------|
| Each module | Its own metrics (e.g. pair turnover) |
| Performance module | Portfolio Sharpe, drawdown, returns |
| SwarmSight | Aggregate / observe — does not own module math |
| Risk | Veto reasons, heat, drawdown gates |

## Safety

- Default mode is **paper**
- `LiveBrokerStub` refuses real orders even if misconfigured
- You are responsible for law, taxes, exchange terms, and capital

## License

MIT — see [LICENSE](LICENSE).
