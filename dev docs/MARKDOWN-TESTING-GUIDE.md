# Markdown Testing Guide

This guide describes the Markdown behavior currently implemented by the Scopien admin documentation renderer and the checks required before publishing complex pages.

## Renderer contract

Documentation uses the shared Scopien Markdown renderer with GitHub Flavored Markdown, authored line breaks, trusted raw HTML, Mermaid diagrams, code-copy controls, and custom table/image styling.

The documentation page adds:

- A page header built from the leading H1 and first plain paragraph.
- Heading IDs shared by rendered headings and quick navigation.
- Desktop and mobile quick-navigation controls for H2 through H6.
- Rewriting of mapped relative .md links to admin documentation routes.
- Previous and next pages derived from nav.yml order.

## Supported feature matrix

| Feature                     | Expected behavior                                   |
| --------------------------- | --------------------------------------------------- |
| H1-H6 ATX headings          | Rendered with generated fragment IDs                |
| Paragraphs and line breaks  | Single authored newlines are preserved              |
| Bold, italic, strikethrough | Rendered                                            |
| Ordered and unordered lists | Rendered, including nesting                         |
| Task lists                  | Rendered as disabled checkboxes                     |
| Links and autolinks         | Rendered; external links open a new tab             |
| Images                      | Lazy-loaded with authored alt text                  |
| Blockquotes                 | Rendered with themed surface styling                |
| GFM tables                  | Wrapped in a keyboard-focusable horizontal scroller |
| Inline code                 | Rendered with themed inline styling                 |
| Fenced code                 | Rendered with language label and copy action        |
| Indented code               | Rendered as code                                    |
| Mermaid fence               | Rendered by the Mermaid component                   |
| Raw HTML                    | Rendered for trusted repository content             |

## Known limitations

Do not mark these as supported without an application change:

- TeX or LaTeX math rendering.
- Setext headings in quick navigation.
- Explicit custom heading ID syntax.
- Repository-relative image URL rewriting.
- Generic shortcodes, MDX components, or framework components.
- Guaranteed syntax highlighting for every fenced-code language.
- Arbitrary MDI icon rendering from nav.yml.

Complex raw HTML may render, but it must not be used to execute scripts, capture data, or bypass the application design system.

## Test fixtures

The published Resources > Markdown Testing folder contains four fixtures:

| File                                 | Primary purpose                                               |
| ------------------------------------ | ------------------------------------------------------------- |
| docs/markdown-cheatsheet.md          | Broad syntax inventory and basic visual regression            |
| docs/markdown-real-world-examples.md | Long-form documentation patterns                              |
| docs/markdown-ultimate-complete.md   | HTML, media, layout, and interaction edge cases               |
| docs/markdown-extreme-stress-test.md | Performance, overflow, deep nesting, and large-content stress |

These are QA fixtures, not promises that every authored example is a supported product feature. Unsupported examples should be labelled clearly in the fixture itself when it is next edited.

Some long-form examples also use fictional relative destinations to demonstrate link syntax. Use links to actual nav.yml pages, or a reduced test snippet, when testing route rewriting.

## Local preview

Run ScopienOS on port 3000 and open:

```text
http://localhost:3000/admin/documentation/home
```

The account must have an authenticated admin session. The ScopienOS environment must point to the repository and include REPOSITORY_TOKEN when the repository is private.

For the canonical repository:

```dotenv
REPOSITORY_TOKEN=repository-scoped-secret
NEXT_PUBLIC_DOCS_REPO=ScopienInc/Scopien-Docs
NEXT_PUBLIC_DOCS_BRANCH=main
NEXT_PUBLIC_DOCS_PATH=docs
```

Never commit the token. Restart the app after changing environment variables.

## Smoke-test order

Test from the smallest and most deterministic fixture to the largest:

1. markdown-cheatsheet.md
2. markdown-real-world-examples.md
3. markdown-ultimate-complete.md
4. markdown-extreme-stress-test.md

This order makes it easier to distinguish a renderer bug from a stress-only performance problem.

## Page-shell checks

On each fixture, verify:

- The leading H1 appears once in the page header.
- A leading plain paragraph becomes the summary without losing content.
- A leading list, table, code block, or HTML block stays in the article body.
- Breadcrumbs match the sidebar location.
- Quick navigation appears when the body contains H2-H6 headings.
- Clicking quick navigation activates the selected heading, not its neighbor.
- Refreshing a URL with a heading fragment lands at that heading.
- Previous and next pages follow nav.yml order.

## Markdown checks

### Text and lists

- Bold, italic, strikethrough, inline code, and escaped punctuation render correctly.
- Ordered, unordered, mixed, and deeply nested lists retain their hierarchy.
- Task-list checkboxes are visible and non-editable.
- Long words and long URLs do not force the entire page wider.

### Code

- Fenced and indented code preserve whitespace.
- The language label is sensible.
- Copy places only the code text on the clipboard.
- Long lines scroll inside the code surface.
- A mermaid fence renders a diagram or a controlled error state.

### Tables

- Header and body cells align.
- Wide tables scroll horizontally without widening the page.
- The table wrapper can receive keyboard focus.
- Inline formatting inside cells remains legible.

### Links and images

- Same-page fragments reach the correct heading.
- Relative .md links to pages mapped in nav.yml reach their admin routes.
- Relative links keep query strings and fragments.
- External links open a new tab.
- Images load lazily and preserve layout.
- Broken images retain useful alt text.

### Raw HTML

- Allowed structural HTML stays inside the reading column.
- Details/summary controls work with keyboard input.
- Embedded media does not overflow the rounded canvas.
- No script, inline event handler, credential form, or unreviewed third-party embed is present.

## Responsive checks

Test at minimum:

- 375px wide mobile viewport.
- 768px tablet viewport.
- 1280px desktop viewport.
- A wide desktop viewport near 1800px.

Verify that the mobile navigation opens, the quick-nav select is usable, the article does not overflow, and the desktop article and quick nav remain in their intended columns.

## Accessibility checks

- Navigate the sidebar, quick navigation, links, tables, details controls, and code copy using only the keyboard.
- Confirm visible focus indicators.
- Confirm heading order with an accessibility tree or screen reader.
- Confirm images have appropriate alt text.
- Confirm link text describes the destination.
- Confirm color is not the only way active or error state is communicated.

## Performance checks

The extreme fixture is intentionally large. Check:

- Initial render completes without freezing the tab.
- Scrolling remains responsive.
- Quick navigation is populated after load.
- Changing headings does not cause repeated layout jumps.
- Images do not all load eagerly.
- Mermaid failures remain isolated to their block.

Do not maintain arbitrary millisecond budgets in this repository. Use the same machine and build mode when comparing a suspected regression.

## Reporting a defect

Include:

1. Repository, branch, page path, and commit.
2. Browser and viewport.
3. Smallest Markdown snippet that reproduces the issue.
4. Expected and actual behavior.
5. Screenshot or recording for a visual issue.
6. Console error and network response when relevant.

Reduce the issue in a small fixture before using the extreme stress page as the reproduction.
