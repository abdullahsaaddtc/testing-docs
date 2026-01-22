---
title: API Authentication
description: Learn how to authenticate with the Scopien AI API
---

# API Authentication

Scopien AI uses API keys for authentication. This guide covers how to generate, manage, and use API keys securely.

## Authentication Methods

### API Key Authentication

The primary authentication method for Scopien API.

**Header Format:**
```http
Authorization: Bearer sk_live_abc123def456ghi789
```

**Example Request:**
```bash
curl https://api.scopien.com/v1/opportunities \
  -H "Authorization: Bearer sk_live_abc123def456ghi789" \
  -H "Content-Type: application/json"
```

## Generating API Keys

### Via Dashboard

1. Go to **Settings → API Keys**
2. Click **"Create New Key"**
3. Configure key settings:
   ```
   Name: Production API Key
   Type: ○ Test  ● Live
   Permissions: [x] Read  [x] Write
   Rate Limit: 1000 req/hour
   Expires: Never
   ```
4. Click **"Generate"**
5. Copy and save the key securely

⚠️ **Important**: Keys are only shown once. Store them securely!

### Via API

```bash
POST https://api.scopien.com/v1/api-keys
Authorization: Bearer <admin-key>
Content-Type: application/json

{
  "name": "Production Key",
  "scopes": ["read", "write"],
  "rateLimit": 1000,
  "expiresAt": null
}
```

**Response:**
```json
{
  "id": "key_abc123",
  "key": "sk_live_abc123def456ghi789",
  "name": "Production Key",
  "scopes": ["read", "write"],
  "rateLimit": 1000,
  "createdAt": "2026-01-22T10:00:00Z"
}
```

## API Key Types

### Test Keys

For development and testing.

```
Format: sk_test_...
Environment: Sandbox
Rate Limit: 100 req/hour
Data Access: Test data only
```

**Use cases:**
- Local development
- Automated testing
- Integration testing
- Debugging

### Live Keys

For production use.

```
Format: sk_live_...
Environment: Production
Rate Limit: 1000+ req/hour
Data Access: Full production data
```

**Use cases:**
- Production applications
- Live integrations
- Customer-facing features

## Permissions & Scopes

### Available Scopes

Control what your API key can access:

| Scope | Description | Actions |
|-------|-------------|---------|
| `read` | Read-only access | GET requests |
| `write` | Create/update records | POST, PUT, PATCH |
| `delete` | Delete records | DELETE requests |
| `admin` | Full access | All operations |

### Granular Permissions

Set permissions per resource:

```json
{
  "scopes": {
    "opportunities": ["read", "write"],
    "accounts": ["read"],
    "contacts": ["read", "write"],
    "reports": ["read"]
  }
}
```

**Example:**
```bash
# This key can only read accounts
curl https://api.scopien.com/v1/accounts \
  -H "Authorization: Bearer sk_live_readonly123"

# This will fail (no write permission)
curl -X POST https://api.scopien.com/v1/accounts \
  -H "Authorization: Bearer sk_live_readonly123" \
  -d '{"name": "New Account"}'
```

## Security Best Practices

### ✅ Do's

**Key Storage**
```javascript
// ✅ Use environment variables
const API_KEY = process.env.SCOPIEN_API_KEY;

// ❌ Never hardcode keys
const API_KEY = 'sk_live_abc123...';  // NEVER DO THIS!
```

**Key Rotation**
- Rotate keys every 90 days
- Use different keys per environment
- Revoke compromised keys immediately
- Monitor key usage

**Access Control**
- Use least-privilege scope
- Create separate keys per application
- Set appropriate rate limits
- Enable key expiration

### ❌ Don'ts

**Never expose keys:**
```javascript
// ❌ Don't commit to version control
// .env
API_KEY=sk_live_abc123...

// ✅ Add to .gitignore
# .gitignore
.env
.env.local
```

**Don't share keys:**
- ❌ Via email
- ❌ In chat messages
- ❌ In screenshots
- ❌ In public repositories

## Rate Limiting

### Rate Limits by Plan

| Plan | Requests/Hour | Requests/Day | Burst |
|------|---------------|--------------|-------|
| Starter | 1,000 | 10,000 | 50 |
| Professional | 5,000 | 50,000 | 200 |
| Enterprise | 10,000+ | Unlimited | Custom |

### Rate Limit Headers

Every response includes rate limit info:

```http
HTTP/1.1 200 OK
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 945
X-RateLimit-Reset: 1674389400
```

**Handling rate limits:**
```javascript
async function makeRequest(url) {
  const response = await fetch(url, {
    headers: {
      'Authorization': `Bearer ${API_KEY}`
    }
  });

  const remaining = response.headers.get('X-RateLimit-Remaining');

  if (remaining < 10) {
    console.warn('Approaching rate limit');
  }

  if (response.status === 429) {
    const resetTime = response.headers.get('X-RateLimit-Reset');
    const waitTime = resetTime - Date.now();
    await sleep(waitTime);
    return makeRequest(url);  // Retry
  }

  return response.json();
}
```

## OAuth 2.0 (Coming Soon)

For user-facing applications:

### Authorization Code Flow

