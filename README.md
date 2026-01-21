# MarketMaking-LOBSTER

A small quant/microstructure project built on **LOBSTER** data.  
Pipeline: **load & clean** → **L1 features** → **OFI L1** → **baseline market making simulator** (event-time, latency, inventory skew).

The repo is intentionally notebook-first (easy to read), but outputs are saved as PNGs so results are visible on GitHub without running anything.

---

## What’s inside

**Notebooks**
- `01_LOBSTER.ipynb` — load message + orderbook, handle dummy prices, compute basic top-of-book fields
- `02_lob_analytics.ipynb` — L1 microstructure features (mid/spread/micro/imbalance) + a quick plot
- `03_ofi_L1.ipynb` — OFI L1 + relation with next-event mid-price change (scatter) + normalized OFI
- `04_mm_simulator_baseline.ipynb` — event-driven MM baseline (type-4 executions), latency in events, inventory-based size skew, metrics + curves

**Outputs**
- `reports/figures/` contains the saved plots used below.

---

## Data

The notebooks expect two LOBSTER CSV files (message + orderbook).  
In my case I used the GOOG sample:

- `GOOG_2012-06-21_34200000_57600000_message_10.csv`
- `GOOG_2012-06-21_34200000_57600000_orderbook_10.csv`

By default the notebooks look for them in `../Data/` relative to `notebooks/`.

Notes:
- Dummy prices are handled (`9999999999` / `-9999999999`) by replacing them with `NaN` on price columns.
- Full LOBSTER datasets are big: keep only small samples in the repo.

---

## Setup

Minimal environment:

```bash
python -m venv .venv
source .venv/bin/activate   # mac/linux
pip install -U pip
pip install numpy pandas matplotlib jupyter pyarrow

## Simulator limitations (important)

This is a baseline backtest built to be readable and consistent with LOBSTER event updates.  
Results should be interpreted as *directional* rather than production-grade.

Key limitations:
- **No queue position model**: a posted quote is assumed to fill when an execution crosses the level; real fills depend on priority and order flow.
- **No hidden liquidity / iceberg orders**: LOBSTER reflects visible book only.
- **No maker/taker fees or rebates** (unless added): PnL ignores exchange micro-fees and rebate structure.
- **No latency in price formation**: latency is modeled in *events* (re-quote delay), not as a full network/processing model.
- **Partial fill / slippage is simplified**: fills are derived from type-4 executions; microstructure frictions are approximated.
- **No dynamic risk controls**: inventory limits / penalties are simple; no volatility regime switching.
- **Single-instrument only**: no cross-asset hedging or multi-venue routing.
- **No train/test split**: parameters are not calibrated out-of-sample.

