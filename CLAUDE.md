# CLAUDE.md

Conventions for this repo. Read this before making any change here.

## What this repo is

A small personal reference site published with **GitHub Pages**. Recipes are
the first section; more sections will follow (Dutch taxes, notes on cities,
useful websites for finding housing). It is built by GitHub Pages' own Jekyll
build from the default branch — no build step, no Actions workflow.

- Live site: <https://frantrucco-proton.github.io/frantrucco-proton/>
- This repo is also the GitHub **profile README** repo, so `README.md` shows on
  the profile page. Keep it short and presentable.

**The live site is the only output.** Never build an offline bundle, a
single-file HTML export, a PDF, or a zip of the site. If something should be
readable, it goes on the site.

## Repo layout

```
_config.yml            site settings (title, baseurl, markdown)
_layouts/
  default.html         page shell: header, auto-built nav, footer
  home.html            front page: lists every section
  section.html         a section index: auto-lists the pages in its folder
  page.html            a plain content page
  recipe.html          a recipe page
assets/css/style.css   the entire design (no theme gem, no framework, no web fonts)
index.md               front page
recipes/
  index.md             the Recipes section index
  websites.md          sites to search for recipes
  <slug>.md            one file per recipe
```

## Theme

Hand-rolled minimal layouts plus one ~330-line stylesheet, rather than Minima,
Hydejack, or al-folio. The reasons: no theme gem or `remote_theme` to pin or
outgrow, GitHub Pages builds it natively with zero configuration, and the
mobile typography is set directly instead of being overridden through someone
else's SCSS variables.

Design rules that must survive any change:

- **Mobile first.** 17px/1.65 body type on phones, 18px from 40em up.
- **No horizontal scrolling at any width.** Long words wrap, source URLs use
  `overflow-wrap: anywhere`, media is `max-width: 100%`, `pre` and `table`
  scroll inside themselves.
- **Tap-friendly.** Entry links in a list are a full-width block of at least
  3.25rem (~99px in practice), never a bare inline link.
- Light and dark both come from `prefers-color-scheme`; every colour is a CSS
  custom property at the top of the stylesheet.
- No web fonts, no JavaScript, no external requests.

## Jekyll gotchas that will bite you

- GitHub Pages builds with **Jekyll 3.x**, whose `where_exp` accepts **one**
  comparison only — `"a and b"` raises a Liquid syntax error. Chain separate
  `where_exp` filters instead (see `_layouts/section.html`).
- The site is a *project* site, so it is served under `/frantrucco-proton/`.
  Link internally with `{{ '/some/path/' | relative_url }}` — a bare `/path/`
  breaks in production while looking fine locally.
- `permalink: pretty`, so `recipes/leczo-warzywne.md` becomes
  `/recipes/leczo-warzywne/`.

## Adding a recipe

1. Create `recipes/<slug>.md`. The slug is lowercase ASCII with hyphens and no
   diacritics: "Leczo warzywne" becomes `recipes/leczo-warzywne.md`.
2. Fill in the template below.
3. Run `bundle exec jekyll build` and confirm the recipe appears on
   `_site/recipes/index.html` and that its own page rendered.
4. Commit the recipe file in the **same commit** as anything else needed to
   make it appear on the site.

Nothing needs to be registered anywhere: `_layouts/section.html` lists every
page in `recipes/` automatically, sorted by title. A recipe shows up as long as
its file is in `recipes/`, has `layout: recipe`, and has a `title`.

### The recipe file template

Copy this exactly. The order of the sections is fixed.

```markdown
---
layout: recipe
title: Leczo warzywne
description: Polish pepper and courgette stew          # one line, shown in the list
cuisine: Polish
prep_time: 15 min                                      # omit any field the source lacks
cook_time: 40 min
servings: 4
diet: [vegetarian, vegan]                              # a YAML list; omit if the source has no tags
source_url: https://aniagotuje.pl/przepis/leczo-warzywne
source_lang: Polish
---

## Ingredients (English)

- ...

## Ingredients (Dutch)

- ...

## Method

1. ...

## Substitutions and notes                             # only when there is something to say

- ...
```

This produces the required page order: **title with times, servings and diet
tags → English ingredients → Dutch ingredients → method in English → source
URL**. Items 1 and 5 come from front matter and are rendered by
`_layouts/recipe.html`; do not repeat the title or the source link in the body.

### Recipe rules

- **No tables. Ever.** Ingredients are bulleted lists, method steps are a
  numbered list. This holds for both language versions.
- The two ingredient lists must be the same ingredients in the same order, so
  they can be read side by side.
- Method is English only.
- **Translate from the source language, not from a guess.** Check the original
  words rather than reasoning from a rough English translation, especially
  mushrooms, fish, and cuts of meat, where a wrong guess produces a plausible
  but wrong ingredient (Polish `podgrzybek` is a bay bolete, not "brown
  mushroom"; `karkówka` is pork collar/neck, not "pork chop"). Dutch names come
  from what Dutch supermarkets actually print on the packet — `runderriblappen`,
  `speklapjes`, `sperziebonen` — not a literal rendering of the English.
- **Flag anything hard to find in the Netherlands** in `Substitutions and
  notes`, with a concrete substitute and where to look (Polish shop, toko,
  Turkish supermarket). Example: Polish `twaróg` — closest is Dutch
  `kwark`/`hüttenkäse`, or the real thing from a Polish shop.
- Keep quantities in metric, as the source has them.

## Finding a recipe

`recipes/websites.md` is the list of sites to search **first**, in order,
before looking anywhere else. Add new sites there as bullets.

If a site cannot be reached — the sandbox egress proxy blocks some domains with
a 403 — say so plainly and ask for the recipe text. **Never invent recipe
content and attribute it to a source URL.** A recipe page carries a link to the
original, so its contents have to actually match that original.

## Adding a new section

One file. Create `<section>/index.md`:

```markdown
---
layout: section
title: Dutch taxes
order: 2                                    # position in the nav and on the front page
description: Working out what I owe and when.
empty_message: Nothing here yet.
---

Optional intro paragraph.
```

That is all. The section appears in the header nav and on the front page
automatically, and every page you drop in that folder is listed on it.

Pages inside a section use `layout: page` (or a purpose-built layout, as
recipes do) and need `title` plus a one-line `description`. Add
`unlisted: true` to keep a page out of its section's list —
`recipes/websites.md` uses this because it is a reference page, not a recipe.

## Verifying a change

```bash
bundle install                 # first time only
bundle exec jekyll build       # must finish without errors
bundle exec jekyll serve       # http://127.0.0.1:4000/frantrucco-proton/
```

A Liquid or YAML error fails the build here and silently keeps the old site
live on GitHub Pages, so always build before committing. Check any layout or
CSS change at a 390px-wide viewport too.

`Gemfile` exists only for local previews; GitHub Pages ignores it and builds
with its own pinned Jekyll.
