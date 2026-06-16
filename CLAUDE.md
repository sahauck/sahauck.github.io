# Project: sahauck.github.io migration to al-folio

## What this is
Personal academic website for Steven A. Hauck, II (planetary scientist,
CWRU). Migrating from a minimal placeholder GitHub Pages site to the
al-folio Jekyll theme, replacing a WordPress site previously hosted at
casfaculty.case.edu/steven-hauck (that domain is being retired by the
university and is not reachable from this repo's tooling).

Full phased task list, rationale, and all content-decision history is in
MIGRATION_PLAN.md at the repo root. Read it at the start of every session
and check off / annotate completed steps as you go so progress survives
context compaction.

## Key resolved decisions (do not re-litigate these)
- Theme: al-folio, pinned to **v0.16.3**. Do not track `main`/upstream HEAD.
- Facilities page/content: dropped entirely, not migrated.
- CV: hybrid approach. Simplified HTML summary via `_data/cv.yml`
  (condensed: current position, education, appointments, key roles) PLUS
  a prominent link to the full CV PDF. The full CV is authored in Word and
  exported to PDF outside this repo — do not attempt to generate or edit
  the full CV content yourself, only place the PDF file and link to it.
- Publications: link to DOI/journal landing pages only. No self-hosted
  PDFs of papers. Do not add a `pdf` field to bibliography entries.
- Banner/profile images: will use the user's own photography. Do not
  source or generate stock/placeholder images for this purpose — wait for
  the user to provide files.

## Environment
- Windows, Ruby installed via Microsoft Store with DevKit/MSYS2 confirmed
  working (`ridk version` and `gem install nokogiri` both succeed).
- Local preview: `bundle exec jekyll serve`.
- Confirm `ruby -v` / `bundle -v` are visible in whatever shell you're
  running in before doing gem/bundle work — PATH can differ between
  terminal types.

## Workflow rules
- **Always work on a feature branch, never commit directly to main.**
  Current branch for the initial theme merge: `al-folio-migration`.
- Never run `git push`, never change GitHub repo settings (Pages source,
  Actions permissions, etc.), and never contact any external service on
  the user's behalf. These are explicitly reserved for the user to do by
  hand — flag them when relevant and stop.
- Phase 6 of MIGRATION_PLAN.md (CWRU contact, redirects, updating external
  profile links) is entirely out of scope for this repo's Claude Code
  sessions. Do not suggest taking action on it beyond noting it's the next
  step once the site is ready.
- When a phase/step is completed, note it in MIGRATION_PLAN.md (e.g. strike
  through or annotate with a date) so future sessions don't redo work or
  lose track of state.
- If asked to do something not covered by MIGRATION_PLAN.md, check whether
  it fits an existing phase before improvising new structure.

## Build/test
- `bundle install` to install gems (DevKit required for native extensions
  like nokogiri — already confirmed working in this environment).
- `bundle exec jekyll serve` for local preview at localhost:4000.
- Check the `.github/workflows/` deploy action exists and matches al-folio
  v0.16.3's expected GitHub Actions-based Pages deployment — the default
  GitHub Pages "deploy from branch" build will NOT work with this theme's
  plugin set (jekyll-scholar, etc.).
