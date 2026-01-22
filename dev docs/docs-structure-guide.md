# Documentation Structure Guide

This guide outlines the recommended structure and improvements for your documentation, based on best practices from n8n and other documentation sites.

## Current Structure ✅

Your documentation is well-organized with:
- ✅ Proper frontmatter (title, description)
- ✅ Logical hierarchy (Getting Started → User Guide → API → Advanced)
- ✅ Clean navigation structure
- ✅ Good content with examples and code blocks

## Recommended Improvements

### 1. Add More Navigation Metadata

**Current nav.yml:**
```yaml
nav:
  - Getting Started:
    - Welcome: docs/README.md
```

**Enhanced version with metadata:**
```yaml
nav:
  - Getting Started:
    - Welcome:
        path: docs/README.md
        icon: 🏠
        description: Start your journey with Scopien AI
    - Introduction:
        path: docs/getting-started/introduction.md
        icon: 📖
        description: Learn what Scopien AI can do
```

### 2. Add Index Files for Each Section

Create index pages for better navigation:
- `docs/getting-started/index.md`
- `docs/user-guide/index.md`
- `docs/api-reference/index.md`
- `docs/advanced/index.md`

### 3. Add More Documentation Sections

Consider adding:

```yaml
nav:
  - Getting Started: [existing]

  - User Guide: [existing]

  - Tutorials:
    - Video Courses: docs/tutorials/video-courses.md
    - Text Tutorials:
      - Your First Query: docs/tutorials/first-query.md
      - Creating Automations: docs/tutorials/creating-automations.md
      - Advanced Filtering: docs/tutorials/advanced-filtering.md

  - Use Cases:
    - Sales Teams: docs/use-cases/sales.md
    - Service Teams: docs/use-cases/service.md
    - Marketing Teams: docs/use-cases/marketing.md
    - Executives: docs/use-cases/executives.md

  - API Reference: [existing]

  - Integrations:
    - Salesforce Clouds:
      - Sales Cloud: docs/integrations/sales-cloud.md
      - Service Cloud: docs/integrations/service-cloud.md
      - Marketing Cloud: docs/integrations/marketing-cloud.md
    - Third-Party Apps:
      - Slack: docs/integrations/slack.md
      - Microsoft Teams: docs/integrations/teams.md
      - Zapier: docs/integrations/zapier.md

  - Advanced: [existing]

  - Resources:
    - Glossary: docs/resources/glossary.md
    - FAQ: docs/resources/faq.md
    - Release Notes: docs/resources/releases.md
    - Video Library: docs/resources/videos.md
    - Community: docs/resources/community.md
```

### 4. Add Navigation Tags/Badges

```yaml
nav:
  - Getting Started:
    - Quick Start:
        path: docs/getting-started/quick-start.md
        badge: "5 min"
        tags: ["beginner", "tutorial"]

  - API Reference:
    - Webhooks:
        path: docs/api-reference/webhooks.md
        badge: "New"
        tags: ["api", "advanced"]
```

### 5. Implement Search Metadata

Add to frontmatter:
```yaml
---
title: AI Assistant Guide
description: Master the Scopien AI Assistant
keywords: [ai, assistant, commands, automation, salesforce]
tags: [user-guide, ai, core-feature]
category: user-guide
weight: 2
---
```

### 6. Add Versioning Support

```yaml
nav:
  version: "1.0.0"
  versions:
    - 1.0.0: /docs/v1
    - 0.9.0: /docs/v0.9

  - Getting Started: [...]
```

### 7. Create a Sidebar Configuration

```yaml
sidebar:
  collapsed: false
  collapsible_sections:
    - Getting Started
    - User Guide
    - API Reference

  always_expanded:
    - Getting Started

  footer_links:
    - Support: https://support.scopien.com
    - Community: https://community.scopien.com
    - Status: https://status.scopien.com
```

### 8. Add Breadcrumb Support

```yaml
breadcrumb:
  enabled: true
  separator: "/"
  home_text: "Home"
```

### 9. Implement Related Pages

Add to markdown frontmatter:
```yaml
---
title: AI Assistant Guide
related:
  - docs/user-guide/dashboard.md
  - docs/advanced/custom-workflows.md
  - docs/getting-started/quick-start.md
next: docs/user-guide/salesforce-integration.md
prev: docs/user-guide/dashboard.md
---
```

### 10. Add External Links Section

```yaml
nav:
  # ... existing navigation

  external_links:
    - Support Center:
        url: https://support.scopien.com
        icon: 🆘
    - Community Forum:
        url: https://community.scopien.com
        icon: 💬
    - Status Page:
        url: https://status.scopien.com
        icon: 📊
    - API Playground:
        url: https://api.scopien.com/playground
        icon: 🎮
        badge: "Interactive"
```

## File Organization Best Practices

### Current Structure:
```
docs/
├── README.md
├── getting-started/
├── user-guide/
├── api-reference/
└── advanced/
```

