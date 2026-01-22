---
title: Webhooks
description: Set up webhooks to receive real-time notifications from Scopien AI
---

# Webhooks

Webhooks allow your application to receive real-time notifications when events occur in Scopien AI.

## Overview

Instead of polling the API, webhooks push data to your application:

```
Event occurs → Scopien sends HTTP POST → Your endpoint receives data
```

**Benefits:**
- Real-time notifications
- Reduced API calls
- Lower latency
- Event-driven architecture

## Setting Up Webhooks

### Via Dashboard

1. Go to **Settings → Webhooks**
2. Click **"Create Webhook"**
3. Configure:
   ```
   Name: Production Webhook
   URL: https://your-app.com/webhooks/scopien
   Events: [x] Opportunity updated
           [x] Lead created
           [ ] Case closed
   Secret: auto-generated
   Active: [x] Enabled
   ```
4. Click **"Create"**
5. Save the webhook secret securely

### Via API

```http
POST https://api.scopien.com/v1/webhooks
Authorization: Bearer sk_live_abc123
Content-Type: application/json

{
  "url": "https://your-app.com/webhooks/scopien",
  "events": ["opportunity.updated", "lead.created"],
  "description": "Production webhook",
  "secret": "whsec_abc123def456"
}
```

**Response:**
```json
{
  "data": {
    "id": "wh_abc123",
    "url": "https://your-app.com/webhooks/scopien",
    "events": ["opportunity.updated", "lead.created"],
    "secret": "whsec_abc123def456",
    "status": "active",
    "createdAt": "2026-01-22T10:00:00Z"
  }
}
```

## Webhook Events

### Available Events

| Event | Description |
|-------|-------------|
| `opportunity.created` | New opportunity created |
| `opportunity.updated` | Opportunity modified |
| `opportunity.deleted` | Opportunity deleted |
| `opportunity.closed` | Opportunity closed (won/lost) |
| `account.created` | New account created |
| `account.updated` | Account modified |
| `contact.created` | New contact created |
| `contact.updated` | Contact modified |
| `lead.created` | New lead created |
| `lead.updated` | Lead modified |
| `lead.converted` | Lead converted |
| `case.created` | New case created |
| `case.updated` | Case modified |
| `case.closed` | Case closed |
| `task.created` | New task created |
| `task.completed` | Task completed |

### Wildcard Events

Subscribe to all events of a type:

```json
{
  "events": ["opportunity.*", "lead.*"]
}
```

## Webhook Payload

### Payload Structure

```json
{
  "id": "evt_abc123",
  "event": "opportunity.updated",
  "timestamp": "2026-01-22T10:30:00Z",
  "data": {
    "id": "opp_xyz789",
    "name": "Acme Corp Deal",
    "amount": 100000,
    "stage": "Closed Won",
    "previousStage": "Negotiation",
    "updatedBy": "usr_def456"
  },
  "changes": {
    "stage": {
      "old": "Negotiation",
      "new": "Closed Won"
    },
    "amount": {
      "old": 95000,
      "new": 100000
    }
  }
}
```

### Example Payloads

**Opportunity Closed**
```json
{
  "id": "evt_abc123",
  "event": "opportunity.closed",
  "timestamp": "2026-01-22T10:30:00Z",
  "data": {
    "id": "opp_xyz789",
    "name": "Acme Corp Deal",
    "amount": 100000,
    "stage": "Closed Won",
    "closedDate": "2026-01-22",
    "probability": 100
  }
}
```

**Lead Created**
```json
{
  "id": "evt_def456",
  "event": "lead.created",
  "timestamp": "2026-01-22T11:00:00Z",
  "data": {
    "id": "lead_new123",
    "firstName": "John",
    "lastName": "Smith",
    "email": "john@example.com",
    "company": "TechStart",
    "source": "Website",
    "status": "New"
  }
}
```

**Case Updated**
```json
{
  "id": "evt_ghi789",
  "event": "case.updated",
  "timestamp": "2026-01-22T11:30:00Z",
  "data": {
    "id": "case_abc456",
    "subject": "Billing Issue",
    "status": "In Progress",
    "priority": "High",
    "assignedTo": "usr_support01"
  },
  "changes": {
    "status": {
      "old": "New",
      "new": "In Progress"
    }
  }
}
```

## Receiving Webhooks

### Endpoint Requirements

Your webhook endpoint must:
- ✅ Accept `POST` requests
- ✅ Use HTTPS (required in production)
- ✅ Return `200 OK` within 5 seconds
- ✅ Verify webhook signature
- ✅ Handle idempotency

### Implementation Examples

