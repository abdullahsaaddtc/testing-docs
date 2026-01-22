# Navigation Variations Guide

This guide explains all the navigation patterns demonstrated in `nav.yml`.

## Overview of Variations

The `nav.yml` file now contains **12 different variations** showing every possible navigation structure you might need. Here's what each demonstrates:

---

## VARIATION 1: Direct Top-Level Link
**Pattern:** Simple direct link at the root level

```yaml
- Home: docs/README.md
```

**Use when:**
- You need a quick link to a single page
- Creating a homepage or landing page link
- Adding standalone pages like "About", "Contact"

**Frontend rendering:**
```
Home
```

---

## VARIATION 2: Simple Section with Direct Children
**Pattern:** One level of nesting

```yaml
- Getting Started:
  - Introduction: docs/getting-started/introduction.md
  - Installation: docs/getting-started/installation.md
  - Quick Start: docs/getting-started/quick-start.md
```

**Use when:**
- Grouping related pages under a category
- Creating a simple menu section
- All items are at the same level of importance

**Frontend rendering:**
```
▼ Getting Started
  ├─ Introduction
  ├─ Installation
  └─ Quick Start
```

---

## VARIATION 3: Section with Nested Subsections
**Pattern:** Two levels deep - section with both direct links and subsections

```yaml
- User Guide:
  - Overview: docs/user-guide/index.md
  - Dashboard:
    - Dashboard Overview: docs/user-guide/dashboard.md
    - Dashboard Settings: docs/user-guide/dashboard-settings.md
  - AI Assistant: docs/user-guide/ai-assistant.md
```

**Use when:**
- Some items need grouping, others don't
- Creating a mixed hierarchy
- One subsection needs more detail than others

**Frontend rendering:**
```
▼ User Guide
  ├─ Overview
  ├▼ Dashboard
  │  ├─ Dashboard Overview
  │  ├─ Dashboard Settings
  │  └─ Custom Widgets
  ├─ AI Assistant
  └─ Salesforce Integration
```

---

## VARIATION 4: Deep Nesting (3+ Levels)
**Pattern:** Multiple levels of subsections

```yaml
- Integrations:
  - Salesforce Clouds:
    - Sales Cloud:
      - Overview: docs/...
      - Advanced Features:
        - Territory Management: docs/...
        - Price Books: docs/...
```

**Use when:**
- Complex feature documentation
- Product with many sub-features
- Hierarchical information architecture

**Frontend rendering:**
```
▼ Integrations
  ├─ Overview
  └▼ Salesforce Clouds
     └▼ Sales Cloud
        ├─ Overview
        ├─ Opportunities
        └▼ Advanced Features
           ├─ Territory Management
           └─ Price Books
```

**⚠️ Warning:** Avoid going deeper than 4-5 levels as it becomes hard to navigate.

---

## VARIATION 5: Mixed Structure
**Pattern:** Combining different nesting depths at the same parent level

```yaml
- Tutorials:
  - Overview: docs/tutorials/index.md
  - Quick Tutorials: docs/tutorials/quick-start.md
  - Video Courses:
    - Beginner Course:
      - Lesson 1: docs/...
  - Interactive Labs: docs/tutorials/labs.md
```

**Use when:**
- Some subsections need deep nesting, others don't
- Flexible content organization
- Different content types in same section

**Frontend rendering:**
```
▼ Tutorials
  ├─ Overview
  ├─ Quick Tutorials
  ├▼ Video Courses
  │  ├▼ Beginner Course
  │  │  ├─ Introduction
  │  │  └─ Lesson 1
  │  └▼ Advanced Course
  └─ Interactive Labs
```

---

## VARIATION 6: API Reference Structure
**Pattern:** Organized by REST resources and operations

```yaml
- API Reference:
  - Authentication:
    - OAuth 2.0: docs/...
  - Endpoints:
    - REST API:
      - Users:
        - List Users: docs/...
        - Create User: docs/...
```

**Use when:**
- Documenting APIs
- CRUD operations need to be grouped
- Multiple API types (REST, GraphQL, etc.)

