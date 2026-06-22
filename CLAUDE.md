# Personal Website — Claude Guide

Built on [al-folio](https://github.com/alshedivat/al-folio) (Jekyll). Hosted locally with:

```bash
export PATH="/opt/homebrew/bin:$HOME/.rbenv/bin:$HOME/.rbenv/shims:$PATH" && eval "$(rbenv init -)"
cd "/Users/yc3965/Dropbox/Personal Website"
bundle exec jekyll serve --port 4001
# → http://localhost:4001
```

---

## Site Structure

| Page         | File                     | Layout                  |
| ------------ | ------------------------ | ----------------------- |
| About (home) | `_pages/about.md`        | `_layouts/about.liquid` |
| Research     | `_pages/publications.md` | `_layouts/page.liquid`  |
| Teaching     | `_pages/teaching.md`     | `_layouts/page.liquid`  |
| CV           | `_pages/cv.md`           | `_layouts/page.liquid`  |

Nav order: About (home) → Research (2) → Teaching (3) → CV (4).  
Page title headings are hidden — removed from `_layouts/page.liquid`.

---

## Common Updates

### Add a new paper

Edit `_pages/publications.md` directly — the al-folio bibliography system is bypassed in favour of plain HTML for full layout control.

Each paper entry follows this pattern:

```html
<li>
  <div class="paper-title">"Paper Title"</div>
  <div class="paper-authors">
    <strong>Yi-Wen Chen</strong>, Co-author One, and Co-author Two &nbsp;<a href="https://SSRN_LINK" target="_blank" rel="noopener">[Paper]</a>
  </div>
  <div class="paper-status">Status, e.g. Working Paper / Invited Revision at <em>Journal Name</em></div>
  <details>
    <summary>Abstract</summary>
    <p>Abstract text here.</p>
  </details>
</li>
```

- **Job Market Paper** section uses `<ul class="paper-list">`.
- **Working Papers** section uses `<ol class="paper-list">` (numbered). Revision paper goes first, then newer papers first.
- If no paper link exists, omit the `<a>` entirely.

### Update photo

Replace `assets/img/prof_pic.jpg` with the new photo (keep the filename), or place a new file and update `_pages/about.md`:

```yaml
profile:
  image: your_new_photo.jpg
```

### Update bio

Edit the markdown body of `_pages/about.md` (below the front matter `---`).

### Update subtitle / affiliation

Edit `subtitle:` in `_pages/about.md`.

### Add a teaching entry

Edit `_pages/teaching.md`. Each institution is an `<h3>`, courses go in a `<table class="teaching-table">`:

```html
<tr>
  <td>Course Name</td>
  <td>Role</td>
  <td>Term</td>
</tr>
```

### Update CV PDF

1. Recompile: `/Library/TeX/texbin/pdflatex -output-directory "CV_PhD" CV_PhD/main.tex`
2. Copy: `cp CV_PhD/main.pdf assets/pdf/CV_Penny_Chen.pdf`

Or drop a new PDF directly at `assets/pdf/CV_Penny_Chen.pdf`.

### Update social links

Edit `_data/socials.yml`. Current fields:

```yaml
email: YChen26@gsb.columbia.edu
scholar_userid: a2RLPYUAAAAJ
linkedin_username: chen-yi-wen
```

To add icons, add fields like `github_username`, `twitter_username`, etc.

---

## Design System

All research and teaching styles live in `_sass/_publications.scss` under `.research-page`.

### Typography hierarchy

| Element           | Class / Tag                | Size    | Weight                   |
| ----------------- | -------------------------- | ------- | ------------------------ |
| Section heading   | `h3` in `.research-page`   | 1.35rem | 700                      |
| Paper title       | `.paper-title`             | 1.05rem | 600                      |
| Authors           | `.paper-authors`           | 0.9rem  | 400 (self in `<strong>`) |
| Status / abstract | `.paper-status`, `details` | 0.88rem | 400                      |

### Abstract toggle

Native HTML `<details>/<summary>`. The `▶` rotates to `▼` on open. No JS needed.

### Social icons

Left-aligned, appear below bio text on the about page. Size controlled by `.contact-icons` in `_sass/_components.scss`.

---

## Key Design Decisions

- **No page title headings** — `_layouts/page.liquid` has the `<header>` block removed entirely.
- **Bibliography bypassed** — `publications.md` uses raw HTML instead of `{% bibliography %}`.
- **CV page** embeds `assets/pdf/CV_Penny_Chen.pdf` via `<iframe>` at 85vh.
- **No search bar** on research page.
- **Teaching page** shares `.research-page` CSS with the research page (`<h3>` headings + `.teaching-table`).
- **Navbar name**: first name bold, last name bold, middle name normal weight — set in `_includes/header.liquid`.
