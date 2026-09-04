# Laneway site working context

## What this is

The public face of Laneway: a landing page and an engineering dev log, served by
GitHub Pages at <https://lanewayapp.github.io/>.

Three pages, all self-contained:

- `index.html` - what Laneway is, the anchor case, how verification works, honest status
- `status.html` - the shape of the work, drawn from the engine repo's history
- `devlog.html` - what got built, what broke, what was decided. Newest entry first.
- `legal.html` - terms of service, privacy policy, and EULA, all three on one page
  with `#terms`, `#privacy`, `#eula` anchors. Canonical copies live in the product
  repo under `docs/legal/`; a change there gets mirrored here.

`devlog.md` is the source of truth for the dev log's entries and its metrics
band. `devlog.html` reads that file at load time and builds the page from it,
so an entry is written once, in markdown, and committing the markdown is the
whole publishing step. Never write entries into the HTML.

The product itself lives in `barongartner/LANEWAY`, which is **private and stays
private**. This repo is public. Treat the boundary between them as the most
important rule in the file.

## status.js, and the one place a number comes from

`status.js` is the only generated file here. It is written by
`ops/export_status.py` **in the private engine repo**, and both `index.html` and
`status.html` load it with a plain `<script src>`, not a fetch, so both pages
still open straight off disk.

To refresh it, from the engine repo:

```bash
python3 ops/export_status.py ../laneway-site/status.js
```

Then commit `status.js` here. It is safe to run any time; it reads the engine
repo and writes nothing to it.

Three rules about it:

- **Never edit `status.js` by hand.** The next export overwrites it, and a
  hand-typed figure in it is exactly the unsourced claim these pages exist to
  avoid. If a number looks wrong, fix the exporter.
- **The exporter lives in the private repo on purpose.** It maps real source
  paths to the five public area names, and that mapping is repo structure. Only
  the names cross over. Keeping the script here would put the paths here.
- **Prose is not counted, and is not named.** Owner call, Thursday Sep 3: the
  engine repo's session journal is its largest file by a wide margin, and
  counting it as source made documentation the biggest bucket on this page and
  put twenty thousand lines into the totals, describing a writing habit rather
  than a product. The exporter now skips every `.md` file and everything under
  `docs/` when counting lines and areas. A commit that only touches prose is
  still a commit and still lands on the per-day chart; it just contributes no
  lines and no area. The page says "prose is not counted anywhere here" and
  names no file, and it should stay that way.
- **It carries nothing per commit.** Only per-day counts, per-area totals, the
  release ladder and the headline numbers. No subject, no sha, no path, no
  author, no time of day. An earlier version published a row per commit to feed
  a redacted log on the page; that section was cut on Thursday Sep 3 for being
  unreadable, and the rows went with it, because publishing data nothing renders
  is the habit this file exists to prevent. If a section is ever cut again,
  check whether it was the only consumer of a field.

The `# Metrics` band in `devlog.md` is still typed by hand and is the one place
a stale number can still appear. Check it against `status.js` when writing an
entry.

## The one rule, inherited

**Never present a fact the page cannot back up.** It is the product's rule and it
applies to the marketing before it applies to anything else. A landing page that
overclaims while the app underneath refuses to state an unsourced fact is a
contradiction a reader will find.

Concretely, on these pages:

- Every capability claimed must exist in the engine or the app today. If it is
  planned, it belongs under "Not there yet" or in the dev log, phrased as planned.
- No download button, no App Store badge, and no waitlist promise until there is
  something real behind it. The status pill says "in development" because it is.
- Numbers (test counts, feed counts, version) come from the engine repo at the
  time of writing, and get updated when they change or removed when they go stale.
  A stale number is a false claim with extra steps.
- No invented testimonials, no invented user counts, no invented benchmarks.

## What must never cross over from the private repo

- API keys, tokens, `.env` contents, or anything that looks like a credential
- Server addresses, local network details, hostnames, ports, deploy targets
- Apple team identifiers, signing identities, provisioning details
- Personal information of any kind: home area, routes actually travelled,
  named test destinations near where the owner lives, device serials
- Spend figures, unit costs, and anything else commercially sensitive
- Source code from the engine. Describe the architecture, never paste it.

City-level transit coverage is fine and is product information: the supported
feeds are published open data. A real errand route is not.

## House style

**No em dashes or en dashes anywhere in this repo.** ASCII hyphen `-` only. This
applies to HTML content, comments, and commit messages. The same substituted
typography gets flagged too: smart quotes, ellipsis characters, non-breaking
spaces. Watch for these especially in HTML, where a curly quote is invisible in
the rendered page and still fails the check.

Inside a sentence, replace the dash with whatever the sentence is doing: a colon
before an explanation, a semicolon between independent clauses, a comma for an
aside, a full stop where it was really two sentences.

Arrow characters count as non-ASCII. Use `->` in text, or draw the arrow in SVG.

Run before every commit:

```bash
python3 -c "
import io, sys, glob
sys.stdout.reconfigure(encoding='utf-8', errors='replace')
ALLOW = {chr(0xa7)}
bad = False
for path in glob.glob('**/*.*', recursive=True):
    if '/.git/' in path: continue
    for i, line in enumerate(io.open(path, encoding='utf-8'), 1):
        if any(ord(c) > 127 and c not in ALLOW for c in line):
            print(path, i, line.rstrip()); bad = True
print('FOUND NON-ASCII' if bad else 'clean')
sys.exit(1 if bad else 0)
"
```

