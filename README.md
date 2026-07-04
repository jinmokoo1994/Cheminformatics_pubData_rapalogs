# Cheminformatics — QSAR & ADMET on public data

A portfolio of reproducible cheminformatics notebooks built on public bioactivity
data ([ChEMBL](https://www.ebi.ac.uk/chembl/)) with **RDKit** and **scikit-learn**.
The through-line is **rapamycin (sirolimus) and its rapalog library** (everolimus,
temsirolimus, ridaforolimus, zotarolimus), carried across a **potency QSAR** and a
growing **ADMET** series. The emphasis is experimental-scientist judgment — data
provenance, **applicability domain**, and honest caveats — over leaderboard numbers.

## Notebooks

| Notebook | Focus | Status | Results |
|---|---|---|---|
| [`rapamycin_qsar.ipynb`](notebooks/rapamycin_qsar.ipynb) | Potency QSAR (target activity, mTOR) | ✅ | [`result_rapamycin_qsar/`](result_rapamycin_qsar) |
| [`rapamycin_ADMET_Absorption.ipynb`](notebooks/rapamycin_ADMET_Absorption.ipynb) | ADMET · **A**bsorption | ✅ | [`result_rapamycin_ADMET_Absorption/`](result_rapamycin_ADMET_Absorption) |
| `rapamycin_ADMET_Distribution.ipynb` | ADMET · **D**istribution | ⏳ planned | — |
| `rapamycin_ADMET_Metabolism.ipynb` | ADMET · **M**etabolism | ⏳ planned | — |
| `rapamycin_ADMET_Excretion.ipynb` | ADMET · **E**xcretion | ⏳ planned | — |
| `rapamycin_ADMET_Toxicity.ipynb` | ADMET · **T**oxicity | ⏳ planned | — |

Coming commits complete the **DMET** of ADMET (Distribution, Metabolism, Excretion,
Toxicity) to sit alongside the Absorption notebook already delivered.

---

### `rapamycin_qsar.ipynb` — potency QSAR

*Does the molecule hit its target (mTOR)?* An end-to-end structure→activity model:

1. **Descriptor of rapamycin** — physicochemical panel (it is a beyond–Rule-of-5
   macrocycle: MW 914, cLogP ~6, 3 Ro5 violations) plus the Morgan/ECFP4 fingerprint
   the model uses.
2. **Model from known data** — ~4,300 mTOR (`CHEMBL2842`) IC50 compounds curated to
   median pIC50; a random-forest regressor (held-out R² ≈ 0.75, MAE ≈ 0.44 log units).
3. **Analog library** — the five named rapalogs rendered, plus a hypothetical
   R-group library.
4. **Recapitulation test** — with the rapalogs **held out of training**, predicted
   vs. measured mTOR potency.

**Headline finding:** the model **qualitatively recapitulates** wet-lab (all rapalogs
predicted potent and are potent, all in-domain), but **under-predicts the most potent
by ~1 log unit** — a random forest cannot extrapolate past the bulk of its training
data. A sound triage filter, not a precise potency ruler.

### `rapamycin_ADMET_Absorption.ipynb` — ADMET · Absorption

*Can the molecule get into the body?* A literature-grounded, ACS-cited absorption
assessment for the rapalogs, honest about the limits of in-silico for macrocycles:

1. **Caveat first** — 900–1030 Da macrocycles sit outside the applicability domain of
   trained absorption models; measured data is decisive, and in-silico is used to
   *rationalize and rank formulations*, not to give a final answer.
2. **Rigorous in-silico stack** — BCS framing (limiting mode = **dissolution /
   solubility**, BCS Class II), with a mechanism→model→dataset table.
3. **Measured wet-lab data** — ~14–20% oral bioavailability for sirolimus; P-gp efflux
   + gut CYP3A4 first-pass; formulation responses (solid dispersions; IV; stent-local).
4. **Run the stack** — ESOL solubility + BOILED-Egg permeability: all "practically
   insoluble," all outside the passive-absorption region; correctly ranks
   formulation need, but under-predicts solubility ~1000× (the out-of-domain penalty).

Claims are cited in **ACS style** from primary literature (PubMed); see the notebook's
Citations section.

---

## Conventions

- **Notebooks are stripped of outputs before commit** (`nbstripout`) — git history
  stays lightweight and code-only.
- **Each notebook's step results are saved as PNGs** in a per-notebook folder,
  `result_<notebookname>/`, so results remain viewable on GitHub.
- **Data are cached** to `data/` (ChEMBL pulls; git-ignored) and regenerated in-notebook.
- **Reproducible environment** via `uv` (Python 3.11, pinned `uv.lock`).

## Quickstart

```bash
uv sync                                   # build the environment (Python 3.11)
uv run jupyter lab notebooks/             # open a notebook; select the project kernel
```

If a notebook won't run, check the **kernel** first — select **"Python
(cheminformatics_pubData)"** (or launch with `uv run jupyter lab` from the repo), so
the environment with RDKit + ChEMBL client is used.

## License

MIT — © 2026 Jin Mo Koo.