```
1. User → Your App: "Connect Scopien"
2. Your App → Scopien: Redirect to /oauth/authorize
3. User → Scopien: Login & approve
4. Scopien → Your App: Authorization code
5. Your App → Scopien: Exchange code for token
6. Scopien → Your App: Access token & refresh token
```

**Authorization URL:**
```
https://app.scopien.com/oauth/authorize
  ?client_id=your_client_id
  &redirect_uri=https://your-app.com/callback
  &response_type=code
  &scope=read write
```

**Token Exchange:**
```bash
POST https://api.scopien.com/oauth/token
Content-Type: application/json

{
  "grant_type": "authorization_code",
  "code": "auth_code_here",
  "client_id": "your_client_id",
  "client_secret": "your_client_secret",
  "redirect_uri": "https://your-app.com/callback"
}
```

## Error Handling

### Authentication Errors

**401 Unauthorized**
```json
{
  "error": {
    "code": "unauthorized",
    "message": "Invalid API key",
    "details": "The API key provided is invalid or has been revoked"
  }
}
```

**403 Forbidden**
```json
{
  "error": {
    "code": "forbidden",
    "message": "Insufficient permissions",
    "details": "This API key does not have 'write' permission for opportunities"
  }
}
```

**429 Too Many Requests**
```json
{
  "error": {
    "code": "rate_limit_exceeded",
    "message": "Rate limit exceeded",
    "details": "Rate limit will reset in 1847 seconds",
    "retryAfter": 1847
  }
}
```

## Code Examples

### JavaScript/Node.js

```javascript
const SCOPIEN_API_KEY = process.env.SCOPIEN_API_KEY;
const BASE_URL = 'https://api.scopien.com/v1';

async function getOpportunities() {
  const response = await fetch(`${BASE_URL}/opportunities`, {
    method: 'GET',
    headers: {
      'Authorization': `Bearer ${SCOPIEN_API_KEY}`,
      'Content-Type': 'application/json'
    }
  });

  if (!response.ok) {
    throw new Error(`API error: ${response.status}`);
  }

  return response.json();
}

// Usage
getOpportunities()
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

### Python

```python
import os
import requests

API_KEY = os.environ.get('SCOPIEN_API_KEY')
BASE_URL = 'https://api.scopien.com/v1'

headers = {
    'Authorization': f'Bearer {API_KEY}',
    'Content-Type': 'application/json'
}

def get_opportunities():
    response = requests.get(
        f'{BASE_URL}/opportunities',
        headers=headers
    )
    response.raise_for_status()
    return response.json()

# Usage
try:
    opportunities = get_opportunities()
    print(opportunities)
except requests.exceptions.RequestException as e:
    print(f'Error: {e}')
```

### PHP

```php
<?php

$apiKey = getenv('SCOPIEN_API_KEY');
$baseUrl = 'https://api.scopien.com/v1';

function getOpportunities($apiKey, $baseUrl) {
    $ch = curl_init("$baseUrl/opportunities");

    curl_setopt_array($ch, [
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_HTTPHEADER => [
            "Authorization: Bearer $apiKey",
            "Content-Type: application/json"
        ]
    ]);

    $response = curl_exec($ch);
    $httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
    curl_close($ch);

    if ($httpCode !== 200) {
        throw new Exception("API error: $httpCode");
    }

    return json_decode($response, true);
}

// Usage
try {
    $opportunities = getOpportunities($apiKey, $baseUrl);
    print_r($opportunities);
} catch (Exception $e) {
    echo "Error: " . $e->getMessage();
}
?>
```

### Ruby

```ruby
require 'net/http'
require 'json'

API_KEY = ENV['SCOPIEN_API_KEY']
BASE_URL = 'https://api.scopien.com/v1'

def get_opportunities
  uri = URI("#{BASE_URL}/opportunities")

  request = Net::HTTP::Get.new(uri)
  request['Authorization'] = "Bearer #{API_KEY}"
  request['Content-Type'] = 'application/json'

  response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
    http.request(request)
  end

  raise "API error: #{response.code}" unless response.is_a?(Net::HTTPSuccess)

  JSON.parse(response.body)
end

# Usage
begin
  opportunities = get_opportunities
  puts opportunities
rescue => e
  puts "Error: #{e.message}"
end
```

## Testing Authentication

### Test Your API Key

```bash
curl https://api.scopien.com/v1/auth/test \
  -H "Authorization: Bearer sk_test_abc123"
```

**Success Response:**
```json
{
  "authenticated": true,
  "keyId": "key_abc123",
  "scopes": ["read", "write"],
  "rateLimit": {
    "limit": 1000,
    "remaining": 999,
    "reset": 1674389400
  }
}
```

## Managing API Keys

### List All Keys

```bash
GET https://api.scopien.com/v1/api-keys
```

### Revoke a Key

```bash
DELETE https://api.scopien.com/v1/api-keys/{key_id}
```

### Update Key Permissions

```bash
PATCH https://api.scopien.com/v1/api-keys/{key_id}
{
  "scopes": ["read"]
}
```

## Next Steps

- [Explore API Endpoints →](endpoints.md)
- [Set Up Webhooks →](webhooks.md)
- [View SDK Documentation →](https://github.com/scopien/sdk)

---

**Security Concern?** Report to security@scopien.com
