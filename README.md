# The Emotion Project — Tweet Labeling Interface

A single-file web app for a human-labeling task on the [dair-ai/emotion](https://huggingface.co/datasets/dair-ai/emotion) dataset (Saravia et al., 2018), built for assignment A1-2.

Participants: read a night-sky "click to open" intro, get a full walkthrough of the task (what the six emotions mean, what to do when several emotions fit or none do, how to use the emotion wheel for narrower emotions), pass a one-question comprehension check, then label 5 randomly-assigned tweets.

## Files

- `index.html` — the entire app (HTML + CSS + JS + the curated tweet data, all in one file). This is all you need to deploy.

## Deploying on GitHub Pages

1. Create a new repo, e.g. `emotion-labeling-interface`.
2. Add `index.html` to the repo root and push it.
3. In the repo, go to **Settings → Pages**, set **Source** to "Deploy from a branch", pick your default branch and the `/ (root)` folder, save.
4. GitHub gives you a URL like `https://<your-username>.github.io/emotion-labeling-interface/` within a minute or two.

To preview locally before pushing, just double-click `index.html` to open it in a browser, or run `python3 -m http.server` in the folder and visit `http://localhost:8000`.

## The dataset

`index.html` embeds 57 tweets pulled from the `train` split of `dair-ai/emotion` (via the Hugging Face dataset viewer), covering all six emotions (anger, fear, joy, love, sadness, surprise) with roughly 8–10 examples each. A few were lightly cleaned up for punctuation/readability; the ground-truth label from the dataset travels with each tweet in the data but is **never shown to participants** — it's only there for your own analysis later (e.g. comparing annotator agreement against the original label).

> Saravia, E., Liu, H-C. T., Huang, Y-H., Wu, J., & Chen, Y-S. (2018). CARER: Contextualized Affect Representations for Emotion Recognition. *Proceedings of EMNLP 2018.*

Two additional example tweets used only in the tutorial (the multi-emotion example and the no-emotion example) were written by hand, not pulled from the dataset, since the assignment asked for specific illustrative cases.

## How responses get recorded (read this before deploying widely)

GitHub Pages only serves static files — there's no server to write a shared database to. So each label a participant submits is saved immediately to **that participant's own browser**, via `localStorage`, as soon as they pick an answer (this is also what makes "Save & Quit" safe — nothing already submitted is ever lost).

Every session has a **"Data"** tab in the nav bar that lists every record saved in that browser — participant alias, tweet ID, tweet text, and label — with buttons to download it all as CSV or JSON. That's what "recording who labeled which tweet with which label" means in practice here.

The catch: this is **per-browser**, not a shared cross-device database. If you and a few classmates all test it on your own laptops, each of you will only see your own labels in your own "Data" tab — there's no automatic syncing between machines. For the assignment's actual requirement (test it yourself once, screenshot the data), this is exactly enough. If you later want a real multi-participant study with everyone's labels landing in one place, the cleanest upgrade is to swap the two functions `getDB()` / `saveDB()` near the bottom of the script for calls to a small hosted store — Firebase Firestore, Supabase, or a Google Sheet behind a Google Apps Script web app are the usual lightweight, free options — the rest of the app doesn't need to change.

## Testing it yourself (for your submission)

1. Open the deployed page (or `index.html` locally).
2. Click through as a participant: envelope → welcome letter → homepage → "Classify" → enter an alias → tutorial → practice tweet → 5 real tweets.
3. Click **"Data"** in the nav bar and screenshot that table — that's your "screenshot of the data collected."

## Notes on the assignment's ethics prompt

The **About** tab includes a short, honest note that classifying human emotion (by AI or by crowdsourced human labelers) is a genuinely debated use case — it's not swept under the rug in the interface itself. The bonus reflection essay isn't included here since that's a separate written piece; happy to help draft it based on the assigned readings if useful.