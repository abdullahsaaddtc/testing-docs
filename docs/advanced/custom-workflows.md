---
title: Custom Workflows
description: Create custom automation workflows in Scopien AI
---

# Custom Workflows

Learn how to create powerful automation workflows that streamline your Salesforce operations.

## Overview

Custom workflows allow you to automate repetitive tasks and create intelligent business processes.

**Example workflow:**
```
When opportunity stage changes to "Closed Won"
→ Create renewal opportunity
→ Send thank you email
→ Notify account manager
→ Update forecast
→ Add to customer success queue
```

## Creating Workflows

### Via Dashboard

1. Navigate to **Automations → Workflows**
2. Click **"Create Workflow"**
3. Configure trigger
4. Add actions
5. Set conditions
6. Test and activate

### Workflow Builder

```
┌─────────────────────────────────────────┐
│  Workflow Builder                       │
├─────────────────────────────────────────┤
│                                         │
│   Trigger: Opportunity Updated        │
│      └─ When: Stage changes             │
│                                         │
│   Conditions                          │
│      └─ Amount > $50,000                │
│      └─ Industry = "Technology"         │
│                                         │
│   Actions                             │
│      1. Send notification               │
│      2. Create task                     │
│      3. Update field                    │
│                                         │
│  [ Test Workflow ]  [ Save & Activate ] │
└─────────────────────────────────────────┘
```

## Workflow Components

### 1. Triggers

Events that start the workflow:

**Record Events**
```
• Record created
• Record updated
• Record deleted
• Field changed
```

**Time-Based**
```
• Scheduled (daily, weekly, monthly)
• After specific time
• Before specific date
```

**Custom Events**
```
• API webhook received
• Manual trigger
• External system event
```

**Example Triggers:**
```json
{
  "trigger": {
    "type": "record.updated",
    "object": "opportunity",
    "conditions": {
      "field": "stage",
      "operator": "changed",
      "value": "Closed Won"
    }
  }
}
```

### 2. Conditions

Filter when actions run:

**Field Conditions**
```
• Field equals value
• Field contains text
• Field greater than
• Field is empty
• Field changed
```

**Logic Operators**
```
• AND (all conditions must match)
• OR (any condition matches)
• NOT (condition must not match)
```

**Example Conditions:**
```json
{
  "conditions": {
    "operator": "AND",
    "rules": [
      {
        "field": "amount",
        "operator": "greater_than",
        "value": 50000
      },
      {
        "field": "industry",
        "operator": "equals",
        "value": "Technology"
      }
    ]
  }
}
```

### 3. Actions

Operations to perform:

**Data Actions**
```
• Create record
• Update record
• Delete record
• Clone record
```

**Communication**
```
• Send email
• Send SMS
• Post to Chatter
• Webhook notification
```

**Tasks & Events**
```
• Create task
• Schedule meeting
• Add to calendar
```

**External Integrations**
```
• Call API
• Run webhook
• Trigger Zapier
```

## Example Workflows

### Lead Nurturing Workflow

```javascript
{
  name: "New Lead Nurture",
  trigger: {
    type: "record.created",
    object: "lead"
  },
  actions: [
    {
      type: "send_email",
      template: "welcome_email",
      to: "{{lead.email}}"
    },
    {
      type: "create_task",
      subject: "Follow up with {{lead.name}}",
      assignTo: "{{lead.owner}}",
      dueDate: "+2 days"
    },
    {
      type: "update_record",
      field: "status",
      value: "Contacted"
    }
  ]
}
```

### Opportunity Win Workflow

```javascript
{
  name: "Deal Won Process",
  trigger: {
    type: "field.changed",
    object: "opportunity",
    field: "stage",
    newValue: "Closed Won"
  },
  conditions: {
    "amount": { "gt": 50000 }
  },
  actions: [
    {
      type: "send_email",
      to: "{{opportunity.owner.email}}",
      subject: "Congratulations on winning {{opportunity.name}}!",
      template: "deal_won"
    },
    {
      type: "create_opportunity",
      name: "{{opportunity.account.name}} - Renewal",
      amount: "{{opportunity.amount}}",
      closeDate: "+365 days",
      stage: "Prospecting"
    },
    {
      type: "webhook",
      url: "https://slack.com/api/chat.postMessage",
      payload: {
        text: " {{opportunity.owner.name}} closed {{opportunity.name}} for ${{opportunity.amount}}"
      }
    }
  ]
}
```