**Node.js/Express**
```javascript
const express = require('express');
const crypto = require('crypto');

const app = express();
app.use(express.raw({ type: 'application/json' }));

const WEBHOOK_SECRET = process.env.SCOPIEN_WEBHOOK_SECRET;

app.post('/webhooks/scopien', (req, res) => {
  // 1. Verify signature
  const signature = req.headers['x-scopien-signature'];
  const isValid = verifySignature(req.body, signature, WEBHOOK_SECRET);

  if (!isValid) {
    return res.status(401).send('Invalid signature');
  }

  // 2. Parse payload
  const event = JSON.parse(req.body);

  // 3. Handle event
  switch (event.event) {
    case 'opportunity.closed':
      handleOpportunityClosed(event.data);
      break;
    case 'lead.created':
      handleLeadCreated(event.data);
      break;
    default:
      console.log(`Unhandled event: ${event.event}`);
  }

  // 4. Return 200
  res.status(200).send('OK');
});

function verifySignature(payload, signature, secret) {
  const expectedSignature = crypto
    .createHmac('sha256', secret)
    .update(payload)
    .digest('hex');

  return crypto.timingSafeEqual(
    Buffer.from(signature),
    Buffer.from(expectedSignature)
  );
}

app.listen(3000);
```

**Python/Flask**
```python
from flask import Flask, request
import hmac
import hashlib
import json

app = Flask(__name__)
WEBHOOK_SECRET = os.environ.get('SCOPIEN_WEBHOOK_SECRET')

@app.route('/webhooks/scopien', methods=['POST'])
def webhook():
    # 1. Verify signature
    signature = request.headers.get('X-Scopien-Signature')
    if not verify_signature(request.data, signature, WEBHOOK_SECRET):
        return 'Invalid signature', 401

    # 2. Parse payload
    event = json.loads(request.data)

    # 3. Handle event
    if event['event'] == 'opportunity.closed':
        handle_opportunity_closed(event['data'])
    elif event['event'] == 'lead.created':
        handle_lead_created(event['data'])

    # 4. Return 200
    return 'OK', 200

def verify_signature(payload, signature, secret):
    expected_signature = hmac.new(
        secret.encode(),
        payload,
        hashlib.sha256
    ).hexdigest()

    return hmac.compare_digest(signature, expected_signature)

if __name__ == '__main__':
    app.run(port=3000)
```

**PHP**
```php
<?php

$webhookSecret = getenv('SCOPIEN_WEBHOOK_SECRET');
$payload = file_get_contents('php://input');
$signature = $_SERVER['HTTP_X_SCOPIEN_SIGNATURE'];

// 1. Verify signature
$expectedSignature = hash_hmac('sha256', $payload, $webhookSecret);

if (!hash_equals($signature, $expectedSignature)) {
    http_response_code(401);
    die('Invalid signature');
}

// 2. Parse payload
$event = json_decode($payload, true);

// 3. Handle event
switch ($event['event']) {
    case 'opportunity.closed':
        handleOpportunityClosed($event['data']);
        break;
    case 'lead.created':
        handleLeadCreated($event['data']);
        break;
}

// 4. Return 200
http_response_code(200);
echo 'OK';

?>
```

## Signature Verification

### Why Verify Signatures

Signature verification ensures:
- Webhook is from Scopien
- Payload hasn't been tampered with
- Request is authentic

### Signature Format

```
X-Scopien-Signature: sha256=abc123def456...
```

### Verification Process

```javascript
const crypto = require('crypto');

function verifyWebhook(payload, signature, secret) {
  // 1. Extract signature
  const expectedSig = signature.replace('sha256=', '');

  // 2. Compute signature
  const computedSig = crypto
    .createHmac('sha256', secret)
    .update(payload)
    .digest('hex');

  // 3. Compare (timing-safe)
  return crypto.timingSafeEqual(
    Buffer.from(expectedSig),
    Buffer.from(computedSig)
  );
}
```

## Best Practices

### ✅ Do's

**Respond Quickly**
```javascript
// ✅ Process asynchronously
app.post('/webhook', async (req, res) => {
  // Return 200 immediately
  res.status(200).send('OK');

  // Process in background
  await queue.add('processWebhook', req.body);
});
```

**Handle Duplicates**
```javascript
// ✅ Track processed event IDs
const processedEvents = new Set();

if (processedEvents.has(event.id)) {
  return res.status(200).send('Already processed');
}

processedEvents.add(event.id);
// Process event...
```

**Implement Retries**
```javascript
// ✅ Graceful error handling
try {
  await processEvent(event);
} catch (error) {
  console.error('Error processing webhook:', error);
  // Still return 200 to prevent retries for permanent errors
  if (isPermanentError(error)) {
    return res.status(200).send('OK');
  }
  // Return 500 for temporary errors (Scopien will retry)
  return res.status(500).send('Error');
}
```

### ❌ Don'ts

**Don't Process Synchronously**
```javascript
// ❌ Slow processing
app.post('/webhook', async (req, res) => {
  await heavyProcessing(req.body);  // Too slow!
  res.status(200).send('OK');
});
```

