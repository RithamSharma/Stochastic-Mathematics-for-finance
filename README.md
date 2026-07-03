# Stochastic-Mathematics-for-finance
# Exotic Derivatives Pricing & Delta-Hedging Engine

A quantitative finance project that bridges continuous-time stochastic theory with discrete, real-world market execution — pricing multi-variant exotic derivatives, validating them against theoretical bounds via variance-reduced Monte Carlo simulation, and stress-testing a Delta-hedging strategy on historical NIFTY 50 data. The core simulation engine was ported from Python to C++ for high-performance, large-scale trial runs.

## Overview

This project implements the full pipeline from stochastic calculus to backtested trading strategy:

1. **Theory** — Analytical derivation of Geometric Brownian Motion (GBM) and the Black-Scholes PDE.
2. **Simulation** — Numerical solutions to the underlying SDEs via Itô calculus and the Euler-Maruyama scheme.
3. **Pricing** — Exotic option structures priced under the Black-Scholes framework and cross-validated with Monte Carlo methods.
4. **Backtesting** — A Delta-hedging engine run over historical NIFTY 50 price data (via `yfinance`) to quantify hedging PnL bleed.
5. **Performance** — A C++ port of the simulation core for large-scale (1M+ trial) Monte Carlo runs, benchmarked against the Python implementation.

## Key Features

- **Analytical GBM derivation** — closed-form solution of the geometric Brownian motion SDE using Itô's Lemma.
- **Euler-Maruyama SDE solver** — numerical integration scheme for simulating asset price paths under stochastic dynamics.
- **Black-Scholes PDE pricing** — exotic, multi-variant derivative structures priced within the Black-Scholes partial differential equation framework.
- **Variance-reduction Monte Carlo** — simulation loops (antithetic variates / control variates) achieving **>99% accuracy** against theoretical pricing bounds across **100,000 simulated paths**.
- **Delta-hedging backtester** — historical backtest engine over NIFTY 50 constituents to measure hedging error and PnL bleed under discrete rebalancing.
- **C++ performance port** — the simulation engine was re-engineered in C++ with optimized memory management and loop structures for high-throughput execution.
- **Python vs. C++ benchmark report** — a documented performance comparison of both implementations at **1,000,000 simulated trials**.

## Methodology

### 1. Stochastic Process Modeling
The underlying asset price is modeled as a Geometric Brownian Motion:

```
dS_t = μ S_t dt + σ S_t dW_t
```

This SDE is solved analytically using Itô's calculus to obtain the closed-form log-normal solution, and numerically via the **Euler-Maruyama discretization scheme** for path simulation:

```
S_(t+Δt) = S_t + μ S_t Δt + σ S_t √Δt · Z,    Z ~ N(0,1)
```

### 2. Derivative Pricing
Exotic, multi-variant option structures are priced by:
- Solving the **Black-Scholes PDE** for closed-form/semi-analytical benchmarks.
- Running **Monte Carlo simulations** with variance-reduction techniques to estimate prices over large sample paths.
- Validating simulated prices against theoretical bounds (>99% accuracy at 100k paths).

### 3. Delta-Hedging Backtest
A historical backtesting engine:
- Pulls historical NIFTY 50 constituent data via `yfinance`.
- Simulates a Delta-hedged position rebalanced at discrete intervals.
- Quantifies **PnL bleed** arising from discrete hedging vs. continuous-time theory.

### 4. Performance Engineering
The Monte Carlo and hedging simulation core was ported from Python to **C++**, with:
- Optimized loop structures and memory allocation patterns.
- Benchmark comparisons of Python vs. C++ runtimes at 1M simulated trials, compiled into an optimization report.

## Tech Stack

| Layer | Tools |
|---|---|
| Language(s) | Python, C++ |
| Market Data | `yfinance` |
| Numerical / Scientific | NumPy, SciPy |
| Data Handling | Pandas |
| Visualization | Matplotlib |
| Performance | Custom C++ simulation engine |

## Project Structure

```
.
├── theory/                # Analytical derivations (GBM, Ito's calculus, Black-Scholes PDE)
├── src/
│   ├── python/             # Python pricing, Monte Carlo, and Euler-Maruyama implementation
│   └── cpp/                # C++ ported high-performance simulation engine
├── backtest/               # Delta-hedging backtester on historical NIFTY 50 data
├── benchmarks/              # Python vs. C++ performance benchmark report
└── README.md
```

## Results

- **Pricing accuracy:** >99% match between Monte Carlo-simulated prices and theoretical Black-Scholes bounds across 100,000 paths.
- **Hedging analysis:** Quantified PnL bleed from discrete Delta-hedging vs. continuous-time theoretical hedging on historical NIFTY 50 data.
- **Performance:** Documented speed comparison between Python and C++ implementations at 1,000,000 simulated trials, demonstrating the throughput gains from the C++ port.

## Getting Started

### Prerequisites
```bash
pip install numpy scipy pandas matplotlib yfinance
```
A C++17 (or later) compiler is required to build the performance engine (e.g. `g++`, `clang++`).

### Running the Python Simulation
```bash
python src/python/monte_carlo_pricer.py
```

### Building the C++ Engine
```bash
cd src/cpp
g++ -O3 -std=c++17 -o mc_engine main.cpp
./mc_engine
```

### Running the Delta-Hedging Backtest
```bash
python backtest/delta_hedge_backtest.py
```

## Notes

This README is a template generated from a project summary and should be adjusted with actual file names, parameter values, and results specific to the final codebase before publishing.

## License

MIT License
