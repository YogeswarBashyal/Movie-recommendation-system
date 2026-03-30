# Movie-recommendation-system

A **content-based movie recommendation system** built from scratch using NLP techniques. Given a movie title, genre, or a vague plot description, the system returns the **top 5 most similar movies** by analyzing and comparing plot summaries using TF-IDF vectorization and cosine similarity.

Raw Plot Descriptions
│
▼
Text Cleaning
(lowercase, remove punctuation, stopwords, stemming)
│
▼
TF-IDF Vectorization
(convert text → numeric vectors)
│
▼
Cosine Similarity
(measure angle between movie vectors)
│
▼
Top-5 Recommendations
(ranked by similarity score)

##  Dataset

| Property | Detail |
|----------|--------|
| Source | [HuggingFace — jquigl/imdb-genres](https://huggingface.co/datasets/jquigl/imdb-genres) |
| Split used | train for building model, validation for testing |
| Key fields | title, description (plot summary), genre |
| Size | ~50,000+ movies |

### 4. Run all cells top to bottom
Important: After running the column inspection cell, update TITLE_COL, DESC_COL, and GENRE_COL with the actual column names from your dataset before continuing.

---

---

## How It Works

### Step 1 — Text Preprocessing

Each movie plot description goes through a 4-step cleaning pipeline:

| Step | What it does | Example |
|------|-------------|---------|
| Lowercase | Normalizes case | "Action" → "action" |
| Remove punctuation | Strips non-alphabetic chars | "don't!" → "dont" |
| Remove stopwords | Drops common filler words | removes the, is, a, of |
| Stemming (Porter) | Reduces words to root form | "running" → "run" |

### Step 2 — TF-IDF Vectorization

Why TF-IDF over Bag-of-Words?

- TF (Term Frequency) — how often a word appears in this movie's description
- IDF (Inverse Document Frequency) — penalises words that appear in almost every movie (e.g. film, story, man)
- Result: words that are distinctive to a movie get high weight; generic words get near-zero weight
TF-IDF = TF × IDF
         Words that are UNIQUE to a movie get HIGH scores.
         Words that appear EVERYWHERE get LOW scores.

### Step 3 — Cosine Similarity

Measures the angle between two TF-IDF vectors:

- Score 1.0 → descriptions are nearly identical
- Score 0.5 → moderately similar
- Score 0.0 → completely unrelated

Cosine similarity ignores document length, making it ideal for plot summaries of varying length.

### Step 4 — Recommendation

python
# Three types of input — any combination works:

get_recommendations(title="Inception")

get_recommendations(genre="Thriller",
                    description="man loses memory hunting a killer")

get_recommendations(description="robots take over the world")

get_recommendations(title="The Matrix",
                    genre="Sci-Fi",
                    description="simulated reality and machines")

### By Movie Title
```
Query: "The Dark Knight"

rank  title                    genre             score
────────────────────────────────────────────────────
1     Batman Begins            Action, Crime     0.4123
2     The Dark Knight Rises    Action, Crime     0.3891
3     Watchmen                 Action, Drama     0.3012
4     V for Vendetta           Action            0.2876
5     Joker                    Crime, Drama      0.2611
```

### By Genre + Description
```
Query genre: "Thriller"
Query: "a man loses his mind and manipulates everyone around him"

rank  title                    genre             score
────────────────────────────────────────────────────
1     Shutter Island           Thriller          0.3542
2     Gone Girl                Thriller, Drama   0.3210
3     Black Swan               Drama, Thriller   0.2987
4     The Machinist            Thriller          0.2765
5     Memento                  Mystery           0.2544
```

## Evaluation Metrics

Since this is **unsupervised** (no ground-truth rankings), we use three proxy metrics:

| Metric | What it measures | Ideal |
|--------|-----------------|-------|
| **Genre Consistency** | % of top-5 sharing a genre with the query | Close to `1.0` |
| **Avg Cosine Similarity** | Mean similarity score of top-5 | `0.05 – 0.50` |
| **Catalog Coverage** | How diverse recommendations are across the catalog | Not too low |

---



