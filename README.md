# jaechenique.com

Personal academic site built with [Quarto](https://quarto.org) and published with GitHub Pages.

## Before the first publish

1. Add your photo as `images/profile.jpg` (square crop, ~800×800 px works well).
2. Optional: add `images/favicon.png`.
3. Fill in the course titles in `teaching.qmd` (search for `TODO`).

## Publish for the first time

1. Create a free GitHub account, then a new **public** repository (e.g. `jaechenique-site`).
2. Upload everything in this folder (drag and drop on the repo page works), including the hidden `.github` folder.
3. In the repo go to **Settings → Pages → Build and deployment → Source: GitHub Actions**.
4. The "Publish site" action runs automatically; after ~2 minutes the site is live at `https://<your-user>.github.io/jaechenique-site/`.

## Connect jaechenique.com

1. In **Settings → Pages → Custom domain** enter `www.jaechenique.com` and save.
2. At your domain registrar's DNS settings:
   - `www` → CNAME → `<your-user>.github.io`
   - (optional, for the bare domain `jaechenique.com`) four A records: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
3. Wait for DNS to update, then tick **Enforce HTTPS**.

If the domain is currently managed inside Google Sites/Google Domains, change the DNS records there.

## Updating content (the easy part)

Edit a `.qmd` file on github.com (pencil icon), commit, and the site republishes itself.

| To add... | Edit... |
|---|---|
| A paper (published or working) | `publications.yml` → copy an entry and edit it (see below) |
| Policy brief / media | `policy-media.qmd` |
| Courses | `teaching.qmd` |
| New CV | Replace `files/Echenique_CV.pdf` with the new PDF (keep the same file name) |
| Menu items | `_quarto.yml` |

## Preview locally (optional)

Install Quarto, then run `quarto preview` in this folder.

## Adding a paper to the Research page

All papers live in `publications.yml`; `research.qmd` and `pub.ejs` only control how they are displayed.
Copy an entry and edit it. Entries appear in the same order as in the file.

```yaml
# Published article
- type: published
  title: Title of the paper
  authors: Echenique, J. A., Coauthor, A., & Coauthor, B.
  venue: Journal Name, 12(3), 45–67
  year: 2026
  doi: 10.xxxx/xxxxx

# Working paper (the abstract is optional and opens in an expandable box)
- type: working
  area: Mental Health        # groups papers under this heading
  title: Title of the paper
  authors: with Coauthor A and Coauthor B
  status: Submitted          # optional tag (Submitted, In preparation, ...)
  url: https://papers.ssrn.com/...   # optional link
  url_text: SSRN
  abstract: |
    First paragraph.

    Second paragraph.
```

Your name is bolded automatically when it appears as `Echenique, J.` / `Echenique, J. A.` / `J. A. Echenique`.
To move a paper from working to published, change `type` to `published` and add `venue`, `year` and `doi`.
A new `area` name creates a new heading.

## Updating papers from Zotero / a .bib file (optional)

If you keep your publications in Zotero, you can regenerate `publications.yml` instead of editing it by hand:

1. In Zotero: right-click your collection → **Export Collection…** → format **BibTeX** → save as `refs.bib` in this folder.
2. From this folder run:

   ```
   python3 scripts/bib_to_yaml.py refs.bib
   ```

   (add `--dry-run` to preview without writing; the old file is saved as `publications.yml.bak`).

The script **merges** with the existing `publications.yml`: papers are matched by title, and abstracts, areas, status tags and links already in the file are kept when the .bib does not have them. Papers only in the old file are kept too (use `--replace` to drop them).

Optional Zotero **Tags** on an item control the layout:

| Tag | Effect |
|---|---|
| `type:working` or `type:published` | which section it goes in (default: journal + year = published) |
| `area:Mental Health` | heading for working papers |
| `status:Submitted` | small tag next to the title |

The Zotero **Abstract** field becomes the expandable abstract, and text such as "Forthcoming", "Submitted" or "In preparation" in the item's **Extra** field is picked up as the status. Merging with the old file needs PyYAML (`pip install pyyaml`).
