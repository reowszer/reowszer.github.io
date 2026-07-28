# Publishing Obsidian notes to the site

`bin/obsidian-to-note.js` converts an Obsidian markdown note into an al-folio
**Notes** post (a file in `_posts/` that shows up on `/notes/`). `bin/` is
excluded from the Jekyll build, so this script and this file never get published.

## Quick start

```bash
# 1. Convert (writes to _posts/YYYY-MM-DD-<slug>.md)
node bin/obsidian-to-note.js "~/Obsidian/QM/oscillator.md" \
    --tags "lecture-notes quantum-mechanics" --toc

# 2. If the note had image embeds, copy those files into assets/img/
#    (the script prints exactly which filenames it expects)

# 3. Preview locally if you have the ruby toolchain, or just push:
git add _posts assets/img && git commit -m "Add note: ..." && git push
```

Use `--dry-run` first to see the result without writing anything.

## What it fixes (Obsidian -> al-folio)

| Obsidian syntax | Becomes | Why |
| --- | --- | --- |
| `$x$` inline math | `$$x$$` | al-folio's MathJax needs `$$` even inline |
| `$$...$$` display math | unchanged | already correct |
| `==highlight==` | `<mark>highlight</mark>` | kramdown has no `==` |
| `%% comment %%` | removed | Obsidian-only |
| `> [!note] Title` callout | `> **Note: Title**` blockquote | no callout support on the site |
| `[[Note\|alias]]`, `[[Note#h]]` | the display text | internal vault links do not exist here |
| `![[pic.png\|300]]` | `![](/assets/img/pic.png)` | you must copy the image into `assets/img/` |
| `![[Other Note]]` | a placeholder blockquote | notes cannot be inlined on a static site |
| trailing `^blockid` | removed | Obsidian block anchors |

Fenced code blocks and inline `` `code` `` are left completely untouched.

## Front matter

Resolved automatically (override any with a flag):

- **title**: `--title` > front-matter `title:` > first `# H1` > filename
- **date**: `--date YYYY-MM-DD` > front-matter `date:`/`created:` > `YYYY-MM-DD`
  prefix in the filename > file mtime
- **tags**: `--tags "a b c"` > front-matter `tags:` (list or inline array; a
  leading `#` is stripped)
- **description**: front-matter `description:`/`summary:` > first clean paragraph
- **categories**: `--category` (default `notes`)
- always adds `related_posts: false`; `--toc` adds a left TOC sidebar;
  `--draft` adds `published: false` (kept in the repo, hidden on the site)

## Options

`--outdir DIR` (default `_posts`), `--assets PATH` (default `/assets/img`),
`--dry-run`, `--force` (overwrite existing output), `--help`.

## Things to check after converting

- The script prints a summary (`inlineMath=…, callouts=…`) and warnings for any
  image/note embeds. Copy referenced images into `assets/img/`.
- Note embeds (`![[Other Note]]`) leave a placeholder; paste the content or link
  it manually.
- If the note cites papers, al-folio uses `{% cite key %}` with
  `_bibliography/papers.bib` and `related_publications: true` in the front
  matter (the script does not do citations automatically).
