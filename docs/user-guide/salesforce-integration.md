---
title: Salesforce Integration
description: Connect and manage your Salesforce integration with Scopien AI
---

# Salesforce Integration

Learn how to connect, configure, and optimize your Salesforce integration with Scopien AI.

## Connecting Salesforce

### Initial Connection

1. Navigate to **Settings → Integrations**
2. Click **"Connect Salesforce"**
3. Choose environment:
   - Production
   - Sandbox
4. Click **"Authorize"**

You'll be redirected to Salesforce login:

```
┌────────────────────────────────────┐
│  Salesforce Login                  │
├────────────────────────────────────┤
│  Username: user@company.com        │
│  Password: ••••••••••              │
│                                    │
│  [ Log In ]                        │
│                                    │
│  ⚠️  Scopien AI is requesting      │
│  access to your Salesforce data    │
└────────────────────────────────────┘
```

### Permissions Required

Scopien AI requests these permissions:

| Permission | Purpose |
|------------|---------|
| Read Data | View all Salesforce records |
| Write Data | Create and update records |
| API Access | Communicate with Salesforce |
| Chatter Access | Post updates and notifications |
| Reporting | Generate custom reports |

✅ **Safe**: All permissions use OAuth 2.0 secure authentication

### Verification

After authorization, verify the connection:

```
✅ Connected to Salesforce
   Org: Acme Corporation
   Edition: Enterprise
   User: john.doe@acme.com
   Status: Active
   Last Sync: Just now
```

## Configuration

### Object Selection

Choose which Salesforce objects to sync:

**Standard Objects**
- [x] Accounts
- [x] Contacts
- [x] Leads
- [x] Opportunities
- [x] Cases
- [x] Tasks
- [x] Events
- [ ] Campaigns
- [ ] Products

**Custom Objects**
- [x] Project__c
- [x] Invoice__c
- [ ] CustomObject__c

💡 **Tip**: Only sync objects you actively use to improve performance

### Field Mapping

Map Salesforce fields to Scopien AI:

```
Salesforce Field          →  Scopien Field
──────────────────────────────────────────
Account.Name              →  Company Name
Account.AnnualRevenue     →  Revenue
Opportunity.Amount        →  Deal Value
Lead.Company              →  Company
Contact.Email             →  Email Address
```

**Custom Mappings**:
```
Custom_Field__c           →  Custom Property
External_ID__c            →  External Reference
Score__c                  →  Lead Score
```

### Sync Settings

Configure how data syncs:

**Frequency**
```
○ Real-time (recommended)
○ Every 15 minutes
○ Every hour
○ Manual only
```

**Direction**
```
☑ Salesforce → Scopien (two-way sync)
☑ Scopien → Salesforce
```

**Conflict Resolution**
```
When conflicts occur:
● Salesforce wins
○ Scopien wins
○ Manual review
```

## Data Management

### Initial Sync

First sync imports all historical data:

```
Syncing Salesforce Data
━━━━━━━━━━━━━━━━━━ 78% (45,231 / 58,000 records)

Accounts:      ✅ 3,456
Contacts:      ✅ 12,345
Opportunities: ✅ 8,234
Cases:         ⏳ 15,678
Leads:         ⏳ 18,287

Estimated time: 3 minutes
```

**Performance Tips**:
- Run during off-peak hours
- Archive old data first
- Use incremental sync after initial

### Incremental Sync

After initial sync, only changes sync:

```
Last Sync: 2 minutes ago
Changes synced: 47 records

• 23 records updated
• 15 records created
• 9 records deleted
```

### Manual Sync

Force a sync anytime:

1. Go to **Settings → Integrations**
2. Click **"Sync Now"**
3. Wait for completion

## Advanced Features

### Multi-Org Support

Connect multiple Salesforce orgs:

```
Connected Organizations
───────────────────────────────────────
🏢 Production Org
   • acme.my.salesforce.com
   • Status: Active
   • Last sync: 2 min ago

🧪 Sandbox Org
   • acme--sandbox.my.salesforce.com
   • Status: Active
   • Last sync: 5 min ago

+ Add Another Org
```

Switch between orgs:
```
You: Show opportunities in production org
AI: [Shows production data]

You: Switch to sandbox
AI: Switched to sandbox org

You: Create test lead
AI: [Creates in sandbox]
```

### Custom API Integration

For developers:

```javascript
// Connect to Salesforce via Scopien API
const scopien = require('@scopien/sdk');

const client = new scopien.Client({
  apiKey: 'your-api-key',
  salesforce: {
    instanceUrl: 'https://acme.my.salesforce.com',
    accessToken: 'your-access-token'
  }
});

// Query Salesforce through Scopien
const opportunities = await client.query(
  'SELECT Id, Name, Amount FROM Opportunity WHERE StageName = "Closed Won"'
);
```

### Webhook Integration

Set up real-time webhooks:

```json
{
  "event": "opportunity.updated",
  "webhook_url": "https://your-app.com/webhook",
  "filters": {
    "stage": "Closed Won",
    "amount_greater_than": 50000
  }
}
```

Receive notifications:
```json
{
  "event": "opportunity.updated",
  "timestamp": "2026-01-22T10:30:00Z",
  "data": {
    "id": "006XX000001234",
    "name": "Acme Corp Deal",
    "amount": 75000,
    "stage": "Closed Won"
  }
}
```