## Technical rules

- **No build step, ever.** No bundler, no framework, no npm, nothing to run
  before a commit. Owner decision, Wednesday Aug 19: writing the dev log means
  editing `devlog.md` and committing it, with no command in between.
- **`index.html` still opens straight off disk** and shows the finished page.
  `devlog.html` no longer does: it fetches `devlog.md`, and a `fetch` from a
  `file://` origin is blocked by the browser, so opening it from Finder shows
  the chrome and a line pointing at the markdown. Served over http, from Pages
  or from `python3 -m http.server`, it renders in full. This is the accepted
  cost of having no build step; the same choice means the entries are not in
  the HTML source, so they are invisible to anything that does not run
  JavaScript.
- **The renderer is ours.** `devlog.html` carries a small parser at the bottom
  of the file for the markdown subset the log uses. It is not a markdown
  library and must never become a fetched one: no CDN, no npm, no third-party
  request. `devlog.md` is a sibling file on the same origin, which is why
  fetching it does not break the no-third-party rule.
- **No third-party requests at runtime.** No CDN, no web fonts, no analytics, no
  embedded video, no form services. The page must render fully offline. This is
  not only privacy hygiene; it is why the page cannot break at load time.
- **Each page is one file.** Styles are inline in a `<style>` block and duplicated
  across the two pages on purpose. That duplication is the price of having no
  build step, and it is worth paying. When a shared token changes, change it in
  both files.
- **The palette is lifted from `ios/Laneway/Theme.swift`** in the product repo:
  ink navy, warm amber, paper light and night dark, plus the verifier's own green
  and red. Those two colours mean "verified" and "contradicted" and are never used
  decoratively. If the app's theme changes, update these pages to match.
- **Dark mode via `prefers-color-scheme`**, both pages, always tested in both.
- Images, if any are ever added, must be committed to the repo, sized for the web,
  and given real `alt` text. Screenshots must be of the app as it actually is.

## Dev log entries

The dev log is the public, curated version of `JOURNAL.md` in the product repo. It
is not a copy. The private journal records everything including the parts that
cannot be published; the dev log picks the entries that are interesting to a
reader who does not work here, and rewrites them so they stand alone.

Keep the private journal's honesty: the bugs, the dead ends, and the corrections
are the most worthwhile entries on the page. A log of nothing but wins reads as
marketing and gets skimmed.

Format per entry: a version or a short label, a spelled-out date with the weekday,
a headline, and the notes. Newest first, at the top of the list.

Entries are written in `devlog.md`, in the small markdown subset the generator
accepts. Nothing else is supported, on purpose:

```markdown
## 0.19.0 | Monday Aug 17          version or phase name, then the date
### The headline for the entry
> A pull quote, optional
> -- who said it, optional
A paragraph, optional.
- **A bold lead in.** Then the note itself.
- [ok] **A verdict tag** instead of a bold lead. [bad] works the same way.
```

Inline marks are `**bold**` and `*italic*`, and that is the whole list. A
version beginning with a digit is set as a display figure; anything else, like
`Direction` or `Day zero`, is set as a mono label, because it is a label and
not a release. The metrics band at the top of the page comes from the
`# Metrics` section of the same file, so the numbers `CLAUDE.md` warns about
going stale live beside the entries that would make them stale.

There is nothing to run. Commit `devlog.md` and the published page shows the
new entry as soon as Pages has served the file.

To see a change locally, serve the folder rather than opening the file:

```bash
python3 -m http.server 8076
```

Anything the parser does not understand is escaped and shown as literal text
rather than emitted as markup, so a mistake in the markdown looks wrong on the
page instead of breaking it.

## JOURNAL.md

Read `JOURNAL.md` before starting, and update it as the session runs. Same rules
as the product repo: spelled-out dates with the weekday, 12-hour times with AM/PM,
and **run `date` before writing any timestamp**. Never estimate one, and never
carry a stale time forward from earlier in the session.

```bash
date '+%A %b %-d'      # Wednesday Aug 12
date '+%-I:%M %p'      # 11:41 AM
```

## Git identity

Baron is the author on every commit (author email barongartner@gmail.com, so the
contribution graph counts it), and every commit Claude makes ends with a
`Co-Authored-By` trailer naming the model that did the work, for example
`Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>`. Never author commits as
Claude directly, and never omit the trailer.

## Why this is a separate repo

This repo lives in the `lanewayapp` organisation, not under `barongartner`. That
is only so the site gets the root of its own subdomain: a user or org site is
served at `<name>.github.io/`, while any other repo is served in a subfolder. The
plain `laneway` org name was already taken by a dormant account registered in
2021, so `lanewayapp` is the closest free variant.

The engine repo is private, and GitHub Pages will not serve a site from a private
repo without a paid plan. Making the engine public to publish a landing page would
trade the moat for a web page.

This split is temporary by design. When there is something to launch, the site
folds back in alongside the product, or moves to a real domain. Until then, keep
the pages free of anything that would make them painful to move: relative links
between the two pages, no absolute paths, no hard-coded host beyond the canonical
URL in the `og:url` tags.
