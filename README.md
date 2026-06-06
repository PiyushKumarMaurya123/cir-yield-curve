# Stochastic Interest-Rate Modelling and Prediction
### Implementing, Calibrating, and Extending the Cox–Ingersoll–Ross (CIR) Model on Real Yield-Curve Data

**Finance Club, IIT Roorkee — Open Projects 2026**

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/USERNAME/REPONAME/blob/main/CIR_Yield_Curve_Project.ipynb)

> **Replace `USERNAME/REPONAME` in the badge URL above with your own GitHub username and repository name** so the "Open in Colab" button works.

---

## Result

| Metric | Value |
| --- | --- |
| **Out-of-sample pooled R²** (curve reconstructed from the 3M rate) | **0.8842** |
| Pass bar | > 0.85 |
| **Status** | ✅ **PASS** |

The notebook reconstructs the entire **6M → 30Y** yield curve for each day in the held-out test period using **only that day's 3-Month rate** as a proxy for the short rate, and scores it against the actual yields.

## What's in here

```
.
├── CIR_Yield_Curve_Project.ipynb   # the full submission (runs top-to-bottom, outputs pre-rendered)
├── data/
│   ├── train_data.csv
│   ├── test_data.csv
│   └── test_data_3M.csv
└── README.md
```

The notebook is the complete deliverable — code and the written report (markdown cells) in one file:

- **§A — Data engineering:** a `YieldCurveData` class that fixes the headers, parses/dedupes dates, drops non-trading days, interpolates gaps, repairs transient spikes (while preserving genuine 2022 regime moves), and enforces positivity.
- **§B — CIR implementation & calibration:** a `CIRModel` class with the closed-form bond price, the affine `A(τ)`/`B(τ)` functions, short-rate inversion, and exact non-central-χ² simulation. Three calibration methods (time-series OLS, exact MLE, and cross-sectional risk-neutral least-squares) are compared, and the choice is justified.
- **§C — The prediction challenge:** curve reconstruction from the 3M rate alone, with per-maturity out-of-sample scores and diagnostic plots.
- **§D — Extensions:** a Brigo–Mercurio **CIR++** deterministic shift (fully backtested out-of-sample) and a **jump-diffusion** short rate (stress-curve simulation).
- **§E — Critical analysis** and **§6 — Key Questions**, each answered explicitly.

## How to run

**Option 1 — view it (no run needed).** GitHub renders the notebook with all plots and metrics already visible.

**Option 2 — Open in Colab (badge above):** click the badge, then in the **§0.1** cell set
`GITHUB_RAW_BASE = "https://raw.githubusercontent.com/USERNAME/REPONAME/main/data"`
and choose **Runtime → Run all**. The notebook downloads the three CSVs from this repo automatically.

**Option 3 — clone and run locally / in Jupyter:**
```bash
git clone https://github.com/USERNAME/REPONAME.git
cd REPONAME
jupyter notebook CIR_Yield_Curve_Project.ipynb   # then Run all
```
The data files in `data/` are discovered automatically.

## Key finding

The single most important result is a *negative* one: adding fitted complexity (the CIR++ shift) improves the **in-sample** fit (0.9127) but **degrades out-of-sample** performance (0.8127) because it bakes in a training-era rate regime. Under the single-3M-input constraint, the parsimonious base model generalises better — a direct, data-driven answer to the project's "does the extension improve or overfit?" question.