## Security

### OAuth Authentication

Scopien uses OAuth 2.0:

```
1. User → Scopien: "Connect Salesforce"
2. Scopien → Salesforce: Request authorization
3. User → Salesforce: Login & approve
4. Salesforce → Scopien: Access token
5. Scopien → User: "Connected!"
```

**Benefits**:
- No password sharing
- Revocable access
- Limited scope
- Secure token storage

### Data Encryption

All data is encrypted:

- **In Transit**: TLS 1.3
- **At Rest**: AES-256
- **Backups**: Encrypted snapshots
- **Logs**: Sanitized data

### Compliance

Scopien AI is compliant with:
- ✅ SOC 2 Type II
- ✅ GDPR
- ✅ CCPA
- ✅ HIPAA (Enterprise plan)
- ✅ ISO 27001

### Access Control

Manage who can access Salesforce data:

**Role-Based Access**
```
Admin:    Full access to all data
Manager:  Read/write team data
User:     Read own data
Guest:    Read-only limited data
```

**Field-Level Security**
```
Hide sensitive fields:
☑ Social Security Number
☑ Bank Account Details
☑ Salary Information
☐ Email Address
```

## Monitoring

### Sync Status

Monitor sync health:

```
Sync Health: ✅ Excellent
──────────────────────────────
Success Rate:    99.8%
Avg Sync Time:   45 seconds
Records/Hour:    12,500
Errors (24h):    2
Last Error:      Rate limit exceeded (resolved)

📊 View Detailed Logs
```

### API Usage

Track API consumption:

```
Salesforce API Usage
──────────────────────────────
Daily Limit:     100,000
Used Today:      12,456 (12%)
Peak Hour:       9 AM - 2,456 calls
Remaining:       87,544

📈 View Usage Trends
```

### Error Monitoring

View and resolve errors:

```
Recent Sync Errors
──────────────────────────────
⚠️ 10:15 AM - Rate limit exceeded
   Action: Waiting 15 minutes
   Status: Resolved

❌ 09:32 AM - Invalid field: Custom__c
   Action: Field mapping updated
   Status: Resolved

⚠️ 08:45 AM - Record locked
   Action: Retry in progress
   Status: Retrying
```

## Troubleshooting

### Connection Issues

**Problem**: Cannot connect to Salesforce

**Solutions**:
1. Verify credentials
2. Check IP whitelist
3. Confirm org is active
4. Review Connected App settings
5. Check Salesforce status: [status.salesforce.com](https://status.salesforce.com)

### Sync Failures

**Problem**: Data not syncing

**Checklist**:
- [ ] Verify Salesforce connection is active
- [ ] Check API limits
- [ ] Review field permissions
- [ ] Confirm object access
- [ ] Check sync logs for errors

### Performance Issues

**Problem**: Slow sync times

**Optimizations**:
1. Reduce number of synced objects
2. Increase sync interval
3. Use selective field sync
4. Archive old records
5. Contact support for bulk optimizations

### Data Mismatches

**Problem**: Data differs between Salesforce and Scopien

**Debugging**:
```
1. Check sync status
2. Review conflict resolution settings
3. Verify field mappings
4. Check for validation rules
5. Force manual sync
6. Compare record audit trails
```

## Best Practices

### ✅ Do's

**Regular Monitoring**
- Check sync status daily
- Review error logs weekly
- Monitor API usage
- Test after Salesforce updates

**Security**
- Rotate API tokens regularly
- Use least-privilege access
- Enable MFA for Salesforce
- Audit access logs

**Performance**
- Sync only needed objects
- Use field-level sync
- Schedule large syncs off-peak
- Archive old data

### ❌ Don'ts

**Security**
- ❌ Share API credentials
- ❌ Disable security features
- ❌ Ignore security alerts
- ❌ Use admin access for apps

**Performance**
- ❌ Sync all objects
- ❌ Use real-time sync unnecessarily
- ❌ Ignore API limits
- ❌ Keep unlimited history

## Migration

### From Other Tools

Migrating from another Salesforce tool?

**Export Existing Data**
```bash
# Export from old tool
old-tool export --format=csv --output=data.csv

# Import to Scopien
scopien import --file=data.csv --map=config.json
```

**Parallel Running**
Run both systems during transition:
- Week 1-2: Parallel testing
- Week 3: Train users
- Week 4: Full migration
- Week 5: Decommission old tool

## Support

### Getting Help

**Documentation**: This guide
**Status Page**: [status.scopien.com](https://status.scopien.com)
**Support Email**: salesforce@scopien.com
**Chat**: Available in app
**Phone**: +1 (800) SCOPIEN

### Salesforce Partner

Scopien AI is a Salesforce ISV Partner:
- ✅ Certified on AppExchange
- ✅ Security reviewed
- ✅ Regular compliance audits
- ✅ Direct Salesforce support channel

## Next Steps

- [Explore Advanced Workflows →](../advanced/custom-workflows.md)
- [Learn about API Integration →](../api-reference/authentication.md)
- [Check Security Best Practices →](../advanced/security.md)

---

**Need help with integration?** Contact our Salesforce specialists at salesforce@scopien.com