**Don't Skip Verification**
```javascript
// ❌ Accepting all requests
app.post('/webhook', (req, res) => {
  // No signature verification - UNSAFE!
  processEvent(req.body);
});
```

## Testing Webhooks

### Using the Dashboard

1. Go to **Settings → Webhooks**
2. Select your webhook
3. Click **"Send Test Event"**
4. Choose event type
5. View response

### Manual Testing

```bash
curl -X POST https://your-app.com/webhooks/scopien \
  -H "Content-Type: application/json" \
  -H "X-Scopien-Signature: sha256=abc123..." \
  -d '{
    "id": "evt_test123",
    "event": "opportunity.updated",
    "timestamp": "2026-01-22T10:30:00Z",
    "data": {
      "id": "opp_test",
      "name": "Test Opportunity",
      "amount": 50000
    }
  }'
```

### Local Testing with ngrok

```bash
# 1. Start ngrok
ngrok http 3000

# 2. Use ngrok URL as webhook URL
# https://abc123.ngrok.io/webhooks/scopien

# 3. Test locally
```

## Monitoring

### Webhook Logs

View webhook delivery history:

```
Recent Deliveries
─────────────────────────────────────────
✅ 10:45 AM - opportunity.updated
   Status: 200 OK
   Duration: 234ms

✅ 10:30 AM - lead.created
   Status: 200 OK
   Duration: 156ms

❌ 10:15 AM - case.updated
   Status: 500 Error
   Retried: 3 times
   Next retry: 10:20 AM
```

### Retry Policy

Failed webhooks are retried automatically:

```
Attempt 1: Immediate
Attempt 2: 5 seconds
Attempt 3: 30 seconds
Attempt 4: 2 minutes
Attempt 5: 10 minutes
Attempt 6: 1 hour (final)
```

### Failure Notifications

Get notified of persistent failures:

```json
{
  "event": "webhook.failed",
  "webhookId": "wh_abc123",
  "url": "https://your-app.com/webhook",
  "failureCount": 6,
  "lastError": "Connection timeout",
  "disabled": true
}
```

## Managing Webhooks

### List Webhooks

```http
GET /v1/webhooks
```

### Update Webhook

```http
PATCH /v1/webhooks/{id}
```

**Request:**
```json
{
  "events": ["opportunity.*", "lead.created"],
  "url": "https://new-url.com/webhook"
}
```

### Disable Webhook

```http
POST /v1/webhooks/{id}/disable
```

### Delete Webhook

```http
DELETE /v1/webhooks/{id}
```

## Advanced Features

### Event Filtering

Filter events by conditions:

```json
{
  "event": "opportunity.updated",
  "filters": {
    "amount_greater_than": 50000,
    "stage": ["Negotiation", "Closed Won"]
  }
}
```

### Custom Headers

Add custom headers to webhook requests:

```json
{
  "headers": {
    "X-Custom-Header": "value",
    "Authorization": "Bearer token123"
  }
}
```

### Batch Webhooks

Receive multiple events in one request:

```json
{
  "batch": true,
  "maxBatchSize": 100,
  "maxWaitTime": 60
}
```

**Payload:**
```json
{
  "events": [
    { "id": "evt_1", "event": "lead.created", /* ... */ },
    { "id": "evt_2", "event": "lead.created", /* ... */ },
    { "id": "evt_3", "event": "opportunity.updated", /* ... */ }
  ]
}
```

## Troubleshooting

### Webhook Not Receiving Events

**Checklist:**
- [ ] Webhook is enabled
- [ ] URL is correct and accessible
- [ ] HTTPS is properly configured
- [ ] Firewall allows Scopien IPs
- [ ] Endpoint returns 200 OK
- [ ] Event type is subscribed

### Signature Verification Failing

**Solutions:**
1. Check secret is correct
2. Use raw request body
3. Verify timing-safe comparison
4. Check signature header name

### High Failure Rate

**Causes:**
- Slow endpoint (> 5s timeout)
- Server errors (500)
- Network issues
- Incorrect error handling

## Security

### IP Allowlist

Restrict webhooks to Scopien IPs:

```
52.91.14.224/32
18.210.11.89/32
34.228.82.133/32
```

### HTTPS Required

Production webhooks must use HTTPS:

```
✅ https://your-app.com/webhook
❌ http://your-app.com/webhook
```

### Rate Limiting

Webhook endpoints should handle bursts:
- Up to 100 events/second
- Batch mode: 10 batches/second

## Next Steps

- [Learn about API Endpoints →](endpoints.md)
- [View Authentication Guide →](authentication.md)
- [Explore Event Reference →](https://docs.scopien.com/events)

---

**Webhook Issues?** Contact webhooks@scopien.com or visit our [troubleshooting guide](../advanced/troubleshooting.md)
