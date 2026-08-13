# Markdown Fixture Inventory and Release Gate

This document defines what each Markdown fixture is responsible for and how to decide whether a documentation renderer change is ready to ship.

## Scope

The suite exercises the admin documentation experience end to end:

- GitHub-backed page loading.
- nav.yml ordering and routes.
- Markdown and trusted HTML rendering.
- Heading extraction and quick navigation.
- Link rewriting.
- Responsive layout and overflow containment.
- Accessibility and interaction.
- Large-page performance.

The files are intentionally broad. They are manual and exploratory fixtures, not automated assertions.

## Fixture 1: Markdown cheat sheet

Path: docs/markdown-cheatsheet.md

Use it for the first rendering pass. It provides the fastest way to find regressions in common syntax.

Primary coverage:

- ATX headings.
- Text emphasis and inline code.
- Ordered, unordered, nested, and task lists.
- Links, autolinks, and images.
- Blockquotes.
- Fenced and indented code.
- GFM tables.
- Escaping and special characters.

Release expectation: common supported elements render correctly before testing any larger fixture.

## Fixture 2: Real-world examples

Path: docs/markdown-real-world-examples.md

Use it to evaluate full-page reading behavior rather than isolated syntax.

Primary coverage:

- API reference layouts.
- Tutorials and procedural steps.
- Long code samples.
- Comparison tables.
- Changelog entries.
- Error documentation.
- FAQs and best-practice guides.

Release expectation: long-form pages remain readable, navigable, and contained within the article column.

This fixture contains fictional product links for realism. Those destinations are not link-integrity assertions. Test route rewriting with a known nav.yml page.

## Fixture 3: Ultimate complete

Path: docs/markdown-ultimate-complete.md

Use it for rich-content and raw-HTML compatibility checks.

Primary coverage:

- Images in lists, tables, and links.
- Wide and complex tables.
- HTML layout constructs and details controls.
- Media and embed examples.
- Nested combinations.
- Unicode and unusual content.
- Long words, URLs, and dense layouts.

Some examples intentionally exceed the supported Markdown contract. Treat them as graceful-degradation checks, not as requirements that the app implement every browser or HTML feature.

Release expectation: supported content works and unsupported content fails safely without breaking the page shell.

## Fixture 4: Extreme stress test

Path: docs/markdown-extreme-stress-test.md

Use it last. It is designed to expose scaling, overflow, and synchronization failures.

Primary coverage:

- Deep nesting.
- Very large tables.
- Large numbers of links, images, and emoji.
- Long single-line content.
- Complex Mermaid source.
- International characters.
- Dense mixed-content sections.

Math examples are plain text/code unless TeX support is added to the application. They must not be used as proof of math-rendering support.

Release expectation: the page stays usable, the browser remains responsive, and a single unsupported block does not break surrounding content.

## Coverage matrix

| Behavior              | Cheat sheet |  Real world  |   Ultimate   |   Extreme    |
| --------------------- | :---------: | :----------: | :----------: | :----------: |
| Common Markdown       |   Primary   |     Yes      |     Yes      |     Yes      |
| GFM tables and tasks  |   Primary   |     Yes      |   Primary    |    Stress    |
| Code blocks           |   Primary   |   Primary    |     Yes      |    Stress    |
| Images                |   Primary   |     Yes      |   Primary    |    Stress    |
| Raw HTML              |    Some     |     Some     |   Primary    |    Stress    |
| Long-form readability |    Some     |   Primary    |     Yes      |    Stress    |
| Responsive overflow   |     Yes     |     Yes      |   Primary    |   Primary    |
| Quick navigation      |     Yes     |   Primary    |   Primary    |   Primary    |
| Link rewriting        |   Primary   | Illustrative | Illustrative | Illustrative |
| Accessibility         |   Primary   |   Primary    |   Primary    |    Stress    |
| Performance           |  Baseline   |   Baseline   |    Heavy     |   Primary    |

## Required environments

Run the release gate against:

- A local development build for fast diagnosis.
- A production build when the change affects rendering, hydration, caching, or bundle behavior.
- The canonical GitHub repository configuration.

