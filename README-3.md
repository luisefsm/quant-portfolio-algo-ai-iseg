# A Dynamic AI-Assisted Algorithmic Framework for Portfolio Construction and Tail-Risk Hedging

### An Application to the Olive Oil Sector

MSc in Finance (MFW) dissertation — ISEG, University of Lisbon.

This repository contains the full algorithmic pipeline developed for the dissertation: a reproducible framework that constructs a multi-asset portfolio around the olive oil sector and hedges its downside (tail) risk, combining classical optimization with a Monte Carlo jump-diffusion engine and an AI-assisted regime tilt.

---

## Overview

The framework integrates five components into a single end-to-end pipeline:

1. **CRITIC weighting** — objective, correlation-aware criteria weighting used to score and screen the asset universe.
2. **Merton Jump-Diffusion simulation** — Monte Carlo paths that capture both diffusive moves and discontinuous jumps in returns, used to model the loss distribution.
3. **Differential Evolution CVaR minimization** — a global optimizer that minimizes Conditional Value-at-Risk (Expected Shortfall) as the sole tail-risk objective.
4. **Markowitz optimization** — mean–variance optimization as the classical benchmark / building block.
5. **AI-assisted regime tilt** — an LLM produces a regime score in the range [−1, +1], which is mapped to per-criterion multipliers (geometric mean = 1, bounds [0.60, 1.60]) to tilt the allocation according to the prevailing market regime.

Common random numbers are reused across the simulation so that optimization and reporting are performed on the same scenario set.

## Data

- **Period:** January 2010 – December 2025 (192 monthly observations).
- **Source:** Yahoo Finance for asset prices; FRED (`DGS3MO`, 3-month Treasury) for the risk-free rate.
- **Commodity exposure via ETF proxies:** GLD, SLV, DJP, GSG, DBB, DBC.

## Repository structure

```
.
├── olive_oil_portfolio_framework.ipynb   # Main notebook (full pipeline)
├── requirements.txt                      # Python dependencies
├── .gitignore
└── README.md
```

## Getting started

### 1. Install dependencies

```
pip install -r requirements.txt
```

### 2. Set your OpenAI API key (required for the AI regime tilt)

The notebook reads the key from the `OPENAI_API_KEY` environment variable and never stores it in code. Set it in your shell before launching Jupyter:

```
export OPENAI_API_KEY="sk-..."     # macOS / Linux
setx OPENAI_API_KEY "sk-..."       # Windows
```

If the variable is not set, the notebook will prompt for the key securely at runtime. **Never commit your key.** A `.env` entry and `*.key` files are already excluded via `.gitignore`.

### 3. Run

```
jupyter notebook olive_oil_portfolio_framework.ipynb
```

Run the cells top to bottom. The notebook fetches data, runs the simulation and optimizations, applies the AI tilt, and produces all result tables and figures.

## Requirements

Python 3.10+ with: `numpy`, `pandas`, `scipy`, `matplotlib`, `arch`, `requests`, `pandas-datareader`, `openai`, `ipython`, `jupyter`.

## Author

Luís — MSc in Finance (MFW), ISEG, University of Lisbon.
The topic is motivated by an olive grove in Macedo de Cavaleiros (Trás-os-Montes), linking real-sector exposure to the quantitative framework.

## License

All rights reserved. This code accompanies an academic dissertation and may not be reused, redistributed, or used commercially without the author's explicit permission.
