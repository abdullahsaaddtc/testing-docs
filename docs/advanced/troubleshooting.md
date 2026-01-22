---
title: Troubleshooting Guide
description: Solutions to common issues and frequently asked questions
---

# Troubleshooting Guide

Find solutions to common issues and answers to frequently asked questions.

## Quick Diagnostics

### System Status Check

**Check Scopien AI Status:**
```bash
curl https://status.scopien.com/api/status
```

**Response:**
```json
{
  "status": "operational",
  "services": {
    "api": "operational",
    "dashboard": "operational",
    "salesforce_sync": "operational",
    "ai_assistant": "operational"
  }
}
```

### Connection Test

**Test your connection:**
```bash
curl https://api.scopien.com/v1/health \
  -H "Authorization: Bearer ${API_KEY}"
```

## Common Issues

### Authentication Problems

#### Cannot Log In

**Problem:** "Invalid credentials" error

**Solutions:**
1. **Reset Password**
   - Click "Forgot Password"
   - Check email for reset link
   - Create new strong password

2. **Check Account Status**
   - Account may be locked after failed attempts
   - Contact admin to unlock
   - Wait 30 minutes for automatic unlock

3. **Verify Email**
   - Ensure email is verified
   - Resend verification email
   - Check spam folder

4. **MFA Issues**
   - Sync device time (TOTP requires accurate time)
   - Use backup codes
   - Contact support to reset MFA

#### API Authentication Fails

**Problem:** 401 Unauthorized error

**Solutions:**
```javascript
// Check 1: Verify API key format
if (!apiKey.startsWith('sk_')) {
  console.error('Invalid API key format');
}

// Check 2: Verify key is active
const response = await fetch('https://api.scopien.com/v1/auth/test', {
  headers: { 'Authorization': `Bearer ${apiKey}` }
});

// Check 3: Check key permissions
const keyInfo = await response.json();
console.log('Scopes:', keyInfo.scopes);
```

### Salesforce Integration Issues

#### Sync Not Working

**Problem:** Data not syncing from Salesforce

**Diagnostic Steps:**
```
1. Check sync status
   Settings → Integrations → Salesforce
   Status should show: "Active" ✅

2. Verify last sync time
   Last Sync: < 1 hour ago ✅
   Last Sync: > 1 hour ago ⚠️

3. Check sync logs
   Settings → Logs → Sync Logs
   Look for error messages

4. Verify API limits
   Salesforce Setup → System Overview
   Check API usage
```

**Common Solutions:**

**API Limit Exceeded:**
```
Solution: Wait for API limits to reset
• Daily limits reset at midnight
• Hourly limits reset every hour
• Consider upgrading Salesforce edition
```

**Permission Issues:**
```
Solution: Verify permissions
1. Go to Salesforce Setup
2. Navigate to Connected Apps
3. Find Scopien AI
4. Check granted permissions
5. Ensure all required objects are accessible
```

**Network/Firewall:**
```
Solution: Whitelist Scopien IPs
Add these IPs to Salesforce whitelist:
• 52.91.14.224/32
• 18.210.11.89/32
• 34.228.82.133/32
```

#### Data Mismatches

**Problem:** Data differs between Salesforce and Scopien

**Debugging:**
```bash
# Compare records
# Salesforce
curl "https://[instance].salesforce.com/services/data/v58.0/sobjects/Opportunity/[id]" \
  -H "Authorization: Bearer ${SF_TOKEN}"

# Scopien
curl "https://api.scopien.com/v1/opportunities/[id]" \
  -H "Authorization: Bearer ${SCOPIEN_KEY}"

# Check sync timestamp
# The record may be mid-sync
```

**Solutions:**
1. Force manual sync
2. Check field mappings
3. Verify record permissions
4. Review conflict resolution settings

### AI Assistant Issues

#### AI Not Responding

**Problem:** AI Assistant doesn't respond

**Checklist:**
- [ ] Check internet connection
- [ ] Verify account is active
- [ ] Check rate limits
- [ ] Try refreshing page
- [ ] Clear browser cache
- [ ] Try different browser

**Browser Console Check:**
```javascript
// Open browser console (F12)
// Look for errors
console.log('WebSocket status:', wsConnection.readyState);
// 0 = Connecting, 1 = Open, 2 = Closing, 3 = Closed
```

#### AI Doesn't Understand Queries

**Problem:** AI returns "I don't understand"

**Improvement Tips:**

**Be Specific:**
```
❌ "Show me data"
✅ "Show me opportunities closing this month"

❌ "Find the thing"
✅ "Find accounts in California with revenue over $1M"
```

**Use Salesforce Terms:**
```
❌ "Show me deals"
✅ "Show me opportunities"

❌ "Find companies"
✅ "Find accounts"
```

**Break Down Complex Queries:**
```
❌ "Show me everything about accounts with opportunities
     closing this month over $50k in tech industry"

✅ Step 1: "Show me opportunities closing this month over $50k"
✅ Step 2: "Filter for tech industry"
✅ Step 3: "Show account details"
```

#### Slow AI Responses

