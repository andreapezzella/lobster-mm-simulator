# MarketMaking-LOBSTER

This repo contains a small project on limit order books using **LOBSTER** data.
I used it to practice market microstructure concepts and to build a simple baseline market making simulation.

The code is mostly in notebooks to keep it easy to follow, but I also save a few figures so that results are visible directly on GitHub.

---

## What’s in the repo

**Notebooks**
- `01_LOBSTER.ipynb`  
  Load LOBSTER message + orderbook files, handle dummy prices, and create a single clean dataframe.
- `02_lob_analytics.ipynb`  
  Basic L1 analytics (mid/spread/microprice/imbalance) and a quick sanity plot.
- `03_ofi_L1.ipynb`  
  Compute OFI at level 1 and compare it with the next-event mid-price change.
- `04_mm_simulator_baseline.ipynb`  
  Baseline market making simulator (event time) with latency (in number of events) and a simple inventory-based size skew.

**Figures**
- `reports/figures/` contains the plots saved from the notebooks.

---

## Data

The notebooks expect the standard LOBSTER format:
- a *message* file
- an *orderbook* file (10 levels in this project)

Example sample (GOOG):
- `GOOG_2012-06-21_34200000_57600000_message_10.csv`
- `GOOG_2012-06-21_34200000_57600000_orderbook_10.csv`

By default I keep the path simple and look for them in `../Data/` relative to the `notebooks/` folder.

Notes:
- Prices are in LOBSTER integer format and I convert them to USD using `/ 10000`.
- Dummy prices (`9999999999` and `-9999999999`) are replaced with `NaN` on price columns.

---

## How to run

### Environment
```bash
python -m venv .venv
source .venv/bin/activate   # mac/linux
pip install -U pip
pip install numpy pandas matplotlib jupyter pyarrow

Simulator limitations
This simulator is meant as a baseline. The goal is to compare simple choices (latency, inventory skew, etc.) rather than to replicate a real exchange.
In particular, I simplify a few things that matter in real market making. For example, I don’t model queue position: being at the best bid/ask does not guarantee you get filled in real life, because fills depend on how much size is ahead of you and how the queue changes. Here fills are approximated using execution events, which is enough for a first version but not “production accurate”.
I also only use the visible book (so anything related to hidden liquidity is out of scope), and I don’t include a full fee/rebate model by default. Latency is implemented as a delay in re-quoting measured in number of events (event time), which is useful to see the effect of being slower, but it’s not a complete network/infra model.
Finally, risk controls are intentionally simple. Inventory is tracked and there is a basic skew mechanism, but there’s no advanced risk engine (volatility regimes, dynamic spread, hedging, etc.).