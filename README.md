# Operating characteristics of a directional quality metric for judgmental overrides

Reproducibility repository for the Monte Carlo study supporting the Electronic
Companion of the paper

> **When forecasters override the model: a directional quality metric and a
> process-governance framework for judgmental adjustments to a default-risk
> forecast**

currently under **double-blind review** at a peer-reviewed journal in management
mathematics. Author and institution identifiers are intentionally omitted from this
repository for the duration of the review.

---

## What this repository does

The paper proposes a quality metric for one-notch judgmental adjustments to a
model-based default-risk grade, built on the directional ordering of realised
default rates:

- **Metric I** — per-band directional separations of the default rate between the
  *up*, *hold*, and *down* adjustment directions, computable from aggregate
  proportions alone.
- **Metric II** — a *stratified directional Gini*, the accuracy-ratio construction
  applied to the adjustment direction within each model band, computable when
  cell-level counts are available.

Because the field data are disclosed only as aggregate proportions, the metrics
cannot be given sampling intervals from the case itself. This repository supplies
that evidence by simulation: it fixes a data-generating process (DGP) to the
disclosed band-by-direction default rates and establishes, under known ground
truth, how the metrics and the governance decision rule behave as the per-cell
sample size grows.

Three questions are answered:

1. **Power and Type I error of Metric I** — how many firms per cell are needed to
   detect a value-adding or value-destroying arm, and whether the one-sided
   two-proportion test is correctly calibrated.
2. **Sampling behaviour of Metric II** — the bias, standard error, and interval
   coverage of the stratified directional Gini and its arms, and the sample size at
   which the one-number summary stabilises.
3. **Operating characteristics of the decision rule** — how reliably the rule
   recovers a *healthy*, *asymmetric*, or *defective* regime, and the per-cell
   false-flag rate on a genuinely value-adding arm.

The Monte Carlo confirms the analytic sample-size statements in the paper: the
value-adding downgrade signal is recoverable at realistic cell sizes (hundreds to
low thousands of firms), whereas the small upgrade reversal is at the edge of what
aggregate data can resolve (tens of thousands), and the directional Gini is
essentially unbiased with a standard error that falls at the Monte Carlo rate.

## Repository layout

```
.
├── data/
│   ├── disclosed_rates.csv      # DGP input: disclosed default rate by band × direction
│   └── direction_mix.csv        # disclosed up / hold / down proportions
├── notebooks/
│   ├── 01_power_type_i_metric_i.ipynb
│   ├── 02_gini_sampling_distribution.ipynb
│   └── 03_decision_rule_operating_characteristics.ipynb
├── results/
│   ├── figures/                 # grayscale .png and .pdf at 600 dpi
│   └── tables/                  # .csv summaries
├── requirements.txt
└── README.md
```

Each notebook is self-contained: a single code cell, commented in English, that
loads the DGP from `data/`, runs the simulation, and writes figures to
`results/figures/` and tables to `results/tables/`. Figures are grayscale, saved as
both PNG and PDF at 600 dpi, and carry no embedded captions (captions belong with
the manuscript).

## Reproducing the results

```bash
python -m venv .venv && source .venv/bin/activate   # optional
pip install -r requirements.txt

# run each notebook top to bottom, e.g. with Jupyter
jupyter notebook            # then run notebooks/01_… , 02_… , 03_…
# or headless:
jupyter nbconvert --to notebook --execute --inplace notebooks/*.ipynb
```

Each notebook fixes its own random seed, so the figures and tables regenerate
exactly.

## Data provenance

`data/disclosed_rates.csv` and `data/direction_mix.csv` contain only the aggregate
summary statistics disclosed in the paper (realised default rate by grade band and
adjustment direction, and the overall direction mix). No firm-level records are
included or required. The simulation treats these disclosed aggregates as the
ground-truth DGP; it does not attempt to recover any confidential underlying data.

## Notes

- The study is a methodological operating-characteristics exercise. It demonstrates
  how the proposed metrics behave under a realistic DGP; it is not an estimate of
  the institution's own confidential figures.
- Output paths are not printed at runtime, to keep execution logs free of
  machine-specific identifiers.

## License

Released for review and reproducibility. A license will be finalised on
de-anonymisation; until then the code may be used to reproduce and verify the
reported results.
