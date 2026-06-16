# Migration Plan: casfaculty.case.edu/steven-hauck → sahauck.github.io (al-folio)

This plan is organized into phases. Each task is tagged:

- **[YOU]** — requires your action (browser/account access, content judgment, file gathering)
- **[CC]** — well-suited for a Claude Code session with repo access
- **[YOU + guidance]** — you execute, Claude (chat or Code) explains/troubleshoots

Recommended al-folio version to pin to: **v0.16.3** (latest stable as of this writing). Pinning avoids landing the merge on a moving target, since al-folio has had a recent major architectural rewrite.

---

## Phase 0 — Pre-flight checks (do this before any file changes)

1. **[YOU]** Check current GitHub Pages deployment source for `sahauck.github.io`:
   - Repo → Settings → Pages → "Build and deployment" → Source
   - If it says **"Deploy from a branch"**, this must change to **"GitHub Actions"** before al-folio (which uses `jekyll-scholar` and other plugins outside GitHub's default Pages whitelist) will build correctly.
   - You don't need to change it yet — just note which setting is currently active, since it determines whether Phase 1's workflow file will actually take effect or needs a settings change alongside it.

2. **[YOU + guidance]** Set up local Jekyll (decided). On Windows, the two viable routes are RubyInstaller with DevKit (native, works alongside your existing Git Bash setup) or WSL2 (Jekyll on Linux, fewer native-gem build headaches but a separate filesystem/toolchain from your Windows Git Bash workflow). Given you're already running Claude Code via Git Bash on Windows for other projects, RubyInstaller+DevKit is the more consistent choice unless you'd rather isolate this project in WSL. This is a prerequisite for testing Phase 1, so it's worth doing first.

3. ~~Gather publication PDFs~~ — **dropped (decided)**. Publications will link to DOI/journal pages instead of self-hosted PDFs; most are freely accessible at their DOI given their age, and this removes the slowest, most tedious task in the project along with any copyright ambiguity around redistributing publisher-formatted PDFs.

---

## Phase 1 — Theme integration into existing repo

4. ~~**[CC]** Fetch al-folio `v0.16.3` and merge its file structure into `sahauck.github.io`, preserving existing git history. This includes `_layouts/`, `_includes/`, `_sass/`, `_plugins/`, `_data/`, `assets/`, `Gemfile`, `Gemfile.lock`, `_config.yml` (template), and `.github/workflows/`.~~ **DONE 2026-06-16.** Branch: `al-folio-migration`. All demo content directories also brought in (to be stripped in step 6). Three `_config.yml` tweaks needed for local build on Windows: `imagemagick.enabled: false` (not installed), `assets/jupyter/` added to `exclude:` (nbconvert not installed), `index.md` added to `exclude:` (conflicts with `_pages/about.md` permalink `/`). MSYS2 DevKit not pre-installed — required `ridk install 1 2 3` before `bundle install`. Local build passes (`bundle exec jekyll build`, exit 0) in ~50s. Sass deprecation warnings and Terser stderr are non-fatal.

5. **[YOU]** If Phase 0 revealed Pages is set to "Deploy from a branch," change it to **"GitHub Actions"** in repo Settings now that the workflow files exist from step 4.

6. ~~**[CC]** Strip out al-folio demo content you won't use: sample blog posts, sample projects, sample books/teaching collection entries, demo images. Disable unused collections (`books`, `teaching`, `news` if you don't want a homepage feed) in `_config.yml`.~~ **DONE 2026-06-16.** Removed all demo posts/projects/books/teachings/news, demo pages, demo images and media. Cleared papers.bib. Disabled books/projects/teachings collections; news kept with output:false. External RSS sources cleared.

7. ~~**[YOU + guidance]** Run a local build (`bundle exec jekyll serve`) or push and check the Actions run, to confirm the bare theme deploys successfully *before* adding your content. This establishes a known-good baseline.~~ **DONE 2026-06-16.** Actions build passes; sahauck.github.io deploys al-folio with correct url/baseurl and working navigation.

---

## Phase 2 — Core content migration

8. **[CC]** Draft `_pages/about.md`:
   - Photo, name, title (Professor and Chair, Planetary Geodynamics), contact info
   - Short bio drawn from the current "Research Interests" paragraphs (condensed)
   - Prominent, standalone callout for the postdoc/grad/undergrad research opportunities paragraph — not buried
   - Decide placement of education/appointments: short version here, full version on CV page (see step 10)

