# Documentation Structure Guide

This guide defines the file and authoring contract for pages rendered in the Scopien admin documentation experience.

## Current repository layout

```text
.
├── nav.yml
├── docs/
│   ├── README.md
│   ├── getting-started/
│   ├── user-guide/
│   ├── api-reference/
│   ├── advanced/
│   └── markdown-*.md
└── dev docs/
    ├── README-NAVIGATION.md
    ├── docs-structure-guide.md
    ├── nav-variations-guide.md
    ├── MARKDOWN-TESTING-GUIDE.md
    └── COMPLETE-MARKDOWN-TEST-SUITE.md
```

Only files below the configured docs path are fetched as pages. The dev docs directory is intentionally outside that path.

## Naming rules

Use lowercase kebab-case for directories and Markdown filenames:

```text
docs/getting-started/quick-start.md
docs/api-reference/authentication.md
```

Keep filenames stable after publication. The public admin route is derived from nav.yml titles, while relative links and external bookmarks may still depend on the source file path.

Choose short, task-oriented titles. Avoid encoding sequence numbers in filenames because nav.yml already owns ordering.

## Page anatomy

A normal page should use this order:

```markdown
---
title: Optional repository metadata
description: Optional repository metadata
---

# Page title

One short introductory paragraph that explains the page outcome.

## First task or concept

Content.

## Next task or concept

Content.
```

The renderer uses the first H1 as the visual page title. The first plain paragraph after it becomes the page summary. Lists, tables, code, HTML, and other blocks stay in the article body.

Frontmatter is removed before rendering. It is allowed for repository tooling, but the current admin UI does not use its fields for navigation or page metadata.

## Heading rules

- Use one ATX-style H1 with a leading hash.
- Use H2 for major page sections and H3/H4 for nested detail.
- Do not skip heading levels without a content reason.
- Keep heading text unique within a page when possible.
- Prefer ASCII words in headings that need stable fragment links.

The quick-navigation panel includes H2 through H6 headings. Heading IDs are generated from their text; duplicate headings receive numeric suffixes.

Setext headings are not included in quick navigation. Explicit custom ID syntax such as {#custom-id} is not interpreted as a custom anchor.

## Links

Use relative Markdown links for pages in this repository:

```markdown
[Install Scopien](../getting-started/installation.md)
[Review authentication](../api-reference/authentication.md#token-authentication)
```

The admin panel rewrites .md links only when the target page is present in nav.yml. Unmapped Markdown links remain unchanged and may not resolve inside the application.

Use normal fragment links for headings on the same page:

```markdown
[Jump to troubleshooting](#troubleshooting)
```

External HTTP and HTTPS links open in a new tab.

## Images and media

Use reachable HTTPS URLs for published images:

```markdown
![Connection status](https://assets.example.com/docs/connection-status.png)
```

Repository-relative image paths are not rewritten to GitHub raw URLs. Do not rely on paths such as ./images/example.png unless the application gains an asset resolver.

Always provide meaningful alt text. Decorative images should use empty alt text.

## Supported content

The documentation renderer supports:

- CommonMark paragraphs, emphasis, links, images, lists, and blockquotes.
- GitHub Flavored Markdown tables, task lists, autolinks, and strikethrough.
- Fenced and indented code blocks with a copy action.
- Mermaid diagrams in fenced mermaid blocks.
- Trusted raw HTML authored in the documentation repository.
- Line breaks authored as single newlines.

The renderer does not currently provide TeX math processing, custom heading IDs, tab components, or a general shortcode/component system.

Raw HTML is powerful and should be used sparingly. Do not add scripts, event handlers, forms that collect data, or remote embeds without a security review.

## Navigation ownership

Creating a file does not publish it when nav.yml is present. Add the page to nav.yml in the desired order.

When moving or renaming a published page:

1. Move the file.
2. Update its nav.yml path.
3. Update inbound Markdown links.
4. Check bookmarks or external references that may use the old admin route.
5. Preview previous/next order and breadcrumbs.

The application has no repository-level redirects.yml integration. Coordinate route changes instead of documenting a redirect file the runtime does not consume.

## Content quality

Each page should:

- State its outcome before prerequisites and steps.
- Use short sections and descriptive headings.
- Put prerequisites before instructions that depend on them.
- Use tested commands and label placeholders clearly.
- Avoid claims about product behavior that are not verified.
- Avoid dates or versions unless someone owns their maintenance.

## Review checklist

- [ ] Filename and directory follow lowercase kebab-case.
- [ ] Page has one H1 and an introductory paragraph.
- [ ] Heading hierarchy is coherent.
- [ ] nav.yml publishes the page once.
- [ ] Relative .md links target other published pages.
- [ ] Images use reachable HTTPS URLs and useful alt text.
- [ ] Code blocks specify a language where useful.
- [ ] No unsupported math, shortcode, or custom-ID syntax is presented as working.
- [ ] No secret, customer data, or private operational detail is included.