### Recommended Structure:
```
docs/
├── index.md                    # Main landing page
├── getting-started/
│   ├── index.md               # Section overview
│   ├── introduction.md
│   ├── installation.md
│   └── quick-start.md
├── user-guide/
│   ├── index.md
│   ├── dashboard.md
│   ├── ai-assistant.md
│   └── salesforce-integration.md
├── tutorials/
│   ├── index.md
│   ├── videos/
│   └── text/
├── use-cases/
│   ├── index.md
│   ├── sales.md
│   ├── service.md
│   └── marketing.md
├── api-reference/
│   ├── index.md
│   ├── authentication.md
│   ├── endpoints.md
│   └── webhooks.md
├── integrations/
│   ├── index.md
│   ├── salesforce/
│   └── third-party/
├── advanced/
│   ├── index.md
│   ├── custom-workflows.md
│   ├── security.md
│   └── troubleshooting.md
└── resources/
    ├── glossary.md
    ├── faq.md
    ├── releases.md
    └── community.md
```

## Frontend Integration Considerations

### 1. Navigation Data Structure

Your frontend should parse the nav.yml into a nested structure:

```typescript
interface NavItem {
  label: string;
  path?: string;
  icon?: string;
  badge?: string;
  description?: string;
  children?: NavItem[];
  external?: boolean;
  tags?: string[];
}

interface Navigation {
  nav: NavItem[];
  version?: string;
  sidebar?: SidebarConfig;
  breadcrumb?: BreadcrumbConfig;
}
```

### 2. Fetching Strategy

```typescript
// Fetch navigation structure
const nav = await fetch('https://api.github.com/repos/user/repo/contents/nav.yml')
  .then(res => res.json())
  .then(data => parseYAML(atob(data.content)));

// Fetch individual doc pages
const doc = await fetch(`https://api.github.com/repos/user/repo/contents/${path}`)
  .then(res => res.json())
  .then(data => parseMarkdown(atob(data.content)));
```

### 3. Caching Recommendations

- Cache nav.yml for 1 hour
- Cache individual pages for 5 minutes
- Use ETags for efficient updates
- Implement service worker for offline access

### 4. Search Implementation

Add to nav.yml:
```yaml
search:
  enabled: true
  placeholder: "Search documentation..."
  index_fields:
    - title
    - description
    - content
    - keywords
```

## Additional Files to Create

### 1. docs.config.yml
```yaml
site:
  title: Scopien AI Documentation
  description: Official documentation for Scopien AI
  url: https://docs.scopien.com
  logo: /assets/logo.svg
  favicon: /assets/favicon.ico

theme:
  primary_color: "#0066cc"
  sidebar_width: 280
  content_width: 800
  code_theme: "github-dark"

features:
  search: true
  dark_mode: true
  edit_on_github: true
  github_repo: "yourusername/testing-docs"
  feedback_widget: true
  table_of_contents: true
  breadcrumbs: true
  prev_next_navigation: true

analytics:
  google_analytics: "GA-XXXXX"
  mixpanel: "xxxxx"

seo:
  og_image: /assets/og-image.png
  twitter_card: summary_large_image
  twitter_site: "@scopienai"
```

### 2. redirects.yml
```yaml
redirects:
  /old-path: /new-path
  /getting-started/setup: /getting-started/installation
```

### 3. algolia.yml (for search)
```yaml
index_name: scopien-docs
application_id: XXXXX
api_key: XXXXX
```

## Metadata Enhancements

### Enhanced Frontmatter Example:
```yaml
---
title: AI Assistant Guide
description: Master the Scopien AI Assistant for maximum productivity
keywords: [ai assistant, salesforce automation, natural language, voice commands]
author: Scopien Team
last_updated: 2026-01-22
category: user-guide
tags: [core-feature, ai, productivity]
difficulty: beginner
reading_time: 15
related:
  - docs/user-guide/dashboard.md
  - docs/advanced/custom-workflows.md
next: docs/user-guide/salesforce-integration.md
prev: docs/user-guide/dashboard.md
toc: true
comments: true
featured: true
---
```

## Testing Your Navigation

### 1. Validate YAML Structure
```bash
# Install yamllint
pip install yamllint

# Validate nav.yml
yamllint nav.yml
```

### 2. Test All Links
Create a script to verify all paths exist:
```bash
# check-links.sh
while IFS= read -r line; do
  path=$(echo "$line" | grep -oP '(?<=: ).*\.md$')
  if [ -n "$path" ] && [ ! -f "$path" ]; then
    echo "Missing: $path"
  fi
done < nav.yml
```

## Summary of Immediate Improvements

✅ **Already Good:**
- Basic navigation structure
- Proper frontmatter
- Logical organization

🚀 **Quick Wins (Implement Now):**
1. ✅ Created `nav.yml` (Done!)
2. Add `docs.config.yml` for site-wide settings
3. Create index.md files for each section
4. Add more sections (Tutorials, Use Cases, Resources)

📈 **Medium Priority:**
5. Add navigation metadata (icons, badges, descriptions)
6. Implement search metadata in frontmatter
7. Add related pages and prev/next navigation
8. Create glossary and FAQ sections

🎯 **Long Term:**
9. Versioning support
10. External links section
11. Advanced search with Algolia
12. Analytics and feedback widgets

## Example Enhanced nav.yml

See the file I created at `/nav.yml` - it follows n8n's structure with your current content!
