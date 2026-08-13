# Navigation Schema and Patterns

This guide documents the nav.yml shapes currently parsed by ScopienOS. Examples are intentionally limited to supported behavior.

## Root schema

nav.yml contains one nav array:

```yaml
nav:
  - type: link
    title: Home
    path: docs/README.md
    icon: mdi:home

  - type: section
    title: Get Started
    items:
      - title: Introduction
        path: docs/getting-started/introduction.md
        icon: mdi:book-open
```

Top-level modern entries must be either link or section.

## Field reference

| Field       | Used on           | Required   | Behavior                                          |
| ----------- | ----------------- | ---------- | ------------------------------------------------- |
| type        | Top-level entry   | Yes        | link or section                                   |
| title       | Any visible item  | Yes        | Sidebar label and route slug source               |
| path        | Link or page item | For a page | Repository path to a Markdown file                |
| icon        | Any visible item  | No         | Hint mapped to the app icon set                   |
| items       | Top-level section | Yes        | Direct section children                           |
| children    | Nested item       | No         | Nested navigation children                        |
| collapsible | Nested item       | No         | Makes an item a folder even before children exist |
| defaultOpen | Nested folder     | No         | Opens that folder on initial render               |

Use items directly below a top-level section. Use children for every deeper level.

## Pattern 1: top-level page

```yaml
- type: link
  title: Home
  path: docs/README.md
  icon: mdi:home
```

This creates /admin/documentation/home.

## Pattern 2: section with pages

```yaml
- type: section
  title: API Reference
  items:
    - title: Authentication
      path: docs/api-reference/authentication.md
      icon: mdi:shield-key
    - title: Endpoints
      path: docs/api-reference/endpoints.md
      icon: mdi:api
```

Section labels group the sidebar but do not become part of child routes. These pages resolve to /authentication and /endpoints below the documentation base path.

Because section titles are not route parents, child titles should be unique across top-level sections.

## Pattern 3: collapsible folder

```yaml
- type: section
  title: Guides
  items:
    - title: Integrations
      icon: mdi:connection
      collapsible: true
      defaultOpen: true
      children:
        - title: Salesforce Integration
          path: docs/user-guide/salesforce-integration.md
          icon: mdi:cloud-sync
```

The child route is /integrations/salesforce-integration. Folder ancestry appears in breadcrumbs.

## Pattern 4: clickable folder

A folder may have its own page and children:

```yaml
- type: section
  title: Guides
  items:
    - title: Automation
      path: docs/automation/README.md
      icon: mdi:workflow
      collapsible: true
      defaultOpen: false
      children:
        - title: Build a Workflow
          path: docs/automation/build-a-workflow.md
          icon: mdi:lightning-bolt
```

The parent page participates in previous/next navigation before its children.

## Pattern 5: deeper nesting

```yaml
- type: section
  title: Guides
  items:
    - title: Integrations
      icon: mdi:connection
      children:
        - title: Salesforce
          icon: mdi:cloud
          children:
            - title: Authentication
              path: docs/integrations/salesforce/authentication.md
              icon: mdi:key
```

The route is /integrations/salesforce/authentication. The runtime limits directory fallback recursion to ten levels, but authored navigation should stay within three levels for usability.

## Route generation

Routes are created from titles, not filenames. Titles are lowercased, ampersands become and, punctuation is removed, whitespace becomes hyphens, and repeated hyphens collapse.

Examples:

| Title                 | Route segment           |
| --------------------- | ----------------------- |
| Quick Start           | quick-start             |
| Security & Compliance | security-and-compliance |
| API: Authentication   | api-authentication      |

Changing a title can therefore change the admin URL even when the file path stays the same.

## Page ordering

Previous and next navigation follows a depth-first traversal of nav.yml:

1. Top-level links in file order.
2. Section items in file order.
3. A clickable parent before its children.
4. Nested children in file order.

Search uses the same navigation tree and matches item titles and generated paths.

## Icons

Icons are hints, not arbitrary runtime icon imports. The app maps known icon/title words to its Lucide icon set and falls back to a file icon.

Use an existing mdi-style value already present in nav.yml. Validate the rendered result; a valid MDI name is not a guarantee that a distinct icon exists in the admin UI.

## Legacy schema

The parser still accepts the older object/array shape:

```yaml
nav:
  - Home: docs/README.md
  - Guides:
      - Introduction: docs/getting-started/introduction.md
```

Do not add new entries in this form. It cannot express the modern icon and folder-default behavior clearly. Convert legacy entries when editing nearby navigation.

## Invalid or ineffective shapes

Avoid:

- A top-level section without items.
- A nested page without title or path.
- Using items below a nested folder; use children there.
- A path outside the configured docs directory.
- Duplicate titles that generate the same route.
- Empty folders with neither a page nor children.
- Treating collapsible as a visual-only flag on a normal page.

Invalid entries may be dropped during parsing. A nav.yml that produces no entries causes documentation loading to fail.

## Fallback behavior

If nav.yml returns 404, the app builds an alphabetical tree from Markdown files under the configured docs directory. Directories come before files.

Fallback mode does not preserve curated labels, icons, ordering, or default-open folders. It is recovery behavior, not the recommended publishing mode.

## Change checklist

- [ ] Every path exists.
- [ ] Every published page appears once.
- [ ] New titles generate unique routes.
- [ ] Top-level sections use items.
- [ ] Nested folders use children.
- [ ] Clickable parents have both path and children.
- [ ] defaultOpen is used only where initial expansion helps.
- [ ] Desktop and mobile sidebar behavior is verified.
- [ ] Breadcrumbs and previous/next order match the intended reading path.
