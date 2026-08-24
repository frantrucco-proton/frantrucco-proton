# CLAUDE.md

Conventions for this repo. Read this before making any change here.

## What this repo is

A small personal reference site published with **GitHub Pages**. Recipes are
the first section; more will follow (Dutch taxes, notes on cities, useful
websites for finding housing).

- Live site: <https://frantrucco-proton.github.io/>
- The repo is named `frantrucco-proton.github.io`, which makes this a GitHub
  Pages **user site**: it is served from the bare root, so `baseurl` is `""`.
  Renaming the repo to anything else would push the site back under a path and
  `baseurl` would have to be set to match.
- This is no longer the profile README repo. That role belongs to a repo named
  exactly `frantrucco-proton` — which this one used to be — so `README.md` here
  is now just the repo's own README and no longer appears on the profile.

**The live site is the only output.** Never build an offline bundle, a
single-file HTML export, a PDF, or a zip of the site. If something should be
readable, it goes on the site.

## How it is built — read this before touching the build

The site uses the **Chirpy** theme (`jekyll-theme-chirpy`, a gem), which needs
Jekyll 4 and the `jekyll-archives` and `jekyll-include-cache` plugins. None of
that is available in GitHub Pages' native build, so:

- **The site is built by GitHub Actions**, not by Pages' own Jekyll.
  `.github/workflows/pages-deploy.yml` builds and deploys it.
- Settings → Pages → Source must stay on **GitHub Actions**. Switching it back
  to "Deploy from a branch" would try the native build and fail.
- **A push is a deploy attempt, not a deploy.** If the workflow fails, nothing
  ships and the previous version stays live. Always build locally first.
- The workflow also runs `html-proofer` over the built site. A broken internal
  link fails the build and blocks the deploy.

## Repo layout

```
_config.yml              Chirpy configuration
_layouts/recipe.html     the only custom layout; wraps Chirpy's `post`
_posts/                  one file per recipe, YYYY-MM-DD-<slug>.md
_tabs/                   sidebar tabs (about, categories, tags, archives)
_plugins/                posts-lastmod-hook.rb, from the Chirpy starter
assets/css/jekyll-theme-chirpy.scss   theme entry point plus custom recipe styles
recipes/websites.md      sites to search for recipes (a page, not a post)
index.html               front page, rendered by Chirpy's `home` layout
```

Everything else — layouts, includes, JavaScript, icons — comes from the gem.
Do not vendor copies of theme files; override through `_layouts/` or the SCSS
entry point instead, so the theme can be upgraded by bumping the gem.

## Adding a recipe

1. Create `_posts/YYYY-MM-DD-<slug>.md`, dated the day it is added. The slug is
   lowercase ASCII with hyphens and no diacritics: "Leczo warzywne" becomes
   `_posts/2026-08-24-leczo-warzywne.md`.
2. Fill in the template below.
3. Build and check it (see *Verifying a change*).
4. Commit the recipe in the **same commit** as anything else needed to make it
   appear on the site.

Nothing needs registering anywhere. The post appears on the front page, under
its categories and tags, and in the archive, automatically.

### The recipe file template

Copy this exactly. The order of the sections is fixed.

