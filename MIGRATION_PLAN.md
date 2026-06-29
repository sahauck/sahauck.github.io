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

8. ~~**[CC]** Draft `_pages/about.md`.~~ **DONE 2026-06-22.** WordPress text verbatim + contact info (office 222 A.W. Smith Bldg, phone, steven.hauck@case.edu). Selected improvements approved and applied.

9. ~~**[CC]** Draft `_pages/research.md` (new page, not in al-folio by default).~~ **DONE 2026-06-22.** WordPress text as baseline; 8 approved grammatical corrections applied. Facilities content dropped (decided). Dead `geology.case.edu` link was not present in the WordPress research page text.

10. ~~**[CC]** Draft `_pages/cv.md` as a hybrid (decided).~~ **DONE 2026-06-22.** `_pages/cv.md` links to `assets/pdf/hauck_cv.pdf`. `_data/cv.yml` has four sections: Current Position (map), Education (time_table), Academic Appointments (time_table), Selected Service and Leadership (time_table). Approved improvements applied.

11. ~~**[CC]** Generate `_bibliography/papers.bib` from the ~45 publications listed on the current Publications page:~~ **DONE 2026-06-16; extended 2026-06-19.** Final bib has 70+ entries: research articles, editorials (JGR:Planets reviewer appreciations, AGU Advances), and {other} category (Eos Editor's Vox pieces, whitepapers, Planetary Society blog, Astronomy Magazine Ask Astro, NRC appendix). All DOIs and html links verified active via CrossRef API and direct page fetches — 2026-06-19. Six corrections applied: DOIs added to hauck2017harassment and hauck2015pluto (Eos assigned DOIs retroactively), html added to hauck2013mercurycore, subtitle corrected on dombard2008despinning and weider2015evidence, Fe-FeS→Fe-S in chen2008non title. Four whitepaper PDFs live in assets/pdf/whitepapers/.

12. ~~**[CC]** Configure `_pages/publications.md` to render from the new bibliography, grouped by year (al-folio default behavior).~~ **DONE 2026-06-16.** Two sections: research articles (main) and "Editorials and Other Publications". Uses `category` field in bib entries (`{editorial}`, `{other}`) to split via jekyll-scholar `--query`. `category` added to `filtered_bibtex_keywords` so it's stripped from rendered output. The `{other}` category produces no output until entries are tagged with it.

---

## Phase 3 — CV PDF

13. ~~**[YOU]** Export current Word CV to PDF (your existing workflow).~~ **DONE 2026-06-22.**

14. ~~**[CC]** Place CV PDF in `assets/pdf/` and link from `cv.md`.~~ **DONE 2026-06-22.** File at `assets/pdf/hauck_cv.pdf`; path already wired into `_pages/cv.md` and `_data/socials.yml`.

---

## Phase 4 — Visual identity and small config items

15. **[YOU]** Photos (decided — yes). Pick a profile photo (current headshot or new) and select 1-3 of your own photographs for use as a banner/header image and/or section accents. Good candidates: a wide-format landscape or action shot that works well cropped to al-folio's banner aspect ratio. **[CC]** can help resize/crop/optimize selected images once chosen, and wire them into the layout via `_config.yml` and the about page front matter.

16. ~~**[CC]** Configure `_config.yml` and related settings.~~ **DONE 2026-06-22.** Set first_name/last_name (Steven A. / Hauck, II), description, keywords, footer (removed Unsplash credit), favicon emoji (🪐), `serve_og_meta: true`, `inspirehep` badge disabled (no ID). ORCID already wired in `_data/socials.yml`. Google Scholar ID deferred until confirmed.

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