Use at least one Chromium browser. Add Safari or Firefox when the change touches scrolling, sticky positioning, raw media, or browser-specific CSS.

## Release gate

### Repository and navigation

- [ ] nav.yml loads without error.
- [ ] Every referenced Markdown path exists.
- [ ] Home is the stable landing page.
- [ ] Sidebar order matches nav.yml.
- [ ] Search finds nested pages.
- [ ] Folder default-open behavior is correct.
- [ ] Breadcrumbs reflect nested folders.
- [ ] Previous and next follow depth-first nav order.

### Page header and headings

- [ ] The leading H1 appears once.
- [ ] The first plain paragraph becomes the summary.
- [ ] Leading non-paragraph blocks remain in the article.
- [ ] Quick navigation includes H2-H6 headings.
- [ ] Duplicate headings receive distinct targets.
- [ ] A clicked heading remains active after smooth scrolling ends.
- [ ] The last heading activates at the bottom of the page.

### Rendering

- [ ] Common Markdown renders correctly.
- [ ] GFM tables and task lists render correctly.
- [ ] Code copy works.
- [ ] Mermaid renders or shows an isolated controlled failure.
- [ ] Trusted HTML cannot break the page shell.
- [ ] Unsupported math/custom-ID syntax is not presented as supported.
- [ ] Relative links to pages mapped in nav.yml resolve.
- [ ] External links open safely in a new tab.
- [ ] Images use reachable URLs and useful alt text.

### Layout and theme

- [ ] Outer gutter and docs sidebar retain the approved dark treatment.
- [ ] The rounded reading canvas retains the shared Scopien glass background.
- [ ] No unintended decorative border appears on the canvas or quick nav.
- [ ] Article width remains readable on wide screens.
- [ ] Quick navigation remains visible in its desktop column.
- [ ] Mobile navigation and quick-nav select remain usable.
- [ ] Tables, code, media, long URLs, and long words do not widen the page.

### Accessibility

- [ ] All controls work by keyboard.
- [ ] Focus indicators are visible.
- [ ] Headings form a coherent outline.
- [ ] Active navigation is exposed with the correct current-state semantics.
- [ ] Table overflow regions are keyboard reachable.
- [ ] Images have appropriate alt text.

### Performance and reliability

- [ ] The extreme page does not freeze the browser.
- [ ] Scrolling and active-heading updates remain responsive.
- [ ] Quick navigation loads reliably on direct navigation and refresh.
- [ ] GitHub errors produce a controlled unavailable/not-found state.
- [ ] No token or private repository data is exposed to client code.

## Failure severity

Use these levels when reporting results:

| Level   | Meaning                                                | Examples                                              |
| ------- | ------------------------------------------------------ | ----------------------------------------------------- |
| Blocker | Documentation cannot be used or exposes sensitive data | Repository fails to load, token leak                  |
| High    | Core navigation or reading flow is broken              | Wrong page, missing quick nav, unusable mobile layout |
| Medium  | Supported content is incorrect but a workaround exists | Table overflow, code copy failure                     |
| Low     | Cosmetic issue with no meaningful loss of function     | Minor spacing or color inconsistency                  |

## Regression record

For each release candidate, record:

- ScopienOS commit.
- Documentation repository commit.
- Environment and build mode.
- Browser and viewport set.
- Fixtures tested.
- Failures with severity and reproduction.
- Reviewer and date.

Do not write a permanent pass/fail claim into this file. Test results belong in the pull request or release record because the fixtures and renderer continue to change.

## Suite maintenance

When adding a renderer feature:

1. Add the smallest representative example to the cheat sheet.
2. Add a realistic example only if the feature appears in normal documentation.
3. Add an edge or scaling example only when it can reveal a distinct failure.
4. Update the feature matrix and known limitations in MARKDOWN-TESTING-GUIDE.md.
5. Remove stale examples instead of preserving unsupported claims indefinitely.

Keep this inventory concise. The fixtures demonstrate syntax; this document defines ownership and acceptance criteria.
