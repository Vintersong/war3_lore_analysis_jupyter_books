# Warcraft Lore Analysis

An academic research support tool for analyzing World of Warcraft characters through philosophical archetypes. It automates lore extraction from web sources and novels, clusters passages thematically, and exports data into an Obsidian vault for qualitative coding.

**Characters:** Arthas, Illidan, Thrall
**Keywords:** duty, power, sacrifice, corruption, freedom, justice

---

## File Summary

| File / Folder | Description |
|---|---|
| `warcraft_lore_analysis.ipynb` | Main Jupyter notebook — full pipeline from data collection to clustering |
| `warcraft_passages.csv` | 431 extracted lore passages with character, source, text, matched keywords, and count |
| `warcraft_passages_clustered.csv` | Same passages as above with an added `cluster` column (K-means, k=3) |
| `trait_density_media.png` | Bar chart of keyword density broken down by source medium |
| `PDFs/arthas_rise.pdf` | Novel: *Arthas: Rise of the Lich King* |
| `PDFs/illidan.pdf` | Novel: *Illidan* |
| `PDFs/lord_clans.pdf` | Novel: *Lord of the Clans* |
| `Obsidian Vault/` | 435 markdown notes (one per passage) for browsing and qualitative coding |
| `.gitignore` | Excludes `paper idea.docx` and `keyword_frequency.png` from version control |

---

## Pipeline Overview

The notebook runs a five-step pipeline:

1. **Setup** — Define keywords, characters, and source URLs; import libraries.
2. **Wowpedia scraping** — Scrape 15 character and lore pages using requests + BeautifulSoup; extract ~120 passages.
3. **PDF parsing** — Parse the three novels with PyPDF2; extract ~311 passages.
4. **Keyword analysis** — Count keyword frequencies per character, generate a bar chart, export `warcraft_passages.csv`.
5. **Clustering** — TF-IDF vectorize all passages and apply K-means (k=3); export `warcraft_passages_clustered.csv` and `trait_density_media.png`.

---

## How to Run

**Prerequisites:**

```
pip install requests beautifulsoup4 pandas PyPDF2 matplotlib scikit-learn selenium
```

ChromeDriver is only required if `USE_WOWHEAD = True` (disabled by default).

**Execution:**

Open `warcraft_lore_analysis.ipynb` in Jupyter and run all cells (Ctrl+Shift+F10 / "Run All").

**Configuration flags** (in the Setup cell):

| Flag | Default | Effect |
|---|---|---|
| `USE_PDFS` | `True` | Parse the three PDF novels |
| `USE_WOWHEAD` | `False` | Scrape Wowhead quest pages (adds 30–60 min) |

**Estimated run time:** ~15–20 minutes without Wowhead; 30–60+ minutes with it enabled.

---

## Outputs

| Output | Description |
|---|---|
| `warcraft_passages.csv` | All extracted passages before clustering |
| `warcraft_passages_clustered.csv` | Passages with cluster labels (0, 1, 2) |
| `trait_density_media.png` | Keyword density per medium (novels vs. web vs. cinematics) |
| `keyword_frequency.png` | Keyword counts per character (excluded from git) |

### Cluster summary

| Cluster | Count | Dominant character | Top keywords |
|---|---|---|---|
| 0 | 88 | Thrall (86) | power, freedom, corruption |
| 1 | 226 | Illidan (176) | power, sacrifice, justice |
| 2 | 117 | Arthas (116) | power, duty, sacrifice |

---

## Obsidian Vault Integration

After running the notebook, each passage is available as an individual markdown note in `Obsidian Vault/CSV import/` (435 notes total). Each note's YAML frontmatter contains `char`, `source`, `text`, `keywords`, `count`, and `cluster` fields.

Open the vault in [Obsidian](https://obsidian.md) to browse all passages in a sortable table view (`CSV import.base`) — useful for tagging, annotating, and qualitative coding during academic paper writing.