**Problem:** AI takes long to respond

**Optimizations:**
```
1. Reduce result set
   ✅ "Show me top 10 opportunities"
   ❌ "Show me all opportunities"

2. Add date filters
   ✅ "Show opportunities from last month"
   ❌ "Show all opportunities ever"

3. Use specific filters
   ✅ "Show opportunities owned by me"
   ❌ "Show all opportunities for everyone"
```

### Performance Issues

#### Slow Dashboard Loading

**Problem:** Dashboard takes long to load

**Solutions:**

**Reduce Widgets:**
```
Optimal: 4-6 widgets
Current: 12 widgets ⚠️

Remove unused widgets:
Settings → Customize Dashboard → Remove
```

**Optimize Date Ranges:**
```
❌ All time (slow)
✅ Last 90 days (fast)
✅ This quarter (fast)
```

**Enable Caching:**
```
Settings → Performance → Cache Settings
Cache Duration: 15 minutes ✅
```

#### API Rate Limits

**Problem:** 429 Too Many Requests error

**Check Current Usage:**
```bash
curl https://api.scopien.com/v1/usage \
  -H "Authorization: Bearer ${API_KEY}"
```

**Response:**
```json
{
  "limit": 1000,
  "used": 956,
  "remaining": 44,
  "resetAt": "2026-01-22T11:00:00Z"
}
```

**Implement Backoff:**
```javascript
async function retryWithBackoff(fn, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (error.status === 429 && i < maxRetries - 1) {
        const delay = Math.pow(2, i) * 1000;  // Exponential backoff
        console.log(`Rate limited. Waiting ${delay}ms...`);
        await sleep(delay);
      } else {
        throw error;
      }
    }
  }
}
```

### Data Issues

#### Missing Records

**Problem:** Records not appearing in Scopien

**Debugging Steps:**

```
1. Check record in Salesforce
   ✅ Exists
   ❌ Deleted → Cannot sync

2. Check sync settings
   Settings → Integrations → Salesforce → Objects
   ✅ Object type is enabled

3. Check permissions
   ✅ User has read access
   ❌ No access → Grant permissions

4. Check filters
   Settings → Data Filters
   ❌ Filter excluding records → Adjust filter

5. Force sync
   Settings → Integrations → Sync Now
```

#### Duplicate Records

**Problem:** Duplicate records in Scopien

**Causes:**
1. Multiple sync sources
2. Conflict resolution settings
3. Salesforce duplicates

**Solutions:**
```
1. Merge duplicates
   Select records → Actions → Merge

2. Enable deduplication
   Settings → Data Quality → Deduplication
   ✅ Enable automatic deduplication
   Rule: Match on Email + Company

3. Fix in Salesforce
   Use Salesforce duplicate management
```

### Workflow Issues

#### Workflow Not Triggering

**Problem:** Automation workflow doesn't run

**Debug Checklist:**
```
1. Verify workflow is active
   ✅ Status: Active
   ❌ Status: Inactive → Activate

2. Check trigger conditions
   Test with sample data
   Conditions: Must ALL be met

3. Review execution logs
   Workflows → [Workflow Name] → Logs
   Look for: "Trigger conditions not met"

4. Check permissions
   User must have permission to execute actions

5. Test workflow
   Workflows → [Workflow Name] → Test
   Use known-good test data
```

**Example Debug Output:**
```
Workflow Execution Debug
────────────────────────────────
✅ Trigger: opportunity.updated
❌ Condition Failed: amount > 50000
   Actual amount: 25000
   Expected: > 50000

Solution: Opportunity didn't meet $50k threshold
```

#### Actions Not Executing

**Problem:** Workflow triggers but actions don't run

**Common Issues:**

**Email Not Sending:**
```
Check:
1. Email template exists
2. Recipient email is valid
3. Email service is not blocked
4. Check spam filters

Logs may show:
"Email bounced: Invalid address"
"Email blocked by recipient server"
```

**Record Not Creating:**
```
Check:
1. Required fields are provided
2. User has create permission
3. Validation rules pass
4. Record type is accessible

Error: "Required field missing: AccountId"
Solution: Add accountId to workflow action
```

## Frequently Asked Questions

### Account & Billing

**Q: How do I upgrade my plan?**

A: Navigate to Settings → Billing → Upgrade Plan. Choose your desired plan and follow the checkout process.

**Q: Can I cancel anytime?**

A: Yes, you can cancel anytime from Settings → Billing → Cancel Subscription. You'll have access until the end of your billing period.

**Q: Do you offer refunds?**

A: We offer a 30-day money-back guarantee for annual plans. Monthly plans are not eligible for refunds but can be canceled anytime.

### Data & Privacy

**Q: Where is my data stored?**

A: Data is stored in AWS data centers in US-East-1 (Virginia) by default. Enterprise customers can choose their region.

**Q: Is my data backed up?**

A: Yes, we perform automated backups every 6 hours with 30-day retention. Enterprise plans include point-in-time recovery.

**Q: Can I export my data?**

A: Yes, you can export all your data anytime from Settings → Data Export. Formats available: JSON, CSV, XML.

