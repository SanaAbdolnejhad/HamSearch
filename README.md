# HamSearch

Persian document retrieval with TF-IDF and cosine similarity, implemented in Python using the Hamshahri news dataset.

## Overview

This educational project implements sparse TF-IDF vectors and cosine ranking directly in Python. Hazm handles Persian normalization, tokenization, and lemmatization. The supplied dataset subset contains **5,375 documents** and **50 queries**.

The notebook covers data loading, preprocessing, vocabulary construction, ranking, and retrieval evaluation. Empty or out-of-vocabulary queries return no results. Only positive-score matches are returned, with document IDs breaking score ties.

## Files

| File | Purpose |
| --- | --- |
| `main.ipynb` | Retrieval pipeline, example query, and evaluation |
| `requirements.txt` | Python dependencies |
| `HamshahriData/README.md` | Required local dataset layout |
| `.gitignore` | Excludes local data, generated output, and temporary files |

## Download Dataset

Download the matching dataset from this [GitHub repository](https://github.com/aminariafar/Ranking-HamshahriNewsletter-Documents):

**[Download dataset source ZIP](https://codeload.github.com/aminariafar/Ranking-HamshahriNewsletter-Documents/zip/c4002859436a2d4755f089108c78b4c0ab124008)**

This third-party repository contains a byte-identical copy of the project’s **5,375 documents**, **50 queries**, relevance judgments, and stopword file. The download is pinned to commit `c4002859436a2d4755f089108c78b4c0ab124008`.

1. Extract the downloaded ZIP.
2. Extract `HamshahriCorpus.zip` inside it.
3. Copy only the dataset items into this project using the following paths:

| Downloaded item | Destination in HamSearch |
| --- | --- |
| Extracted `HamshahriCorpus/` | `HamshahriData/HamshahriCorpus/` |
| `Queries/` | `HamshahriData/Queries/` |
| `RelativeAssessment/judgements.txt` | `HamshahriData/RelativeAssesemnt/judgements.txt` |
| `persian_stopwords.txt` | `HamshahriData/persian_stopwords.txt` |

**Rename the downloaded `RelativeAssessment` folder to `RelativeAssesemnt`** when placing it inside `HamshahriData`; the notebook expects that exact spelling. Ignore any `__MACOSX` folder.

Keep `HamshahriData` beside `main.ipynb`. Dataset files remain excluded from Git by `.gitignore`. If you already have the original project dataset, no new download is needed.

## Run

1. Download and place the dataset as described in [Download Dataset](#download-dataset).
2. Open a terminal in this project folder. Use your existing working Python environment, or create a separate one:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m notebook main.ipynb
```

On Windows, activate the environment with `.venv\Scripts\activate` instead.

3. Select **Restart Kernel and Run All Cells**. Run the notebook from the folder containing `HamshahriData`.
4. Change `qi` to select another query by its zero-based row index. Change `TOP_K` to set the evaluation depth.

Dependency versions are not pinned because the exact versions from the successful local run have not been recorded. A fresh environment has not been independently verified. Reuse the working environment for the closest reproduction.

## Method

- Apply the same normalization, tokenization, stopword filtering, and lemmatization pipeline to documents and queries.
- Compute `TF(t, d) = count(t, d) / len(d)`.
- Compute smoothed `IDF(t) = log((N + 1) / (DF(t) + 1)) + 1`.
- Rank documents by cosine similarity between sparse TF-IDF vectors.

## Evaluation

Results reported from the successful local run of the revised notebook on the supplied subset:

| Metric | Value |
| --- | ---: |
| Mean Precision@10 | 0.7860 |
| Mean Recall@10 | 0.3395 |
| Mean F1@10 | 0.3924 |
| MAP@10 | 0.7860 |

Precision@K divides relevant hits by K even when fewer than K documents are returned. AP@K divides the sum of precision values at relevant ranks by `min(R, K)`, where R is the total number of relevant documents for that query. MAP@K is the mean of these AP@K values. With `k=None`, AP instead uses R as its denominator; full MAP requires a full ranking.

Precision, Recall, and F1 are averaged separately over queries. Mean F1 is therefore not necessarily the harmonic mean of mean Precision and mean Recall.

These scores describe the supplied subset and this metric convention, not the complete Hamshahri corpus. The notebook also lists the strongest and weakest queries by AP@K.
