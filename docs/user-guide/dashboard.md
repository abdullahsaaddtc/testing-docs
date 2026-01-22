---
title: Dashboard Overview
description: Navigate and customize your Scopien AI dashboard
---

# Dashboard Overview

Your Scopien AI dashboard is your command center for all Salesforce operations. This guide will help you navigate and customize it to fit your workflow.

## Dashboard Layout

```
┌──────────────────────────────────────────────────────────────┐
│  Scopien AI             Search       Alerts       Profile    │
├──────────────────────────────────────────────────────────────┤
│  Sidebar    │  Main Content Area                             │
│  ─────────  │  ───────────────────────────────────           │
│  Home       │  ┌─────────────┐  ┌─────────────┐             │
│  AI Chat    │  │  Revenue    │  │  Pipeline   │             │
│  Reports    │  │  $2.5M      │  │  45 Deals   │             │
│  Teams      │  └─────────────┘  └─────────────┘             │
│  Settings   │                                                │
│             │  Recent Activity                               │
│             │  • Lead created at 10:23 AM                    │
│             │  • Opportunity updated at 9:45 AM              │
└──────────────────────────────────────────────────────────────┘
```

## Key Sections

### 1. Top Navigation Bar

**Search Bar** (`Ctrl/Cmd + K`)
- Search across all Salesforce records
- Natural language queries
- Recent searches dropdown
- Quick filters

**Notifications**
- Real-time alerts
- Task reminders
- System updates
- Team mentions

**Profile Menu**
- Account settings
- Preferences
- Help & support
- Sign out

### 2. Sidebar Navigation

**Home Dashboard**
- Overview of key metrics
- Quick access widgets
- Recent activity feed

**AI Assistant**
- Natural language interface
- Command history
- Saved queries
- Smart suggestions

**Reports**
- Pre-built reports
- Custom reports
- Scheduled reports
- Export options

**Teams**
- Team members
- Performance metrics
- Collaboration tools

**Settings**
- Account configuration
- Integration settings
- Automation rules
- User management

### 3. Main Content Area

The main area adapts based on your current view:

**Widgets**
Customizable cards showing:
- Revenue metrics
- Pipeline health
- Activity timeline
- Top accounts
- Team performance
- Forecasts

**Activity Feed**
Real-time updates on:
- Record changes
- Team actions
- System events
- Automated tasks

## Customizing Your Dashboard

### Adding Widgets

1. Click "Customize" button (top right)
2. Browse available widgets
3. Drag widget to desired position
4. Configure widget settings
5. Click "Save Layout"

Available widgets:

| Widget | Description | Size |
|--------|-------------|------|
| Revenue Chart | Visual revenue trends | Medium |
| Pipeline Funnel | Deal stage breakdown | Large |
| Activity Stream | Recent actions | Small |
| Top Accounts | Highest value accounts | Medium |
| Task List | Upcoming tasks | Small |
| Team Performance | Team metrics | Large |
| Quick Stats | Key numbers | Small |
| Forecast | Revenue predictions | Medium |

### Arranging Layouts

**Grid System**
- Dashboards use a 12-column grid
- Widgets snap to grid
- Responsive on all devices

**Layout Presets**
Choose from templates:
- **Executive**: High-level metrics
- **Sales Manager**: Team and pipeline focus
- **Sales Rep**: Personal performance
- **Service Agent**: Case management
- **Custom**: Build your own

### Filtering Data

Apply filters to focus on what matters:

```
Time Range:
○ Today
○ This Week
● This Month
○ This Quarter
○ Custom Range

Team:
☑ My Records
☐ My Team
☐ All Records

Status:
☑ Active
☐ Archived
☐ All
```

## Widgets Deep Dive

### Revenue Chart Widget

Track revenue over time with multiple views:

```javascript
// Configure options
{
  timeRange: "last_90_days",
  groupBy: "week",
  metrics: ["actual", "forecast"],
  displayType: "line"  // or "bar", "area"
}
```

