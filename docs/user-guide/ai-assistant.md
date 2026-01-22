---
title: AI Assistant Guide
description: Master the Scopien AI Assistant for maximum productivity
---

# AI Assistant Guide

The Scopien AI Assistant is your intelligent companion for all Salesforce tasks. This guide will help you unlock its full potential.

## Accessing the AI Assistant

### Quick Access
- **Keyboard**: `Ctrl/Cmd + K`
- **Button**: Click  AI Assistant in sidebar
- **Voice**: Say "Hey Scopien" (if voice enabled)
- **Mobile**: Tap microphone icon

### Interface Overview

```
┌──────────────────────────────────────────┐
│   AI Assistant                    Settings │
├──────────────────────────────────────────┤
│                                          │
│   How can I help you today?           │
│                                          │
│  Suggested actions:                      │
│  • Show my pipeline                      │
│  • Create a new lead                     │
│  • Find overdue tasks                    │
│                                          │
├──────────────────────────────────────────┤
│  Type your message here...            │
└──────────────────────────────────────────┘
```

## Natural Language Commands

### Basic Queries

**View Data**
```
Show me all opportunities
List contacts at Microsoft
Find cases opened today
Display accounts in California
```

**Create Records**
```
Create a lead for John at Apple
Add new opportunity worth $50k
Make a task to call Sarah tomorrow
Log a meeting with the sales team
```

**Update Records**
```
Change opportunity ABC to closed won
Update John's email to john@example.com
Set case #12345 to high priority
Mark task "Follow up" as completed
```

**Delete Records**
```
Delete lead "Old Lead"
Remove opportunity XYZ
Archive closed cases from last year
```

### Advanced Queries

**Filtering & Conditions**
```
Show me opportunities over $100k closing this quarter
that are in negotiation stage and owned by Sarah
```

**Aggregations**
```
What's the total value of all open opportunities?
Count how many leads we got last month
Calculate average deal size by industry
```

**Comparisons**
```
Compare Q1 revenue to Q4
Show me accounts that haven't been contacted in 30 days
Find opportunities that closed faster than average
```

**Complex Operations**
```
Create a campaign for all leads from last week's webinar,
send them a welcome email, and assign them to John
```

## Command Categories

### Data Retrieval

| Command | Example |
|---------|---------|
| Show/Display | "Show me top accounts" |
| List | "List all contacts in New York" |
| Find | "Find cases with subject containing 'billing'" |
| Get | "Get details for opportunity ABC" |
| Search | "Search for leads from webinars" |

### Data Manipulation

| Command | Example |
|---------|---------|
| Create/Add | "Create a new account for XYZ Corp" |
| Update/Change | "Update lead status to qualified" |
| Delete/Remove | "Delete old test data" |
| Convert | "Convert lead John to opportunity" |
| Merge | "Merge duplicate accounts" |

### Analytics

| Command | Example |
|---------|---------|
| Calculate | "Calculate win rate by product" |
| Analyze | "Analyze pipeline trends" |
| Compare | "Compare this month to last month" |
| Forecast | "Forecast next quarter revenue" |
| Summarize | "Summarize account activity" |

### Automation

| Command | Example |
|---------|---------|
| Schedule | "Schedule weekly pipeline report" |
| Remind | "Remind me to follow up in 3 days" |
| Notify | "Notify me when deal closes" |
| Automate | "Automate lead assignment" |

## AI Features

### Context Awareness

The AI remembers your conversation:

```
You: Show me opportunities closing this month
AI: [Shows 15 opportunities]

You: Filter for tech companies only
AI: [Shows 8 opportunities from tech companies]

You: Sort by value
AI: [Sorts the filtered results]

You: Export to CSV
AI: [Exports the sorted, filtered results]
```

### Smart Suggestions

As you type, the AI suggests:
- Common queries
- Field names
- Record names
- Filters
- Actions

### Multi-step Operations

Break complex tasks into conversations:

```
You: I need to prepare for my meeting with Acme Corp
AI: I'll help you prepare. Here's what I found:
    • Account details
    • Recent activity
    • Open opportunities
    • Support cases
    What else do you need?

You: Show me their contact history
AI: [Displays email, calls, meetings]
    Would you like me to create a meeting summary?

You: Yes, and add a follow-up task
AI: Meeting summary created
    Follow-up task added for next Tuesday
```

### Error Handling

The AI helps when things go wrong:

```
You: Show me all opps
AI: Did you mean "opportunities"?
    Here are your opportunities...

You: Create a led
AI: I think you meant "lead".
    What information should I include?
```

## Voice Commands

### Enabling Voice

1. Go to Settings > AI Assistant
2. Enable "Voice Commands"
3. Grant microphone permissions
4. Choose wake word: "Hey Scopien"

### Voice Examples

**Hands-free Operations**
```
"Hey Scopien, show my calendar for today"
"Hey Scopien, create a reminder for 2 PM"
"Hey Scopien, what's my pipeline value"
```

