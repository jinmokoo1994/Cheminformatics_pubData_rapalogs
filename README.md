# Cheminformatics — public data QSAR

Cheminformatics deliverables built on public bioactivity data (ChEMBL) with RDKit
and scikit-learn. The emphasis is **experimental-scientist judgment** — data
provenance, applicability domain, and honest caveats — over leaderboard numbers.

## Deliverables

| Notebook | What it does |
|---|---|
| [`notebooks/rapamycin_qsar.ipynb`](notebooks/rapamycin_qsar.ipynb) | End-to-end QSAR on **rapamycin** and its **rapalog** library: descriptor calculation → train a pIC50 model on ChEMBL mTOR data → render the analog library (naming the real rapalogs) → predict activity and **test whether predictions recapitulate wet-lab measurements**. |

### rapamycin_qsar — headline finding

Holding the real rapalogs **out of training** and predicting their mTOR potency:

- **Qualitatively recapitulated:** every rapalog is predicted potent and is potent
  in the lab (and all sit inside the model's applicability domain, Tanimoto ~0.84–0.92).
- **Quantitatively limited:** the most potent rapalogs (measured pIC50 ~8.3–8.8) are
  under-predicted by ~1 log unit — a random forest cannot extrapolate past the bulk
  of its training data.
- **Ground-truth is noisy:** temsirolimus's pooled ChEMBL value is implausibly low,
  a reminder that the "measurement" we validate against is itself uncertain.

Conclusion: this QSAR is a sound **triage filter** ("plausibly active + in-domain?"),
not a precise potency oracle — the same discipline as validating an assay before
trusting its readout.

## Quickstart

```bash
uv sync                                          # Python 3.11 environment
uv run jupyter lab notebooks/rapamycin_qsar.ipynb
```

Bioactivity is pulled live from [ChEMBL](https://www.ebi.ac.uk/chembl/) via
`chembl_webresource_client` and cached to `data/` (git-ignored). Notebook outputs
are heavy (embedded figures) — strip them with `nbstripout` before committing.

## License

MIT — © 2026 Jin Mo Koo.
