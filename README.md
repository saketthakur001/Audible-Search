# Audible-Search

A small Flask web app for browsing and filtering audiobooks scraped from [Audible](https://www.audible.com), backed by a local SQLite database.

## What it does

- **`audible scrape.py`** — scrapes audiobook search results from Audible (title, subtitle, author, narrator, series, length, release date, language, summary, cover image, link, rating, votes) using `requests` + `BeautifulSoup`, and stores them in a SQLite database (`audible.db`) via a small `AudibleDB` class.
- **`webapp.py`** — a Flask app that loads `audible.db` into a pandas DataFrame and serves a searchable/filterable/sortable, paginated listing at `/`, rendered with `templates/index.html` and `static/style.css`. Supported query params include `search`, `sort_by`, `author`, `narrator`, `series`, `language`, `min_length`, `min_rating`, `min_votes`, `page`, and `per_page`.
- **`audible.db`** — a pre-scraped SQLite database (checked into the repo) with an `audiobooks` table, so the web app works out of the box without running the scraper first.
- **`vercel.json`** — config for deploying `webapp.py` on Vercel using `@vercel/python`.
- **`pytorch tests.ipynb`** and **`test.ipynb`** — scratch/learning notebooks (basic PyTorch tensor exercises, and ad-hoc scraping experiments/snippets). Not part of the app; kept as personal notes/experiments.

## Setup

```bash
pip install -r requirements.txt
```

(`requirements.txt` lists `flask` and `pandas`; the scraper additionally needs `requests` and `beautifulsoup4`, which aren't currently pinned there.)

## Usage

Run the web app (uses the `audible.db` already included in the repo):

```bash
python webapp.py
```

Then open `http://127.0.0.1:5000/` and search/filter the audiobook listing.

To refresh the data yourself, run the scraper (edit the category/genre/page range at the bottom of the file first):

```bash
python "audible scrape.py"
```

This will populate/append to `audible.db`, which `webapp.py` then reads.

## Limitations / notes

- `webapp.py` loads the entire `audiobooks` table into memory once at startup; it won't pick up new rows scraped after the app has started without a restart.
- The scraper's category/genre/language mappings are hardcoded to a handful of values (e.g. Science Fiction & Fantasy, Romance, Mystery/Thriller/Suspense) and would need extending for broader coverage.
- `requirements.txt` doesn't list all of the scraper's actual dependencies (`requests`, `beautifulsoup4`).
- No automated tests.