**Best practices:**
- Group by resource (Users, Accounts, etc.)
- Then by operation (List, Create, Update, Delete)
- Keep consistent naming patterns

---

## VARIATION 7: Use Cases by Category
**Pattern:** Real-world examples organized by dimensions

```yaml
- Use Cases:
  - By Department:
    - Sales Teams:
      - Lead Management: docs/...
  - By Industry:
    - Healthcare: docs/...
```

**Use when:**
- Content can be categorized multiple ways
- Users have different entry points
- Solution-focused documentation

**Frontend rendering:**
```
▼ Use Cases
  ├─ Overview
  ├▼ By Department
  │  ├▼ Sales Teams
  │  │  ├─ Lead Management
  │  │  └─ Pipeline Automation
  │  └▼ Service Teams
  └▼ By Industry
     ├─ Healthcare
     └─ Finance
```

---

## VARIATION 8: Advanced Topics with Templates
**Pattern:** Complex feature documentation with reusable patterns

```yaml
- Advanced:
  - Workflows:
    - Custom Workflows: docs/...
    - Workflow Templates:
      - Sales Templates:
        - Lead Nurturing: docs/...
      - Service Templates:
        - Case Routing: docs/...
```

**Use when:**
- Power user / admin documentation
- Template-based features
- Configuration-heavy features

---

## VARIATION 9: Resources with Mixed Depth
**Pattern:** Support content with varying complexity

```yaml
- Resources:
  - Glossary: docs/resources/glossary.md
  - Release Notes:
    - Latest: docs/...
    - Archive:
      - 2025: docs/...
```

**Use when:**
- Some resources are simple (single page)
- Others need organization (releases by version)
- Mixed content types

---

## VARIATION 10: Single-Item Section
**Pattern:** Section with only one child

```yaml
- Changelog:
  - All Changes: docs/changelog.md
```

**Use when:**
- Future-proofing for more items
- Maintaining consistent structure
- Section might expand later

**Alternative:** Could be a direct link instead
```yaml
- Changelog: docs/changelog.md
```

**Decision criteria:**
- If you'll add more items → Use section
- If it will always be one page → Use direct link

---

## VARIATION 11: Very Deep Nesting (4-5 Levels)
**Pattern:** Maximum depth for complex enterprise features

```yaml
- Enterprise Features:
  - Administration:
    - User Management:
      - Roles & Permissions:
        - Role Types:
          - Admin Roles: docs/...
```

**Use when:**
- Enterprise-level features
- Complex permission systems
- Highly hierarchical data

**⚠️ Considerations:**
- **UX Challenge:** Users can get lost
- **Mobile:** Difficult on small screens
- **Alternative:** Consider flattening with better naming

**Better alternative:**
```yaml
- Enterprise Features:
  - Admin Roles Overview: docs/...
  - Configure Admin Roles: docs/...
  - Configure User Roles: docs/...
```

---

## VARIATION 12: Parallel Sections
**Pattern:** Multiple independent sections at same level

```yaml
- Developer Tools:
  - CLI:
    - Installation: docs/...
  - IDE Extensions:
    - VS Code: docs/...
  - Testing Tools:
    - Unit Testing: docs/...
```

**Use when:**
- Multiple related but independent topics
- Different tool categories
- Each subsection is self-contained

---

## Navigation Best Practices

### 1. Depth Guidelines
- **1-2 levels:** Ideal for most documentation
- **3 levels:** Good for complex products
- **4 levels:** Maximum recommended depth
- **5+ levels:** Avoid - restructure instead

### 2. Naming Conventions
```yaml
# ✅ Good - Clear and concise
- Getting Started
- User Guide
- API Reference

# ❌ Bad - Too verbose
- Getting Started With Scopien AI Platform
- Complete User Guide and Manual
- Comprehensive API Reference Documentation
```

### 3. Consistency
```yaml
# ✅ Good - Consistent structure
- Sales Cloud:
  - Overview: docs/...
  - Features: docs/...
- Service Cloud:
  - Overview: docs/...
  - Features: docs/...

# ❌ Bad - Inconsistent
- Sales Cloud:
  - Overview: docs/...
  - Features: docs/...
- Service Cloud: docs/... # Missing Overview and Features
```

