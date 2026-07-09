# Cheminformatics — QSAR & ADMET on rapamycin and its rapalogs

## ⚠️ Major caveat (read first)

**Rapamycin and its rapalogs are the *wrong* molecules for de-novo ADMET ML.** They are
**900–1030 Da macrocycles** (sirolimus, everolimus, temsirolimus, ridaforolimus, zotarolimus)
that sit **outside the applicability domain** of essentially every publicly available trained
ADMET model — which are built on small, drug-like molecules. Concretely, their nearest-neighbour
**Tanimoto similarity to the models' training sets is only ~0.2–0.4** (well below the ~0.3
reliability guideline), so the in-silico ADMET numbers here are **extrapolation, not trustworthy
predictions.** **Measured / label data is decisive.**

This series is therefore, in large part, an **honest demonstration of where ML QSAR/ADMET breaks** —
it computes the applicability domain and says so, rather than trusting the output. QSAR/ADMET-by-ML
is most fruitful for **small, in-domain drug-like molecules**; these macrocycles are a deliberate
stress test. (Only two things transfer: the **potency** model still ranks them roughly right, and
**BBB** is correctly called "non-CNS" — because both are size/similarity-driven.)

## What each notebook does

| Notebook | Axis | What it does | Candid finding |
|---|---|---|---|
| [`rapamycin_qsar`](notebooks/rapamycin_qsar.ipynb) | **Potency** (mTOR) | RF on ~4,300 ChEMBL mTOR IC50s; random **and** scaffold split; predict rapalogs held-out | Qualitatively right (all potent) but **under-predicts the most potent ~1 log**; triage filter, not a ruler |
| [`rapamycin_ADMET_Absorption`](notebooks/rapamycin_ADMET_Absorption.ipynb) | **A** | BCS framing + ESOL solubility + BOILED-Egg; addendum = **Tanimoto applicability-domain check** of HIA/bioavailability/Caco-2 ML | Dissolution-limited (BCS II); ML models **out of domain** (Tanimoto ~0.2–0.4) → not usable; measured decides |
| [`rapamycin_ADMET_Distribution`](notebooks/rapamycin_ADMET_Distribution.ipynb) | **D** | CNS-MPO + BOILED-Egg (BBB); measured Vss/fu/B:P | **BBB call works** (non-CNS); Vss/B:P dominated by **FKBP12-driven RBC sequestration** — un-modelable from structure |
| [`rapamycin_ADMET_Metabolism`](notebooks/rapamycin_ADMET_Metabolism.ipynb) | **M** | CYP3A4/2D6 inhibition + hepatocyte clearance ML (TDC) | Measured: **CYP3A4/3A5 + P-gp substrates**; models out of domain, and *inhibition ≠ substrate* |
| [`rapamycin_ADMET_Excretion`](notebooks/rapamycin_ADMET_Excretion.ipynb) | **E** | Half-life + clearance ML (TDC) | Biliary/fecal, long t½ (~62 h); half-life **barely learnable** (negative R²) and under-predicted |
| [`rapamycin_ADMET_Toxicity`](notebooks/rapamycin_ADMET_Toxicity.ipynb) | **T** | hERG / Ames / DILI / LD50 ML (TDC) | Real toxicity is **on-target mTOR** (immunosuppression, metabolic, stomatitis) — **invisible** to structural QSAR |

Each notebook follows the same shape (Purpose → Steps → Citations), reports its **Tanimoto
applicability domain**, compares in-silico to **measured** data, and cites primary literature in
**ACS style** (retrieved from PubMed). Public data: ChEMBL (potency) and the Therapeutics Data
Commons `admet_group` benchmark (ADMET).

## Conventions

- Notebooks are **stripped of outputs** before commit (`nbstripout`); each step's result is saved as
  a **PNG in `result_<notebook>/`** so results stay viewable on GitHub.
- Data cached to `data/` (git-ignored); reproducible `uv` environment (Python 3.11).

## Quickstart

```bash
uv sync
uv run jupyter lab notebooks/     # select the "Python (cheminformatics_pubData)" kernel
```

If a notebook won't run, check the **kernel** first (wrong kernel → `ModuleNotFoundError`).

## License

MIT — © 2026 Jin Mo Koo.
