# LOBSTER Limit Order Book Analytics & Market Making Simulator

This project uses LOBSTER Level-10 (limit order book) data to:
- load and align **message** and **orderbook** files (event-by-event),
- compute core microstructure features (spread, midprice, microprice, imbalance),
- build an event-driven **market making simulator** (baseline + inventory-aware variants).

## Repository structure
- `Code/`: Jupyter notebooks (data loading, analytics, experiments)
- `Class/`: reusable Python code (loader, features, simulator) *(work in progress)*
- `Data/`: local datasets (**excluded from git** via `.gitignore`)

## How to run
1. Create/activate your Python environment (example: `anna5`)
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
