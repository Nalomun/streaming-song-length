# Has Streaming Made Hit Songs Shorter?
DS4200 final project: Billboard Hot 100 (2000–2023) joined to the Spotify 1M Tracks dataset.

## Run order
1. `01_data_prep.ipynb` cleans both datasets and writes `data/processed/`
2. `02_checkpoint_charts.ipynb` builds the charts into `figures/` (interactive `.html` + static `.png`)

`pip install -r requirements.txt` first. The Billboard file downloads automatically. The Spotify CSV
(`spotify_data.csv`, 176 MB, [Kaggle](https://www.kaggle.com/datasets/amitanshjoshi/spotify-1million-tracks)) goes in `data/raw/` or the
parent folder. It's too big for GitHub, so `data/raw/` is gitignored.

## Processed data (`data/processed/`)
| file | rows | what it is |
|---|---|---|
| `hits_clean.csv` | 10,432 | one row per Hot 100 song that debuted 2000–2023; Spotify fields filled for the 7,779 matched songs (`matched == True`) |
| `all_music_yearly.csv` | 24 | "all music" baseline: yearly median / 25th / 75th percentile track length from all 1.16M Spotify tracks |
| `genre_family_yearly.csv` | 168 | same summaries by genre family × year |

Key columns in `hits_clean.csv`: `debut_year`, `era`, `peak_pos`, `weeks_on_chart`, `top10`, `number_one`,
`chart_outcome` (Top 10 / 11-40 / 41-100), `duration_min`, `duration_band` (<2:30 / 2:30-3:30 / >3:30),
`genre_family` (7 families), Spotify audio features, plus match-quality flags (`match_pass`, `release_gap_yrs`, `after_spotify_scrape`).

## Data caveats
- Match rate is 77% for songs that debuted before July 2023, but coverage drops to about 60% in 2020–21. Several big artists are absent from the Spotify file (Post Malone, Kanye West, Bad Bunny), so recent hip-hop and Latin hits are under-represented among matched songs.
- The Spotify scrape ended in June 2023, so 2023 is partial in both sources and the charts stop at 2022.
- Spotify caps tracks at about 1,000 per genre per year, so track counts reflect collection, not real release volume. Compare medians, not counts.