### High-Value Alert Workflow

```javascript
{
  name: "High-Value Opportunity Alert",
  trigger: {
    type: "record.created",
    object: "opportunity"
  },
  conditions: {
    operator: "AND",
    rules: [
      { field: "amount", operator: "gte", value: 100000 },
      { field: "stage", operator: "in", value: ["Prospecting", "Qualification"] }
    ]
  },
  actions: [
    {
      type: "notify",
      users: ["sales_manager", "vp_sales"],
      message: "High-value opportunity: {{opportunity.name}} - ${{opportunity.amount}}"
    },
    {
      type: "create_task",
      subject: "Review high-value opportunity",
      assignTo: "sales_manager",
      priority: "High",
      dueDate: "+1 day"
    }
  ]
}
```

### Case Escalation Workflow

```javascript
{
  name: "Case Escalation",
  trigger: {
    type: "scheduled",
    schedule: "0 */4 * * *"  // Every 4 hours
  },
  actions: [
    {
      type: "query",
      object: "case",
      filters: {
        status: "Open",
        priority: "High",
        createdDate: { lt: "-24 hours" }
      },
      forEach: [
        {
          type: "update_record",
          field: "status",
          value: "Escalated"
        },
        {
          type: "notify",
          users: ["support_manager"],
          message: "Case {{case.caseNumber}} escalated - no resolution in 24h"
        }
      ]
    }
  ]
}
```

## Advanced Features

### Variables & Expressions

Use dynamic values in workflows:

**Field References**
```
{{record.fieldName}}
{{record.owner.name}}
{{record.account.industry}}
```

**Date Operations**
```
{{record.createdDate | addDays(7)}}
{{now | format('YYYY-MM-DD')}}
{{record.closeDate | daysUntil}}
```

**String Operations**
```
{{record.name | uppercase}}
{{record.email | split('@')[1]}}
{{record.description | truncate(100)}}
```

**Math Operations**
```
{{record.amount * 0.1}}
{{record.quantity + 5}}
{{record.price | round(2)}}
```

### Branching Logic

Create conditional paths:

```javascript
{
  type: "branch",
  conditions: [
    {
      if: { field: "amount", gt: 100000 },
      then: [
        { type: "notify", users: ["vp_sales"] }
      ]
    },
    {
      elseif: { field: "amount", gt: 50000 },
      then: [
        { type: "notify", users: ["sales_manager"] }
      ]
    },
    {
      else: [
        { type: "assign", to: "sales_team" }
      ]
    }
  ]
}
```

### Loops

Iterate over records:

```javascript
{
  type: "query",
  object: "contact",
  filters: { accountId: "{{opportunity.accountId}}" },
  forEach: [
    {
      type: "send_email",
      to: "{{contact.email}}",
      template: "deal_announcement"
    }
  ]
}
```

### Error Handling

Handle failures gracefully:

```javascript
{
  actions: [
    {
      type: "send_email",
      to: "{{lead.email}}",
      onError: {
        type: "notify",
        users: ["admin"],
        message: "Failed to send email to {{lead.email}}"
      }
    }
  ]
}
```

### Delays & Scheduling

Add timing controls:

```javascript
{
  actions: [
    {
      type: "send_email",
      template: "immediate_response"
    },
    {
      type: "wait",
      duration: "2 days"
    },
    {
      type: "send_email",
      template: "follow_up"
    },
    {
      type: "wait",
      duration: "1 week"
    },
    {
      type: "send_email",
      template: "final_reminder"
    }
  ]
}
```

## Testing Workflows

### Test Mode

Test workflows without affecting data:

```javascript
{
  mode: "test",
  dryRun: true,
  mockData: {
    opportunity: {
      id: "opp_test123",
      name: "Test Opportunity",
      amount: 75000,
      stage: "Closed Won"
    }
  }
}
```

