# Project Documentation Hub

This repository hosts the GitHub Pages site for my project documentation.

## Structure

```
/index.html              # Portfolio landing page (custom design, no theme chrome)
/assets/css/portfolio.css# Landing-page styles
/_data/projects.yml      # Projects shown on the landing page (data-driven)
/_config.yml             # Jekyll + just-the-docs config (color_scheme: ember)
/_sass/color_schemes/ember.scss  # Docs dark palette (mirrors the portfolio)
/_sass/custom/custom.scss        # Docs restyle (fonts, code blocks, callouts)
/_includes/head_custom.html      # Web fonts for the docs
/MapVoter/               # MapVoter plugin documentation
	index.md              # MapVoter landing page
	installation.md  configuration.md  commands.md
	wipe-scheduling.md  discord.md  approval.md  changelog.md
	full-guide.md         # Exhaustive configuration reference
```

The site is two layers: a **custom portfolio landing page** (`index.html`) and
**just-the-docs** for every documentation page. They share one palette (ember on
dark) so they read as one brand.

## MapVoter
- Rust server plugin for automated map voting, wipe scheduling, and Discord integration.
- Purchase: https://codefling.com/plugins/map-voter-and-auto-wipe-script

## Adding a project
1. **Show it on the landing page** — add an entry to [`_data/projects.yml`](_data/projects.yml)
   (name, tagline, tags, icon, docs path, links). A template entry is commented at
   the bottom of that file.
2. **Add its docs** — create a top-level folder (e.g. `NewProject/`) with an
   `index.md`:
   ```
   ---
   layout: default
   title: NewProject
   has_children: true
   nav_order: 3
   permalink: /NewProject/
   ---
   ```
   Add more `.md` pages in the folder, each with `parent: NewProject` and a
   `nav_order` to control sidebar order.

To change your name/bio/links on the landing page, edit the `<!-- IDENTITY -->`
block in [`index.html`](index.html).

## Local Preview
```
bundle install
bundle exec jekyll serve   # http://localhost:4000
```
(The `csv`/`base64`/`bigdecimal`/`logger` gems in the Gemfile are only needed
for local builds on Ruby 3.4+/4.0; GitHub Pages ignores them.)

## License
Content license: as per each project. Plugin source/licensing stays in its respective repository or marketplace page.

---
Generated structure prepared on October 6, 2025.