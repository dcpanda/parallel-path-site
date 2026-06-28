# Parallel Path Site

A Governed State Control Plane. Orchestrate complex async workflows across parallel layers with guarded autonomy. Every action becomes a versioned, auditable state transition.

This is the landing/marketing site for the Parallel Path project. The site should be generated based on the project in the https://github.com/dcpanda/parallel-path repository.

## Setup

Requires Ruby 3.3.6 (set by `.ruby-version`).

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

## Deployment

GitHub Pages deployment target: [dcpanda.github.io/parallel-path-site](https://dcpanda.github.io/parallel-path-site)

## Constraints

- This is a **landing/marketing site**, not the Parallel Path Java application.
- No tests, linters, or CI — verify by reviewing output HTML.