**Dictation**
```
"Hey Scopien, create a note: Met with client,
discussed pricing, send proposal by Friday"
```

## Advanced Techniques

### Using Variables

Save values for reuse:

```
You: Set @target to 50000
AI: Variable @target set to 50000

You: Show opportunities over @target
AI: [Shows opportunities > $50,000]

You: Calculate pipeline above @target
AI: Pipeline over $50,000: $2.3M (23 deals)
```

### Batch Operations

Process multiple records:

```
You: For each lead from yesterday's webinar:
     - Set status to "Contacted"
     - Assign to John
     - Send welcome email

AI: Processing 45 leads...
    45 leads updated
    45 assignments made
    45 emails sent
```

### Conditional Logic

Add intelligence to commands:

```
You: If opportunity value > 100k, notify sales director
     Otherwise, assign to team lead

AI: Rule created. Monitoring all opportunities.
```

### Scheduled Commands

Automate recurring tasks:

```
You: Every Monday at 9 AM, send me a pipeline report
     with opportunities closing this month

AI: Schedule created
    Next run: Monday, Jan 27 at 9:00 AM
```

## Personalization

### Training the AI

The AI learns your preferences:

**Industry Terms**
```
You: In our company, "hot lead" means score over 80
AI: Got it. I'll remember that "hot lead" = score > 80
```

**Custom Shortcuts**
```
You: When I say "my pipeline", show opportunities
     I own that are open

AI: Shortcut created: "my pipeline"
```

### Saved Queries

Bookmark frequent queries:

```
You: Save this as "Weekly Review"
AI: Saved as "Weekly Review"
    Access with: /weekly-review
```

Use saved queries:
```
You: /weekly-review
AI: [Runs your saved query]
```

## Collaboration

### Sharing Conversations

Share AI sessions with teammates:

```
You: Share this conversation with Sarah
AI: Shared with sarah@company.com
    She can view and continue this conversation
```

### Team Queries

Query across team data:

```
You: Show my team's total pipeline
AI: Team Pipeline: $5.2M (127 opportunities)

    Top performers:
    • Sarah: $1.8M (35 deals)
    • John: $1.5M (42 deals)
    • Mike: $1.2M (28 deals)
```

### @Mentions

Involve team members:

```
You: @Sarah can you follow up on the Acme Corp deal?
AI: Notification sent to Sarah
    Task created: "Follow up on Acme Corp deal"
```

## Best Practices

### Do's

**Be Specific**
```
Bad: "Show me leads"
Good: "Show me uncontacted leads from last week worth over $10k"
```

**Use Filters**
```
Bad: "Show all opportunities" (could be thousands)
Good: "Show opportunities closing this quarter in tech industry"
```

**Break Down Complex Tasks**
```
Bad: "Do everything for new lead John at Apple interested
     in enterprise plan with $100k budget meeting next week"
Good: "Create lead for John at Apple"
      "Set product interest to Enterprise"
      "Add budget $100k"
      "Schedule follow-up meeting next week"
```

### Don'ts

**Avoid Ambiguity**
```
Bad: "Show me the data"
Good: "Show me opportunity data for Q1"
```

**Don't Skip Context**
```
Bad: "Update it to qualified"
Good: "Update lead John Smith to qualified"
```

**Don't Ignore Confirmations**
```
Bad: Requesting deletion without review
Good: Review what will be deleted, then confirm
```

## Troubleshooting

### AI Not Understanding

**Problem**: AI doesn't understand your query

**Solutions:**
- Rephrase using simpler language
- Break into smaller requests
- Use Salesforce object names
- Check for typos
- Try voice input

### Incorrect Results

**Problem**: AI returns wrong data

**Solutions:**
- Add more specific filters
- Verify field names
- Check date formats
- Review permissions
- Provide feedback to improve

### Slow Responses

**Problem**: AI takes long to respond

**Solutions:**
- Simplify query
- Add filters to reduce data
- Check internet connection
- Use during off-peak hours

## Pro Tips

### Tip 1: Natural Language

Talk to the AI naturally:
```
"Hey, can you show me which opportunities are at risk?"
"I need to find all the leads we got from the website"
```

### Tip 2: Ask for Help

```
You: How do I create a report?
AI: I can help! Here's how to create a report:
    1. Tell me what data you want
    2. Specify any filters
    3. Choose how to group it

    For example: "Create a report of opportunities
    by stage for this quarter"
```

### Tip 3: Iterate

Refine results through conversation:
```
You: Show opportunities
AI: [Shows all opportunities]

You: Only tech companies
AI: [Filters to tech]

You: Over $50k
AI: [Adds value filter]

You: Closing this month
AI: [Adds date filter]
```

## Next Steps

- [Learn Dashboard Customization →](dashboard.md)
- [Master Salesforce Integration →](salesforce-integration.md)
- [Explore Advanced Workflows →](../advanced/custom-workflows.md)

---

**Remember**: The AI Assistant learns from your usage. The more you use it, the better it gets at understanding your needs!