**Test Results:**
```
 Workflow Test Results
──────────────────────────
Trigger: Matched
Conditions: Passed
Actions:
  1.  Email sent (test mode)
  2.  Task created (test mode)
  3.  Record updated (test mode)

Execution time: 234ms
```

### Debug Mode

View detailed execution logs:

```
Workflow Execution Log
──────────────────────────────────────
10:30:00.123 - Trigger fired
10:30:00.145 - Conditions evaluated: PASS
10:30:00.167 - Action 1: send_email
               → Template loaded
               → Recipients: john@example.com
               → Status: SUCCESS
10:30:00.234 - Action 2: create_task
               → Task ID: task_abc123
               → Status: SUCCESS
10:30:00.289 - Workflow completed
```

## Best Practices

###  Do's

**Keep It Simple**
```
 One workflow per business process
 Clear naming conventions
 Document workflow purpose
```

**Error Handling**
```javascript
//  Handle errors gracefully
{
  type: "send_email",
  retries: 3,
  retryDelay: "5 minutes",
  onError: {
    type: "log",
    level: "error"
  }
}
```

**Performance**
```
 Use bulk actions for multiple records
 Limit queries in loops
 Set reasonable delays
 Monitor execution times
```

###  Don'ts

**Avoid Complexity**
```
 Too many nested conditions
 Infinite loops
 Excessive API calls
 Circular workflow triggers
```

**Don't Over-Automate**
```
 Automating one-time tasks
 Complex manual processes
 Processes requiring human judgment
```

## Monitoring

### Workflow Dashboard

```
Active Workflows: 23
──────────────────────────────────────
Lead Nurture          145 runs/day
Deal Won Process      12 runs/day
Case Escalation       48 runs/day
High-Value Alert     Warning 3 errors today
```

### Execution History

```
Recent Executions
──────────────────────────────────────
10:45 AM - Lead Nurture
            Success (234ms)

10:30 AM - Deal Won Process
            Success (1.2s)

10:15 AM - High-Value Alert
            Failed: Email delivery error
           Retry: In progress
```

### Performance Metrics

```
Workflow Performance
──────────────────────────────────────
Avg Execution Time:  450ms
Success Rate:        98.5%
Total Runs (30d):    4,234
Actions Performed:   12,702
```

## Troubleshooting

### Workflow Not Running

**Checklist:**
- [ ] Workflow is active
- [ ] Trigger conditions met
- [ ] User has permissions
- [ ] No conflicting workflows
- [ ] Check error logs

### Actions Not Executing

**Debug steps:**
1. Enable debug mode
2. Check conditions
3. Verify field references
4. Test with mock data
5. Review execution logs

### Performance Issues

**Optimizations:**
```
 Use bulk operations
 Reduce API calls
 Optimize queries
 Add appropriate delays
 Use async where possible
```

## Integration Examples

### Slack Notification

```javascript
{
  type: "webhook",
  url: "https://hooks.slack.com/services/YOUR/WEBHOOK/URL",
  method: "POST",
  payload: {
    text: "New high-value opportunity: {{opportunity.name}}",
    blocks: [
      {
        type: "section",
        text: {
          type: "mrkdwn",
          text: "*{{opportunity.name}}*\nAmount: ${{opportunity.amount}}\nOwner: {{opportunity.owner.name}}"
        }
      }
    ]
  }
}
```

### Zapier Integration

```javascript
{
  type: "webhook",
  url: "https://hooks.zapier.com/hooks/catch/YOUR_HOOK_ID",
  payload: {
    event: "opportunity_won",
    data: {
      name: "{{opportunity.name}}",
      amount: "{{opportunity.amount}}",
      closeDate: "{{opportunity.closeDate}}"
    }
  }
}
```

## Next Steps

- [Learn about Security Best Practices →](security.md)
- [Explore API Integration →](../api-reference/authentication.md)
- [View Troubleshooting Guide →](troubleshooting.md)

---

**Need workflow help?** Contact automation@scopien.com or join our [Community Forum](https://community.scopien.com/workflows)