```markdown
---
layout: recipe                                         # not Chirpy's default `post`
title: Leczo warzywne
date: 2026-08-24 12:00:00 +0200
categories: [Recipes, Polish]                          # always [Recipes, <cuisine>]
tags: [vegetarian, one-pot, peppers]                   # lowercase
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

`layout: recipe` matters: Chirpy's default for posts is `layout: post`, which
would drop the times, the diet tags and the source link. `_layouts/recipe.html`
renders those around the body, producing the required order — **title with
times, servings and diet tags → English ingredients → Dutch ingredients →
method in English → source URL**. Do not repeat the title or the source link in
the body.

Where a source splits its ingredients into groups, keep the groups, as a bold
lead-in above each bulleted list, and keep them identical in both languages
(see `risotto-z-kurkami`).

### Recipe rules

- **Every recipe on the site is vegetarian.** No meat, poultry, or fish, in a
  recipe I went looking for and in one handed to me alike. Watch the
  ingredients that hide in otherwise-vegetarian dishes: meat or chicken stock
  (Polish `rosół`, Dutch `runderbouillon`/`kippenbouillon`), lard (Polish
  `smalec`, Dutch `reuzel`), bacon or `boczek`, gelatine, anchovies, fish
  sauce, Worcestershire sauce, and animal rennet in hard cheeses like Parmesan.
- **A recipe handed to me that is not vegetarian gets adapted, not skipped.**
  Say so before saving it, substitute the offending ingredient, and name the
  swap in `Substitutions and notes`: what the original used, what replaced it,
  and anything it changes about the method. The page links to the original, so
  every deviation from that original is stated on the page — never silently.
  `diet:` describes the version on the page, not the source.
- Where a source offers a choice that straddles the rule — "chicken or
  vegetable stock" — take the vegetarian option and say on the page that this
  is what the page does.
- **No tables. Ever.** Ingredients are bulleted lists, method steps are a
  numbered list. This holds for both language versions.
- The two ingredient lists must be the same ingredients in the same order, so
  they can be read side by side.
- Method is English only.
- **Translate from the source language, not from a guess.** Check the original
  words rather than reasoning from a rough English translation, especially
  mushrooms, fish, and cuts of meat, where a wrong guess produces a plausible
  but wrong ingredient (Polish `kurki` are chanterelles, not anything to do with
  `kura`, a hen; `pieczarki` are ordinary button mushrooms; `podgrzybek` is a
  bay bolete, not "brown mushroom"; `karkówka` is pork collar/neck, not "pork
  chop"). Dutch names come from what Dutch supermarkets actually print on the
  packet — `gezeefde tomaten`, `gerookte paprikapoeder`, `sperziebonen` — not a
  literal rendering of the English.
- **Flag anything hard to find in the Netherlands** in `Substitutions and
  notes`, with a concrete substitute and where to look (Polish shop, toko,
  Turkish supermarket). Example: Polish `twaróg` — closest is Dutch
  `kwark`/`hüttenkäse`, or the real thing from a Polish shop.
- Keep quantities in metric, as the source has them.

## Finding a recipe

**Search for vegetarian recipes only** — see the first two Recipe rules above.
Where a site has its own vegetarian category (aniagotuje.pl files them under
`dania bez mięsa`), search inside it rather than sifting general results by
hand, and still read the ingredient list before proposing the recipe.

`recipes/websites.md` is the list of sites to search **first**, in order,
before looking anywhere else. Add new sites there as bullets.

If a site cannot be reached — the sandbox egress proxy blocks some domains with
a 403, aniagotuje.pl among them — say so plainly and ask for the recipe text.
**Never invent recipe content and attribute it to a source URL.** A recipe page
carries a link to the original, so its contents have to actually match that
original.

## Adding a new section

Sections are **categories**. A new one needs no configuration: give the posts
`categories: [Dutch taxes, ...]` and the category appears on the Categories tab
with its posts under it.

For a section that is a single reference page rather than a set of posts, add a
page with `layout: page` and an explicit `permalink:`, as `recipes/websites.md`
does. To put something in the sidebar, add a file to `_tabs/` with an `icon:`
(Font Awesome class) and an `order:`.

## Working in this repo

**Work on `main` and push to `main`.** No feature branches, no pull requests,
unless Francisco asks for one in so many words.

## Verifying a change

```bash
bundle install                 # first time only
bundle exec jekyll build       # must finish without errors
bundle exec jekyll serve       # http://127.0.0.1:4000/
```

Before pushing, run what CI runs, or CI will find it instead:

```bash
JEKYLL_ENV=production bundle exec jekyll b
LANG=C.UTF-8 bundle exec htmlproofer _site --disable-external \
  --ignore-urls "/^http:\/\/127.0.0.1/,/^http:\/\/0.0.0.0/,/^http:\/\/localhost/"
```

`LANG=C.UTF-8` is not optional in a bare container: without it Ruby defaults to
US-ASCII and html-proofer dies on the first accented character rather than
reporting a real problem.

Check any layout or CSS change at a 390px-wide viewport, in both light and dark
mode. Chirpy pulls fonts and icons from a CDN, so in a sandbox that blocks
outbound requests the icons will be missing locally while being fine in a real
browser — do not "fix" that.