**Q: What happens if I cancel?**

A: Your data is retained for 90 days after cancellation. You can export your data or reactivate your account during this period.

### Integration

**Q: Can I connect multiple Salesforce orgs?**

A: Yes, Professional and Enterprise plans support multiple Salesforce org connections.

**Q: Does it work with Salesforce sandboxes?**

A: Yes, you can connect both production and sandbox environments.

**Q: What Salesforce editions are supported?**

A: Professional, Enterprise, Unlimited, and Developer editions. Essentials edition has limited support.

**Q: Can I use Scopien without Salesforce?**

A: Currently, Scopien requires a Salesforce connection. Standalone features coming soon.

### AI Assistant

**Q: Does the AI learn from my data?**

A: The AI uses your data context during conversations but doesn't permanently learn or store training from your specific data.

**Q: Can I customize the AI's behavior?**

A: Yes, you can train custom shortcuts, set preferences, and define company-specific terms.

**Q: Is my conversation data private?**

A: Yes, conversations are encrypted and only accessible to you and your authorized team members.

### Security

**Q: Is Scopien SOC 2 compliant?**

A: Yes, we are SOC 2 Type II certified. Certificate available upon request.

**Q: Do you support SSO?**

A: Yes, we support SAML 2.0, Okta, Azure AD, Google Workspace, and custom SSO providers.

**Q: Can I restrict access by IP?**

A: Yes, Enterprise plans include IP allowlisting features.

## Error Codes

### HTTP Status Codes

| Code | Meaning | Solution |
|------|---------|----------|
| 400 | Bad Request | Check request format and parameters |
| 401 | Unauthorized | Verify API key or login credentials |
| 403 | Forbidden | Check permissions and scopes |
| 404 | Not Found | Verify resource ID exists |
| 429 | Rate Limited | Implement backoff or upgrade plan |
| 500 | Server Error | Retry or contact support |
| 503 | Service Unavailable | Check status page |

### Application Error Codes

| Code | Description | Solution |
|------|-------------|----------|
| SYNC_001 | Salesforce connection failed | Reconnect Salesforce |
| SYNC_002 | API limit exceeded | Wait for reset |
| AUTH_001 | Invalid credentials | Reset password |
| AUTH_002 | MFA required | Enable MFA |
| RATE_001 | Rate limit exceeded | Reduce request rate |
| DATA_001 | Record not found | Verify record ID |
| PERM_001 | Insufficient permissions | Contact admin |

## Getting Help

### Self-Service Resources

**Documentation:**
- [Getting Started Guide](../getting-started/introduction.md)
- [API Reference](../api-reference/authentication.md)
- [Video Tutorials](https://scopien.com/tutorials)

**Community:**
- [Community Forum](https://community.scopien.com)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/scopien)
- [GitHub Discussions](https://github.com/scopien/discussions)

### Contact Support

**Email Support:**
- General: support@scopien.com
- Technical: tech@scopien.com
- Security: security@scopien.com
- Billing: billing@scopien.com

**Live Chat:**
- Available 24/7 in dashboard
- Average response: < 2 minutes

**Phone Support** (Enterprise only):
- US: +1 (800) SCOPIEN
- UK: +44 20 XXXX XXXX
- Hours: 24/7

**Emergency Support** (Critical issues):
- Email: emergency@scopien.com
- Response SLA: < 1 hour
- Available to Enterprise customers

### Support Ticket Priority

| Priority | Response Time | Examples |
|----------|---------------|----------|
| Critical | 1 hour | Complete service outage, data loss |
| High | 4 hours | Major feature broken, security issue |
| Normal | 24 hours | Feature not working, sync issues |
| Low | 72 hours | Questions, feature requests |

### Before Contacting Support

**Include this information:**
1. Account email
2. Error message/screenshot
3. Steps to reproduce
4. Expected vs actual behavior
5. Browser/device information
6. API request/response (if applicable)

**Example Support Request:**
```
Subject: Cannot connect to Salesforce

Account: john@acme.com
Issue: Getting "Connection failed" error when trying to
       connect Salesforce org

Steps to reproduce:
1. Go to Settings → Integrations
2. Click "Connect Salesforce"
3. Enter credentials
4. Error appears: "Connection failed: Invalid client"

Expected: Should connect successfully
Actual: Connection fails

Browser: Chrome 120 on macOS
Screenshot: [attached]

I've already tried:
- Reconnecting
- Different browser
- Checking Salesforce Connected App settings
```

## Maintenance Windows

**Scheduled Maintenance:**
```
Frequency: Monthly
Time: First Sunday, 2 AM - 4 AM EST
Duration: Up to 2 hours
Notification: 7 days advance notice
```

**During Maintenance:**
- Dashboard may be unavailable
- API requests may fail
- Salesforce sync paused
- Resume automatically after completion

## Next Steps

- [Review Security Best Practices →](security.md)
- [Learn Custom Workflows →](custom-workflows.md)
- [Explore API Documentation →](../api-reference/authentication.md)

---

**Still need help?** Contact support@scopien.com or chat with us in the dashboard.
