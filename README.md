# The Emotion Project: tweet labeling interface

A web app for a human-labeling task on the [dair-ai/emotion](https://huggingface.co/datasets/dair-ai/emotion) dataset (Saravia et al., 2018), built for assignment A1-2.

**Live task:** [https://gellerjuliaa.github.io/emotion-labeling-interface/](https://gellerjuliaa.github.io/emotion-labeling-interface/)

## For graders 

1. Open the live link above.
2. Click the envelope to open the intro, then **Continue** on the welcome letter.
3. On the homepage, click **Classify**.
4. Enter any name/alias (or leave it blank — one will be generated for you) and continue.
5. Read through the short tutorial (**Next** through each step), covering what the six emotions mean, what to do when several fit or none do, and how to use the emotion wheel for narrower emotions.
6. Answer the one-question comprehension check on a practice tweet, then click **Start**.
7. Label 5 real tweets, one at a time, by picking one of the six emotion buttons (or a "several fit" / "none fit" option) and clicking **Next**.
8. After the 5th tweet, click **Data** in the nav bar — that table is the record of what was just submitted (participant alias, tweet ID/text, label, timestamp), with buttons to export it as CSV or JSON.


## How responses get recorded

GitHub Pages only serves static files, so there's no shared server-side database. Each label is saved immediately to **the browser doing the labeling**, via `localStorage` — so the Data tab only shows records from that browser/device, not a cross-device pool. If you test it and then reload, your data is still there; if you test it from a different computer, that's a separate, empty Data tab. This is intentional for this assignment's scope (test once yourself, screenshot the Data tab); a real multi-participant deployment would need a small hosted backend (Firebase, Supabase, etc.) in place of `getDB()`/`saveDB()` in the script.

## Files in this repo

- `index.html` — the app itself (HTML + CSS + JS).
- `tweets.json` — the tweet pool the app fetches at startup. **Both files must be deployed together** — `index.html` has no built-in fallback copy of the data.
- `process_data.ipynb` — the notebook that generated `tweets.json` from the Hugging Face dataset.

## Running it locally (optional — only needed to inspect/modify the code)

`tweets.json` is loaded via `fetch()`, which browsers block under `file://`, so you can't just double-click `index.html`. Instead, from this folder, run:

```
python3 -m http.server
```

and open `http://localhost:8000`.

If you redeploy after editing either file, push **both** `index.html` and `tweets.json` — if `tweets.json` is missing or unreachable, the page shows a "Couldn't load tweets.json" error instead of the task.

## The dataset

`tweets.json` holds 200 tweets pulled from the `train` split of `dair-ai/emotion` via `process_data.ipynb` (the first 200 rows, unfiltered), covering all six emotions (anger, fear, joy, love, sadness, surprise) — the split is naturally imbalanced, so counts range from ~8 (surprise) to ~81 (joy) rather than being even. That's well over the assignment's 50-tweet minimum and still covers every category. The ground-truth label travels with each tweet in the data but is **never shown to participants** — it's only there for later analysis (e.g. comparing annotator agreement against the original label).

> Saravia, E., Liu, H-C. T., Huang, Y-H., Wu, J., & Chen, Y-S. (2018). CARER: Contextualized Affect Representations for Emotion Recognition. *Proceedings of EMNLP 2018.*

Two additional example tweets used only in the tutorial (the multi-emotion example and the no-emotion example) were written by hand, not pulled from the dataset.

## Notes on the assignment's ethics prompt

The **About** tab includes a short, honest note that classifying human emotion (by AI or by crowdsourced human labelers) is a genuinely debated use case — it's not swept under the rug in the interface itself.