# Movie Recommendation System

A small **content-based** movie recommender built with **Pandas**. It matches what you type (genres, moods, keywords) against each movie’s genres and keywords using **cosine similarity** over word tokens, then returns the top matches—breaking ties with higher ratings.

## Requirements

- Python 3.9+ (uses `from __future__ import annotations` and built-in generics)
- [pandas](https://pandas.pydata.org/)

Install dependencies:

```bash
pip install pandas
```

## Run interactively

From the project directory:

```bash
python movie_recommendation_system.py
```

You’ll be prompted for interests (e.g. `sci-fi space thriller mind-bending`). The script prints the top 5 recommendations with title, genres, IMDb-style rating, and similarity score. Answer `yes` / `y` to query again, or anything else to exit.

## Use as a library

```python
from movie_recommendation_system import build_movie_dataset, recommend_movies

df = build_movie_dataset()
results = recommend_movies(df, "horror psychological suspense", top_n=5)
print(results)
```

`recommend_movies` returns a DataFrame with columns: `title`, `genres`, `rating`, `similarity`.

If the interest string has no valid tokens after normalization, it raises `ValueError`.

## How it works

1. **`build_movie_dataset()`** — Returns a fixed in-memory catalog: title, genres, keywords, and rating.
2. **`normalize_tokens()`** — Lowercases text, strips punctuation (keeps letters, digits, spaces, hyphens), splits into tokens.
3. **`user_profile_tokens()`** / **`movie_tokens()`** — Build `Counter` objects from your input and from each movie’s `genres` + `keywords`.
4. **`cosine_similarity()`** — Treats counters as sparse vectors and computes cosine similarity.
5. **`recommend_movies()`** — Scores every row, sorts by `similarity` then `rating`, returns top `N`.

## Dataset

The catalog is **embedded in code** (about a dozen well-known titles) for demos and learning—not a full production database. To recommend from your own data, build a DataFrame with the same columns (`title`, `genres`, `keywords`, `rating`) and pass it to `recommend_movies`.

## File

- `movie_recommendation_system.py` — Dataset, scoring logic, CLI loop, and public helpers above.
