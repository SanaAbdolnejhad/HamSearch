# Local Dataset Setup

Copy these items from the original project into this folder:

| Path | Expected content |
| --- | --- |
| `HamshahriCorpus/` | Year subfolders containing `.ham` documents |
| `Queries/` | Query files named with numeric IDs, such as `1.q` |
| `RelativeAssesemnt/judgements.txt` | Whitespace-separated query ID and relevant document ID pairs |
| `persian_stopwords.txt` | One stopword entry per line |

Keep the original spelling `RelativeAssesemnt`; the notebook uses that path.

Each document’s filename without `.ham` is its ID. The first non-empty line is its title; the remaining lines form its body. Every relevance-judgment document must exist in the corpus, and query IDs must match the judgment groups.

Dataset files are local-only and excluded by `.gitignore`. This repository does not bundle or download them.
