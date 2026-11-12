# Report skeleton (10–11 pt). Fill <…> from `outputs/results/*.csv` and `outputs/figures/*`

## 1. Introduction
Task: 300-d word embeddings for Gujarati, Hindi, Marathi, Punjabi, Tamil, Telugu, Urdu, trained from scratch on IndicCorpV2 only;
evaluated by Spearman ρ against Akhtar et al. (2017). Contribution summary: <one sentence on best config and macro ρ>.

## 2. Methodology
- Data & preprocessing: NFC normalisation, ZWJ/ZWNJ removal, Arabic→Urdu letter unification, Unicode-aware tokenisation (keeps matras), sentence split on danda/full stop. Corpus stats: `corpus_stats.csv`.
- Model: FastText skip-gram, dim 300, char n-grams <min_n–max_n>, negative sampling 10, window <w>, epochs <e>, min_count 2, bucket <b>.
- **OOV strategy**: subword composition (mean of character n-gram vectors); multi-token entries averaged; optional all-but-the-top post-processing (d=<d>).
- Mono-lingual vs multilingual training (<which was chosen and why>).
- Disclosure: no pre-trained models, no external corpora; tools: gensim, datasets, regex, numpy, scipy, scikit-learn, matplotlib, pandas.
  Benchmark scores were not used for training; only for evaluation and choosing among <n> pre-declared configurations.

## 3. t-SNE Visualization (100 words per language)
Insert `figures/tsne_all.png` (+ per-language PNGs); mention observed clusters (e.g. animals, finance); word tables in `figures/tsne_<lang>_words.csv`.

## 4. Results
Table: language | scored pairs | Spearman ρ (from `final_spearman.csv`) + macro average.
Table: vocabulary coverage & OOV (from `oov_coverage.csv`): unique tokens, in-vocab, OOV %, pairs with ≥1 OOV, OOV n-gram coverage %.
Note: Marathi public file has 104 scored of 253 pairs.

## 5. Experiments
Table from `experiments.csv`: config × scope (mono/multi) × post-processing d → macro ρ and per-language ρ. Short commentary on what helped.

## 6. Analysis (5–10 examples)
Pick from `analysis_examples.csv` (correct + incorrect). For each: pair, human score, cosine, why (polysemy, rare word, morphology, loan words, OOV…).

## 7. Discussion
Design choices (FastText for morphology-rich languages; subwords for OOV; per-language vs shared model; post-processing).
Limitations: small corpus, script barrier between Hindi/Urdu, polysemy, tiny benchmarks → noisy ρ, config selection done on public data.

## 8. References
- Akhtar et al. (2017) Word Similarity Datasets for Indian Languages: Annotation and Baseline Systems.
- Bojanowski et al. (2017) Enriching Word Vectors with Subword Information.
- Mikolov et al. (2013) Efficient Estimation of Word Representations in Vector Space.
- Mu & Viswanath (2018) All-but-the-Top: Simple and Effective Postprocessing for Word Representations.
- Doddapaneni et al. (2023) IndicXTREME / IndicCorp v2 (data source) — check the exact citation on the dataset card.
- Řehůřek & Sojka (2010) Software Framework for Topic Modelling with Large Corpora (gensim).