### 4. Index Pages
Always provide an `index.md` or overview page for sections:
```yaml
- User Guide:
  - Overview: docs/user-guide/index.md  # ✅ Helps orient users
  - Dashboard: docs/...
```

### 5. Logical Grouping
```yaml
# ✅ Good - Logical progression
- Getting Started  # New users start here
- User Guide       # Learn features
- API Reference    # Technical details
- Advanced         # Power users

# ❌ Bad - Random order
- Advanced
- Getting Started
- API Reference
- User Guide
```

---

## Common Patterns by Doc Type

### Documentation Site
```yaml
nav:
  - Home: docs/README.md
  - Getting Started: [...]
  - Guides: [...]
  - API Reference: [...]
  - Resources: [...]
```

### API Documentation
```yaml
nav:
  - Overview: docs/README.md
  - Authentication: [...]
  - Endpoints:
    - Users: [...]
    - Accounts: [...]
  - SDKs: [...]
  - Webhooks: [...]
```

### Product Documentation
```yaml
nav:
  - Welcome: docs/README.md
  - Setup: [...]
  - Features:
    - Feature A: [...]
    - Feature B: [...]
  - Use Cases: [...]
  - Support: [...]
```

### Developer Portal
```yaml
nav:
  - Overview: docs/README.md
  - Quickstart: [...]
  - Guides: [...]
  - API Reference: [...]
  - Tools:
    - CLI: [...]
    - SDKs: [...]
  - Community: [...]
```

---

## Testing Your Navigation

### 1. Check for Broken Links
```bash
# Install yamllint
pip install yamllint

# Validate structure
yamllint nav.yml

# Check all file paths exist
find . -name "*.md" > existing-files.txt
grep -oP '(?<=: docs/).*\.md' nav.yml | while read path; do
  if [ ! -f "docs/${path#docs/}" ]; then
    echo "Missing: $path"
  fi
done
```

### 2. Measure Depth
```bash
# Find maximum nesting depth
# Counts leading spaces in nav.yml
awk '{match($0, /^ */); print RLENGTH}' nav.yml | sort -n | tail -1
```

### 3. Validate YAML
```bash
# Check for syntax errors
python -c "import yaml; yaml.safe_load(open('nav.yml'))"
```

---

## Edge Cases to Handle in Frontend

### 1. Empty Sections
```yaml
- Empty Section:
  # No children
```
**Handle by:** Show placeholder or hide section

### 2. Circular References
```yaml
- A:
  - B: docs/b.md
- B:
  - A: docs/a.md  # Circular!
```
**Handle by:** Validate during build, prevent rendering

### 3. External Links
```yaml
- External:
  - GitHub: https://github.com/...
```
**Handle by:** Open in new tab, add external icon

### 4. Special Characters
```yaml
- "Section with: Special/Characters": docs/...
```
**Handle by:** Quote strings, encode URLs properly

### 5. Long Labels
```yaml
- "This is a very long navigation label that might break layout": docs/...
```
**Handle by:** Truncate with ellipsis, show full in tooltip

---

## Migration Path

If you're starting simple and want to expand:

**Phase 1: Basic Structure**
```yaml
nav:
  - Home: docs/README.md
  - Getting Started: [...]
  - User Guide: [...]
```

**Phase 2: Add Depth**
```yaml
nav:
  - Home: docs/README.md
  - Getting Started:
    - Introduction: [...]
    - Installation: [...]
  - User Guide:
    - Dashboard: [...]
    - AI Assistant: [...]
```

**Phase 3: Add Nested Sections**
```yaml
nav:
  - Home: docs/README.md
  - Getting Started: [...]
  - User Guide:
    - Dashboard:
      - Overview: [...]
      - Settings: [...]
```

---

## Conclusion

The `nav.yml` file demonstrates **12 variations** covering:
- Simple links to complex hierarchies
- 1 level to 5+ levels of nesting
- Mixed structures and edge cases
- Real-world patterns for different doc types

Use these patterns as a reference when building your documentation structure!
