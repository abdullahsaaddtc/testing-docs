# Documentation Maintainer Guide

This directory explains how the Scopien documentation repository is consumed by the admin panel. It is for maintainers and QA; files here are not rendered in the documentation sidebar.

## Repository contract

The admin panel reads:

- nav.yml from the repository root.
- Markdown pages from the configured docs directory.
- Page order, labels, icons, and folder defaults from nav.yml.

The canonical production repository is ScopienInc/Scopien-Docs on main. The abdullahsaaddtc/testing-docs repository mirrors the same content while the migration is completed.

Runtime access is configured in ScopienOS:

| Environment key         | Purpose                                         | Production value         |
| ----------------------- | ----------------------------------------------- | ------------------------ |
| REPOSITORY_TOKEN        | Server-side read token for a private repository | Repository-scoped secret |
| NEXT_PUBLIC_DOCS_REPO   | GitHub owner and repository                     | ScopienInc/Scopien-Docs  |
| NEXT_PUBLIC_DOCS_BRANCH | Branch to read                                  | main                     |
| NEXT_PUBLIC_DOCS_PATH   | Directory containing Markdown pages             | docs                     |

REPOSITORY_TOKEN is the only credential used for documentation reads. Do not put a token in nav.yml, Markdown, a client-side variable, or a committed environment file.

## Maintainer references

- [Documentation Structure Guide](docs-structure-guide.md): file layout, page anatomy, links, and naming.
- [Navigation Variations Guide](nav-variations-guide.md): supported nav.yml shapes and their routes.
- [Markdown Testing Guide](MARKDOWN-TESTING-GUIDE.md): renderer behavior and manual QA.
- [Complete Markdown Test Suite](COMPLETE-MARKDOWN-TEST-SUITE.md): fixture ownership and release checks.

## Standard change workflow

1. Create or edit a Markdown page under docs/.
2. Add, remove, or move its nav.yml entry in the same change.
3. Validate nav.yml and every referenced path.
4. Preview desktop and mobile documentation in the admin panel.
5. Test links, quick navigation, search, previous/next navigation, code copy, and any rich content used.
6. Merge to main and allow the documentation cache to refresh.

The application caches GitHub navigation and page content for up to one hour. A correct merge may not appear immediately in an already running environment.

## Source-of-truth rules

- nav.yml controls visible navigation order. Directory order does not.
- Every published page should have exactly one nav.yml entry.
- Use root-relative repository paths such as docs/getting-started/introduction.md.
- A top-level Home link should remain first so /admin/documentation has a stable landing page.
- Keep maintainer-only material in dev docs/, outside NEXT_PUBLIC_DOCS_PATH.
- Do not document files that do not exist or propose configuration the application does not read.

If nav.yml is missing, the application falls back to an alphabetical directory tree. If nav.yml exists but is invalid or empty, documentation loading fails instead of silently using a different order.

## Pull request checklist

- [ ] All new or renamed pages are reflected in nav.yml.
- [ ] All nav.yml paths exist and remain inside docs/.
- [ ] Each page has one H1 and useful H2/H3 headings.
- [ ] Relative Markdown page links resolve to published navigation entries.
- [ ] Images use reachable HTTPS URLs.
- [ ] Desktop and mobile quick navigation select the correct heading.
- [ ] Previous and next links follow nav.yml order.
- [ ] No secrets, internal tokens, or customer data are present.
- [ ] Both repository copies are synchronized when a mirrored update is required.

## Repository synchronization

During the migration period, make the same documentation commit in both repositories. Compare the trees before declaring the sync complete:

```sh
git -C testing-docs ls-tree -r --name-only main
git -C Scopien-Docs ls-tree -r --name-only main
diff -qr --exclude=.git testing-docs Scopien-Docs
```

The commit hashes do not need to match because each repository has its own history. The tracked content should match byte-for-byte.
