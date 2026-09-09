# Monochrome Redesign With Navy Accents

The redesign is isolated on `codex/monochrome-redesign`. The original
`map-first-redesign` checkout has not been changed by this work.

## Exact Rollback Point

`codex/before-monochrome-redesign` (`477ed1e`) records the page exactly as it
was before this redesign, including the existing uncommitted interaction
work. It is a Git snapshot, not a reconstruction of the previous look.

To restore the five website pages from that snapshot on a branch where the
redesign has been applied:

```sh
git restore --source=codex/before-monochrome-redesign -- index.html devlog.html status.html legal.html 404.html
git diff --check
git diff
```

Inspect and commit the restoration. This restores the design while retaining
the journal history. If later work has changed these pages, preserve that work
before restoring the snapshot. The redesign commit can also be reverted as
a whole using its exact commit ID.

## Scope

Black and white page surfaces and neutral typography form the foundation.
Navy is the main accent, with orange limited to small markers. This follows
the user's final clarification after reviewing the preview. The existing scroll sequence and map animation are retained.
Supporting page palettes match. No dependencies, build tooling, signups,
analytics or public release promises were introduced.

## Review Status

The local preview is for review. The live GitHub Pages website is unchanged.
Source checks passed; visual browser testing has not been performed.
