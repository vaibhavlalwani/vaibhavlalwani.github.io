# Vaibhav Lalwani — academic website

A fast, chic, professional academic website built with [Quarto](https://quarto.org).
The **Research and Publications** page refreshes automatically every quarter from
[OpenAlex](https://openalex.org) using your ORCID, while every page remains plain
Markdown that you can edit by hand at any time.

Live site: **https://vaibhavlalwani.github.io/**

---

## What's here

| Path | Purpose |
|---|---|
| `_quarto.yml` | Site title, navigation, footer, theme |
| `index.qmd` | **Bio** page (home) |
| `teaching.qmd` | **Teaching** page |
| `research.qmd` | **Research and Publications** page (auto-filled) |
| `data-code.qmd` | **Data and Code** page |
| `styles.css` | Colours, fonts, and layout — edit freely |
| `_includes/head.html` | Fonts and `<head>` additions |
| `scripts/update_publications.py` | Pulls your works from OpenAlex |
| `scripts/settings.toml` | ORCID, hidden works, manual links, and notes |
| `scripts/pitches.toml` | Short plain-text pitch shown beside each paper |
| `_publications.md`, `publications.bib` | **Auto-generated — do not edit by hand** |
| `.github/workflows/publish.yml` | Quarterly update + deploy |

## Local preview

1. Install Quarto: <https://quarto.org/docs/get-started/>
2. Install Python 3.11+ (already required only for the update script).
3. Generate the publication list (optional — a copy is committed):

   ```bash
   python scripts/update_publications.py
   ```

4. Preview the site live:

   ```bash
   quarto preview
   ```

5. Build the static site into `_site/`:

   ```bash
   quarto render
   ```

## How the automatic update works

- `.github/workflows/publish.yml` runs on the 1st of January, April, July and
  October (and on every push, and on demand from the **Actions** tab).
- The script queries OpenAlex for `orcid = "0000-0001-7476-5311"`, splits the
  results into **peer-reviewed publications** and **working papers**, and writes
  `_publications.md` and `publications.bib`.
- If anything changed, the bot commits it, then Quarto rebuilds and publishes to
  the `gh-pages` branch.
- Nothing is scraped from Google Scholar (which has no API and forbids scraping);
  Google Scholar stays linked in the navigation bar as a profile.

## Making manual edits

**Everyday edits** (bio text, teaching courses, data/code links): open the matching
`.qmd` file and edit the Markdown. Commit and push — the site updates.

**Tweaks to the auto-generated list** — edit `scripts/settings.toml`, never
`_publications.md`:

- Hide a duplicate or unwanted entry:

  ```toml
  exclude_ids = ["W7134948737"]
  ```

- Add a PDF, code, dataset or slide link to a paper (use the OpenAlex id shown in
  the URL of its entry, e.g. `W7213254616`):

  ```toml
  [links.W7213254616]
  pdf = "https://example.org/paper.pdf"
  code = "https://github.com/you/repo"
  ```

- Add a note under a paper:

  ```toml
  [notes]
  W7213254616 = "Forthcoming."
  ```

- Edit the short pitch shown to the right of a paper (keyed by OpenAlex id):

  ```toml
  [pitches]
  W7213254616 = "One or two plain sentences about the paper."
  ```

Then re-run `python scripts/update_publications.py`.

## Replacing the photo

Put your photo in `assets/` (e.g. `assets/profile.jpg`) and update the `<img>` in
`index.qmd` to point at it. The current `assets/profile.svg` is a placeholder.

## Deploying to GitHub Pages

This site is published to the user-site repository
`vaibhavlalwani/vaibhavlalwani.github.io` and served at
**https://vaibhavlalwani.github.io/**.

1. Push this folder to the `main` branch of that repository.
2. The `.github/workflows/publish.yml` action renders the site and pushes it to
   the `gh-pages` branch.
3. In **Settings → Pages**, set **Source** to the `gh-pages` branch.
4. Tick **Enforce HTTPS** once the certificate is issued.

No custom domain or DNS changes are required.

## Changing colours / fonts

Everything is in `styles.css` under `:root` (`--navy`, `--accent`, `--muted`, …)
and in `_includes/head.html` for the Google Fonts.
