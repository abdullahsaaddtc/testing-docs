---
title: Quick Start Guide
description: Get up and running with Scopien AI in 10 minutes
---

# Quick Start Guide

Get up and running with Scopien AI in just 10 minutes! This guide will walk you through your first AI-powered Salesforce interactions.

## Step 1: Access Your Dashboard

After logging in, you'll see your main dashboard with:

```
┌─────────────────────────────────────────────────┐
│  Scopien AI Dashboard                           │
├─────────────────────────────────────────────────┤
│  📊 Quick Stats        🤖 AI Assistant          │
│  • 1,234 Records       • Ask me anything        │
│  • 56 Opportunities    • Get instant answers    │
│  • 89 Open Cases       • Automate tasks         │
└─────────────────────────────────────────────────┘
```

## Step 2: Your First AI Query

Let's start with something simple. Click the AI Assistant button or press `Ctrl+K` (Windows) / `Cmd+K` (Mac).

### Example 1: Retrieve Data

**You ask:**
```
Show me all opportunities closing this month
```

**AI responds:**
```
Found 12 opportunities closing in January 2026:

1. Acme Corp Renewal - $150,000 (90% probability)
2. TechStart Implementation - $75,000 (80% probability)
3. Global Industries Expansion - $200,000 (95% probability)
...

Total Pipeline: $1,250,000
Weighted Pipeline: $1,062,500
```

### Example 2: Create Records

**You ask:**
```
Create a new lead for John Smith at Apple Inc,
email john.smith@apple.com, interested in our Enterprise plan
```

**AI responds:**
```
✅ Lead created successfully!

Details:
• Name: John Smith
• Company: Apple Inc
• Email: john.smith@apple.com
• Status: New
• Lead Source: Web
• Product Interest: Enterprise Plan

View in Salesforce →
```

### Example 3: Generate Reports

**You ask:**
```
Create a report showing top 5 accounts by revenue this quarter
```

**AI responds with a table:**

| Account Name | Revenue | Change | Status |
|--------------|---------|--------|--------|
| Enterprise Corp | $2.5M | +15% ⬆️ | Active |
| Global Tech | $1.8M | +8% ⬆️ | Active |
| Innovation Labs | $1.2M | -3% ⬇️ | Active |
| Digital Solutions | $950K | +22% ⬆️ | Active |
| Cloud Systems | $820K | +5% ⬆️ | Active |

## Step 3: Explore Key Features

### Natural Language Commands

Scopien AI understands natural language. Try these commands:

**Data Retrieval:**
- "Find all contacts at Microsoft"
- "Show me cases opened in the last week"
- "List opportunities in the negotiation stage"

**Data Creation:**
- "Create a task to follow up with Sarah tomorrow"
- "Add a note to the Acme Corp account about our meeting"
- "Log a call with John about pricing"

**Data Updates:**
- "Change the status of opportunity XYZ to closed won"
- "Update contact email for Jane Doe"
- "Set the priority of case #12345 to high"

**Analytics:**
- "What's my team's average deal size?"
- "Show me win rate by product line"
- "Compare Q1 to Q4 revenue"

### Keyboard Shortcuts

Speed up your workflow with shortcuts:

| Shortcut | Action |
|----------|--------|
| `Ctrl/Cmd + K` | Open AI Assistant |
| `Ctrl/Cmd + /` | Show all shortcuts |
| `Ctrl/Cmd + S` | Quick search |
| `Esc` | Close modal |
| `?` | Help menu |

## Step 4: Set Up Your Workspace

### Customize Your Dashboard

1. Click the ⚙️ icon in the top right
2. Select "Customize Dashboard"
3. Add widgets:
   - Revenue Chart
   - Pipeline Funnel
   - Activity Feed
   - Team Performance
4. Drag to reorder
5. Click "Save Layout"

### Configure Notifications

Stay informed about important events:

1. Go to Settings > Notifications
2. Choose notification types:
   - [ ] New opportunities
   - [x] High-value deals
   - [x] Overdue tasks
   - [ ] Daily summary
3. Select delivery method:
   - [x] Email
   - [x] In-app
   - [ ] SMS
   - [ ] Slack

### Create Your First Automation

Automate repetitive tasks:

1. Click "Automations" in sidebar
2. Click "Create New Automation"
3. Set trigger: "When opportunity stage changes to Closed Won"
4. Add actions:
   ```
   → Create renewal opportunity
   → Send thank you email
   → Notify account manager
   → Update forecast
   ```
5. Save and activate

## Step 5: Mobile Access

Download the Scopien AI mobile app:

**iOS:** [App Store Link](https://apps.apple.com/scopien)
**Android:** [Play Store Link](https://play.google.com/scopien)

Mobile features:
- Voice commands
- Push notifications
- Offline access
- Camera for document scanning

## Common Tasks

### Task 1: Weekly Pipeline Review

```
Show me all opportunities in negotiation stage with
next steps due this week, sorted by value
```

### Task 2: Lead Follow-up

```
Find all new leads from the last 3 days that
haven't been contacted yet
```

### Task 3: Case Management

```
Show me all high-priority cases assigned to my team
that are still open
```

### Task 4: Account Analysis

```
Give me a summary of Acme Corp including revenue,
open opportunities, and recent activities
```

## Pro Tips

### 💡 Tip 1: Use Filters
```
Show me opportunities closing next month
with value over $100k in the healthcare industry
```

### 💡 Tip 2: Batch Operations
```
Update all leads from yesterday's webinar
to status "Contacted"
```

### 💡 Tip 3: Scheduled Reports
```
Send me a pipeline report every Monday at 9am
```

### 💡 Tip 4: Context Memory
The AI remembers your conversation context:
```
You: Show me top 10 opportunities
AI: [Shows results]
You: Filter for just tech companies
AI: [Updates results based on previous query]
```

## Troubleshooting

### AI Doesn't Understand

If the AI doesn't understand your request:
1. Rephrase more specifically
2. Break complex requests into steps
3. Use Salesforce terminology
4. Check the [Command Reference](../api-reference/endpoints.md)

### Slow Response

If queries are slow:
1. Check your internet connection
2. Reduce result set size with filters
3. Check Salesforce API limits
4. Contact support if persistent

## Next Steps

Now that you're familiar with the basics:

- [Deep dive into the Dashboard →](../user-guide/dashboard.md)
- [Master the AI Assistant →](../user-guide/ai-assistant.md)
- [Learn about Advanced Workflows →](../advanced/custom-workflows.md)
- [Explore API Integration →](../api-reference/authentication.md)

## Resources

- 📹 [Video Tutorials](https://scopien.com/tutorials)
- 📖 [Best Practices Guide](https://scopien.com/best-practices)
- 💬 [Community Forum](https://community.scopien.com)
- 🎓 [Certification Program](https://academy.scopien.com)

---

**Estimated Time**: 10-15 minutes
**Next**: [Dashboard Overview →](../user-guide/dashboard.md)
