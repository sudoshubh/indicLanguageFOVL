# FOVL Assignment — Multilingual word embeddings for Indian languages

## Goal (one paragraph)
Train **300-d word embeddings from scratch on IndicCorpV2 only** (no pre-trained vectors, no external data) that capture
word meaning across Indian languages, and score them by **Spearman correlation between model cosine similarity and
human similarity judgements** (Akhtar et al., 2017; Gujarati, Hindi, Marathi, Punjabi, Tamil, Telugu, Urdu).
Ranking = **macro-average Spearman on a hidden test set**, so the model must embed *any* word, including unseen (OOV) ones.

## Our approach
FastText skip-gram (gensim) with character n-grams, 300-d.
* **OOV**: vector = mean of the word's character n-gram vectors (subword composition); multi-token entries = mean of token vectors.
* **Experiments** (small pre-declared grid): skip-gram vs CBOW, n-gram ranges, window, epochs; **per-language models vs one multilingual model**;
  optional unsupervised "all-but-the-top" post-processing (mean removal + top-d PCs, fitted on corpus vectors only).
* Human scores are only used to *report* and to pick among the declared configurations — never to train vectors.

## Files
| file | purpose |
|---|---|
| `FOVL_embeddings.ipynb` | **the submission notebook** (train, evaluate, export, inference API) |
| `REPORT_TEMPLATE.md` | report skeleton with the required 8 sections |
| `build_notebook.py` | helper that generated the notebook — **not** part of the submission (no .py allowed) |

## How to run (Colab / any machine with internet)
1. Upload `FOVL_embeddings.ipynb`, *Runtime → Run all* (GPU not needed; CPU is fine).
2. Cell "IndicCorpV2 loader" prints the dataset schema — if auto-detection of text/language columns is wrong, edit only that cell.
3. Outputs appear in `outputs/`: `embeddings/*.npy` (N,2,300), `vocab/*`, `models/`, `results/*.csv`, `figures/*`.
4. Write the report (from `REPORT_TEMPLATE.md` + `outputs/results` + `outputs/figures`) to `report.pdf`, then run `make_zip()` (last cell).

Switches (env vars): `FOVL_QUICK=1` fewer experiments · `FOVL_RERUN=1` redo experiments · `FOVL_SMOKE=1` synthetic data, **testing only**.

## Verified so far / not yet verified
* Verified (on a synthetic stand-in corpus, mechanics only): notebook runs end to end; `(N,2,300)` shapes and order; every pair exported
  (Marathi 253 incl. 149 pairs with no human score); Spearman recomputed from the saved `.npy` matches; OOV composition; multi-word entries; unknown-language fallback.
* **Not verified**: real IndicCorpV2 loading (host was blocked in the build sandbox) and any real scores.

## Known gotchas
* Public Marathi file: only 104 of 253 pairs have a score → Spearman uses 104, export covers all 253.
* Gujarati has one line with an extra 4th column; Hindi is comma-separated, others tab-separated (handled).
* Zip size: models dominate (n-gram table ≈ 120 MB per model at `bucket=100k`). Shrink `bucket` or drop `models/mono` extras if the portal has a size cap.
* t-SNE plots label points with numbers (matplotlib cannot shape Indic/Arabic text); word tables are saved next to each figure.
