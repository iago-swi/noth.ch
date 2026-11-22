# Versioning

noth.ch is a single product. Versions follow MAJOR.MINOR.PATCH.

## What counts as a version

- MAJOR: a redesign or a change of structure visitors notice
  (new layout, pages or languages removed, URLs changed).
- MINOR: something new a visitor can see or use
  (a new page, section, language or feature).
- PATCH: a visible fix or small adjustment
  (typos, wording, a broken link, an image swap, a style fix).
- Documentation only (README, CLAUDE.md, AGENTS.md, design-system.md,
  FAQ.md, VERSIONING.md, docs/): no version, no tag, no release.

Each version has a section in CHANGELOG.md (newest first), a git tag
vX.Y.Z on the merge commit in `main`, and a GitHub release whose notes
are that changelog section, written for visitors in plain language.

## How a version is published

Every change goes through a branch and a pull request.
Nobody commits or pushes directly on `main`.

"Publish" means:
1. Bring `main` up to date locally.
2. Create a branch from it: `release/noth-ch-vX.Y.Z` for a version,
   `docs/<short-name>` for a documentation-only change.
3. Commit on that branch under the owner's own Git identity, with a
   plain message and no attribution lines.
4. Push the branch and open a pull request with `gh pr create`.
   Title: the version number and a few words. Body: the changelog
   section for this version, in plain language.
5. Merge it with a merge commit:
   `gh pr merge --merge --delete-branch` (no squash, no rebase).
6. Tag the merge commit on `main` (vX.Y.Z), push the tag, and create
   the GitHub release with the changelog section as its notes.
   A documentation-only change stops after step 5.

"Open a pull request" means: stop after step 4.

Never force-push `main`. Never move or delete a pushed tag.
Never rewrite existing history.