9. **[CC]** Draft `_pages/research.md` (new page, not in al-folio by default):
   - Two subsections: planetary heat transfer, planetary tectonics — adapted from current Research page text
   - **[YOU]** Flag: fix or remove the dead `geology.case.edu/~hauck` link to the old Mars page before this is finalized
   - Facilities content is dropped entirely (decided) — appropriate given the site is no longer framed within a department structure

10. **[CC]** Draft `_pages/cv.md` as a hybrid (decided):
    - Simplified HTML CV via `_data/cv.yml` — condensed sections (current position, education, appointments, key roles), not a full line-by-line replica of the Word/PDF version
    - Prominent download link/button to the full CV PDF (you'll continue exporting this from Word as the source of truth, since publications already live on their own page and don't need duplicating in the CV)
    - **[YOU]** Before finalizing, confirm the appointments dates are still accurate (e.g., "Department Chair, 2019–Present")
    - Workflow note: each time you update the Word CV, re-export to PDF and replace the file in `assets/pdf/` — the HTML summary on the page won't need to change as often as the full PDF

11. **[CC]** Generate `_bibliography/papers.bib` from the ~45 publications listed on the current Publications page:
    - Correct BibTeX types (`@article` for journal papers, `@incollection` for book chapters like the two "Mercury after the MESSENGER Mission" entries)
    - `year`, `journal`/`booktitle`, `volume`, `pages`, `doi` fields populated
    - `html` field pointing to the DOI/journal landing page for each entry (no `pdf` field — links go to DOI, not self-hosted files)
    - Your name marked for al-folio's author-highlighting (via `_data/coauthors.yml` or theme convention)
    - **[YOU]** Spot-check a handful of older entries to confirm the DOI links actually resolve to free/open access versions as expected, particularly any pre-DOI-era papers that might only have a journal table-of-contents link rather than a working DOI

12. **[CC]** Configure `_pages/publications.md` to render from the new bibliography, grouped by year (al-folio default behavior).

---

## Phase 3 — CV PDF

13. **[YOU]** Export current Word CV to PDF (your existing workflow).

14. **[CC]** Place CV PDF in `assets/pdf/` and link from `cv.md`.

---

## Phase 4 — Visual identity and small config items

15. **[YOU]** Photos (decided — yes). Pick a profile photo (current headshot or new) and select 1-3 of your own photographs for use as a banner/header image and/or section accents. Good candidates: a wide-format landscape or action shot that works well cropped to al-folio's banner aspect ratio. **[CC]** can help resize/crop/optimize selected images once chosen, and wire them into the layout via `_config.yml` and the about page front matter.

16. **[CC]** Configure:
    - `_config.yml`: site title, description, author name, social links (ORCID, Google Scholar — both already known from current Publications page), theme color variable in `_sass/_themes.scss`
    - Favicon (`assets/img/`)
    - `og:image` / social preview image, with `serve_og_meta: true`

---

## Phase 5 — Testing and deployment

17. **[YOU + guidance]** Full local build test (or Actions build) of the complete site — check all four pages, publications rendering, DOI links, nav menu order.

18. **[CC]** Write a custom `404.md` page that notes the site has moved from `casfaculty.case.edu`, in case old bookmarks/links resolve to a 404 on the new domain (they won't resolve to your new domain automatically, but this helps if you ever do get a redirect set up — see step 20).

19. **[YOU]** Final review pass on all page content for accuracy and tone — this is the proofreading task only you can do.

---

## Phase 6 — Cutover and cleanup

20. **[YOU]** Once satisfied, contact CWRU (per the new policy) about:
    - Whether a redirect from `casfaculty.case.edu/steven-hauck/*` to `sahauck.github.io` is possible (even a single root-level redirect helps)
    - Updating the EEPS department directory/listing to point to the new URL
    - Updating your CV's "personal website" field and ORCID/Google Scholar profile links to the new domain

21. **[Future enhancement, optional]** Populate `_data/cv.yml` for the full rendered HTML CV (deferred from step 10), and consider whether the `news` collection is worth using for occasional updates (new papers, talks, etc.) given it displays on the homepage.

---

## Decisions (resolved)

- Local Jekyll dev environment: **yes**, via RubyInstaller+DevKit on Windows (Phase 0, step 2)
- Facilities content: **dropped entirely** (Phase 2, step 9)
- CV page: **hybrid** — simplified `_data/cv.yml` HTML summary + link to full PDF kept up to date from Word (Phase 2, step 10)
- Banner/header photo: **yes**, using your own photography (Phase 4, step 15)

## Suggested starting point

Phases 0-1 (local Jekyll setup + theme merge) are the natural first Claude Code session, since everything else depends on having a working local build to iterate against. Photo selection (Phase 4, step 15) can proceed in parallel on your own schedule — it doesn't block the technical work.
