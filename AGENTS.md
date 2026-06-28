# Parallel Path Site — Agent Instructions

## Setup

```bash
ruby -v  # requires 3.3.6
bundle install
bundle exec jekyll serve  # dev server on localhost:4000
```

## Structure

- `_data/features.yml` — feature cards on homepage (add/remove cards here)
- `_data/nav.yml` — top navigation links
- `features/*.md` — individual feature pages
- `releases.md` — changelog
- `assets/css/main.css` — all styles
- `assets/images/` — SVGs (architecture, guard-system, favicon)
- `_plugins/taint_patch.rb` — workaround plugin, do not remove

## Constraints

- Ruby 3.3.6 required (set by `.ruby-version`)
- This is a **landing/marketing site**, not the Parallel Path Java application
- No tests, linters, or CI — verify by reviewing output HTML
- GitHub Pages deployment target: `dcpanda.github.io/parallel-path-site`