**Features:**
- Compare actual vs forecast
- Multiple time ranges
- Export to CSV/PDF
- Drill-down capability

### Pipeline Funnel Widget

Visualize your sales pipeline:

```
┌────────────────────────────┐
│ Prospecting    [45] $2.5M  │ ░░░░░░░░░░░░░░░░░░░░
│ Qualification  [32] $1.8M  │ ░░░░░░░░░░░░░░
│ Proposal       [18] $1.2M  │ ░░░░░░░░░
│ Negotiation    [12] $0.9M  │ ░░░░░░
│ Closed Won     [8]  $0.6M  │ ░░░░
└────────────────────────────┘
```

**Insights:**
- Conversion rates between stages
- Average deal size per stage
- Time in stage analysis
- Bottleneck identification

### Activity Stream Widget

Stay updated on recent changes:

```
10:45 AM | 📝 Sarah created Opportunity "Acme Corp"
10:23 AM | 📞 John logged call with Enterprise Inc
09:56 AM | ✉️ Lead "Jane Doe" converted to Contact
09:30 AM | 📊 Report "Q1 Pipeline" generated
```

**Filters:**
- Record type
- Team member
- Action type
- Time range

### Top Accounts Widget

Monitor your most important accounts:

| Account | Revenue | Health | Next Action |
|---------|---------|--------|-------------|
| Acme Corp | $500K | Excellent | Renewal call next week |
| TechStart | $350K | Good | Upsell opportunity |
| Global Inc | $280K | Good | Quarterly review due |
| Innovation | $250K | At Risk | Urgent: Address concerns |

**Health Score** based on:
- Engagement frequency
- Support cases
- Payment history
- Product usage

## Quick Actions

Access common tasks quickly:

**Create New** (+ button)
- Lead
- Opportunity
- Account
- Contact
- Case
- Task

**Bulk Actions**
Select multiple records and:
- Update fields
- Assign owner
- Change status
- Add to campaign
- Export data

## Dashboard Shortcuts

Master these shortcuts for efficiency:

| Shortcut | Action |
|----------|--------|
| `Ctrl/Cmd + K` | Open command palette |
| `Ctrl/Cmd + N` | Create new record |
| `Ctrl/Cmd + S` | Global search |
| `Ctrl/Cmd + ,` | Open settings |
| `G` then `H` | Go to home |
| `G` then `R` | Go to reports |
| `?` | Show all shortcuts |

## Mobile Dashboard

Access your dashboard on mobile:

**Optimized Views**
- Single column layout
- Swipe gestures
- Touch-optimized controls
- Offline mode

**Mobile Features**
- Voice commands
- Camera integration
- GPS check-ins
- Push notifications

## Performance Tips

### Optimize Load Times

1. **Limit Widgets**: Keep 4-6 widgets max
2. **Reduce Date Range**: Shorter ranges load faster
3. **Cache Results**: Enable caching in settings
4. **Archive Old Data**: Remove unnecessary records

### Best Practices

**Do:**
- Arrange most-used widgets at top
- Use filters to reduce data
- Enable auto-refresh for live data
- Customize per role/function

**Don't:**
- Overload with widgets
- Use all-time date ranges
- Ignore performance warnings
- Share sensitive dashboards

## Troubleshooting

### Widgets Not Loading

**Problem**: Widget shows "Loading..." indefinitely

**Solutions:**
1. Refresh the page
2. Check internet connection
3. Verify Salesforce API limits
4. Clear browser cache
5. Check widget configuration

### Data Not Updating

**Problem**: Dashboard shows stale data

**Solutions:**
1. Click refresh button
2. Check auto-refresh settings
3. Verify sync status
4. Review Salesforce permissions

## Next Steps

- [Master the AI Assistant →](ai-assistant.md)
- [Learn about Salesforce Integration →](salesforce-integration.md)
- [Create Custom Reports →](../advanced/custom-workflows.md)

---

**Pro Tip**: Create multiple dashboard layouts for different contexts (daily work, weekly reviews, presentations).
