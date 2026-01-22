---
title: Real-World Markdown Examples
description: Complex real-world documentation patterns and combinations
keywords: [markdown, examples, documentation, patterns]
category: reference
---

# Real-World Markdown Examples

This document shows **real-world scenarios** combining multiple markdown features as they appear in actual documentation.

---

## API Documentation Example

### GET /api/users/{id}

Retrieve a single user by their ID.

**Endpoint:** `GET https://api.example.com/v1/users/{id}`

**Authentication:** Required (Bearer token)

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | integer | ✅ Yes | The user's unique identifier |
| `include` | string | ❌ No | Comma-separated related resources (orders, profile) |

**Headers:**

```http
Authorization: Bearer YOUR_API_TOKEN
Content-Type: application/json
Accept: application/json
```

**Example Request:**

```bash
curl -X GET \
  'https://api.example.com/v1/users/123?include=orders,profile' \
  -H 'Authorization: Bearer abc123def456' \
  -H 'Accept: application/json'
```

**Success Response (200 OK):**

```json
{
  "data": {
    "id": 123,
    "name": "John Doe",
    "email": "john@example.com",
    "created_at": "2024-01-15T10:30:00Z",
    "status": "active",
    "profile": {
      "bio": "Software developer",
      "location": "San Francisco, CA"
    },
    "orders": [
      {
        "id": 456,
        "total": 99.99,
        "status": "completed"
      }
    ]
  },
  "meta": {
    "timestamp": "2024-01-22T15:45:00Z"
  }
}
```

**Error Responses:**

<details>
<summary><strong>401 Unauthorized</strong></summary>

```json
{
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Invalid or missing authentication token"
  }
}
```

**Possible causes:**
- Token expired
- Invalid token format
- Missing Authorization header

**Solution:** Refresh your access token using the `/auth/refresh` endpoint.

</details>

<details>
<summary><strong>404 Not Found</strong></summary>

```json
{
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User with ID 123 does not exist"
  }
}
```

**Possible causes:**
- User was deleted
- Invalid user ID
- Wrong environment (staging vs production)

</details>

<details>
<summary><strong>429 Too Many Requests</strong></summary>

```json
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Rate limit exceeded. Retry after 60 seconds.",
    "retry_after": 60
  }
}
```

**Rate Limits:**
- Free tier: 100 requests/hour
- Pro tier: 1,000 requests/hour
- Enterprise: Unlimited

</details>

**Code Examples:**

<details>
<summary>JavaScript/TypeScript</summary>

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  created_at: string;
  status: 'active' | 'inactive' | 'suspended';
}

async function getUser(id: number): Promise<User> {
  const response = await fetch(`https://api.example.com/v1/users/${id}`, {
    headers: {
      'Authorization': `Bearer ${process.env.API_TOKEN}`,
      'Accept': 'application/json'
    }
  });

  if (!response.ok) {
    throw new Error(`HTTP ${response.status}: ${response.statusText}`);
  }

  const data = await response.json();
  return data.data;
}

// Usage
try {
  const user = await getUser(123);
  console.log(user.name);
} catch (error) {
  console.error('Failed to fetch user:', error);
}
```

</details>

<details>
<summary>Python</summary>

```python
import requests
from typing import Dict, Any
import os

def get_user(user_id: int, include: list = None) -> Dict[str, Any]:
    """
    Retrieve a user by ID.

    Args:
        user_id: The user's unique identifier
        include: Optional list of related resources to include

    Returns:
        Dictionary containing user data

    Raises:
        requests.HTTPError: If the request fails
    """
    url = f"https://api.example.com/v1/users/{user_id}"

    headers = {
        "Authorization": f"Bearer {os.getenv('API_TOKEN')}",
        "Accept": "application/json"
    }

    params = {}
    if include:
        params["include"] = ",".join(include)

    response = requests.get(url, headers=headers, params=params)
    response.raise_for_status()

    return response.json()["data"]

# Usage
if __name__ == "__main__":
    try:
        user = get_user(123, include=["orders", "profile"])
        print(f"User: {user['name']}")
    except requests.HTTPError as e:
        print(f"Error: {e}")
```

</details>

<details>
<summary>cURL</summary>

```bash
#!/bin/bash

# Set variables
API_TOKEN="your_api_token_here"
USER_ID=123
BASE_URL="https://api.example.com/v1"

# Make request
response=$(curl -s -w "\n%{http_code}" \
  -X GET \
  "${BASE_URL}/users/${USER_ID}?include=orders,profile" \
  -H "Authorization: Bearer ${API_TOKEN}" \
  -H "Accept: application/json")

# Extract status code
http_code=$(echo "$response" | tail -n1)
body=$(echo "$response" | sed '$d')

# Handle response
if [ "$http_code" -eq 200 ]; then
  echo "Success!"
  echo "$body" | jq '.data.name'
else
  echo "Error: HTTP $http_code"
  echo "$body" | jq '.error.message'
fi
```

</details>

:::tip Best Practice
Always implement exponential backoff when retrying failed requests. Start with 1 second, then 2, 4, 8, up to a maximum of 60 seconds.
:::

---

## Tutorial Example with Steps

### Building Your First Dashboard

This tutorial will walk you through creating a custom dashboard with real-time data.

**Prerequisites:**
- ✅ Node.js 18+ installed
- ✅ API token configured
- ✅ Basic JavaScript knowledge

**Time to complete:** ~30 minutes

**What you'll build:**

![Dashboard Preview](https://picsum.photos/800/400)

---

#### Step 1: Project Setup

First, create a new project directory and initialize it:

```bash
mkdir my-dashboard
cd my-dashboard
npm init -y
```

Install the required dependencies:

```bash
npm install react react-dom recharts axios
npm install -D @types/react @types/react-dom vite
```

Your `package.json` should look like this:

```json
{
  "name": "my-dashboard",
  "version": "1.0.0",
  "scripts": {
    "dev": "vite",
    "build": "vite build"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "recharts": "^2.5.0",
    "axios": "^1.4.0"
  },
  "devDependencies": {
    "@types/react": "^18.2.0",
    "@types/react-dom": "^18.2.0",
    "vite": "^4.3.0"
  }
}
```

<details>
<summary>💡 <strong>Tip:</strong> Using npm vs yarn vs pnpm</summary>

All three package managers work fine. Here's how to use each:

| Package Manager | Install Command | Speed | Disk Usage |
|----------------|-----------------|-------|------------|
| npm | `npm install` | Medium | High |
| yarn | `yarn install` | Fast | Medium |
| pnpm | `pnpm install` | Fastest | Low |

We recommend **pnpm** for better performance and disk efficiency.

</details>

---

#### Step 2: Create the Dashboard Component

Create a new file `src/Dashboard.tsx`:

```typescript
import React, { useState, useEffect } from 'react';
import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, Legend } from 'recharts';
import axios from 'axios';

interface DataPoint {
  timestamp: string;
  value: number;
}

interface DashboardProps {
  apiToken: string;
  refreshInterval?: number; // in milliseconds
}

export const Dashboard: React.FC<DashboardProps> = ({
  apiToken,
  refreshInterval = 5000
}) => {
  const [data, setData] = useState<DataPoint[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await axios.get('https://api.example.com/v1/metrics', {
          headers: {
            'Authorization': `Bearer ${apiToken}`
          }
        });
        setData(response.data.data);
        setError(null);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Unknown error');
      } finally {
        setLoading(false);
      }
    };

    // Initial fetch
    fetchData();

    // Set up polling
    const interval = setInterval(fetchData, refreshInterval);

    // Cleanup
    return () => clearInterval(interval);
  }, [apiToken, refreshInterval]);

  if (loading && data.length === 0) {
    return <div className="loading">Loading dashboard...</div>;
  }

  if (error) {
    return (
      <div className="error">
        <h3>⚠️ Error loading dashboard</h3>
        <p>{error}</p>
        <button onClick={() => window.location.reload()}>
          Retry
        </button>
      </div>
    );
  }

  return (
    <div className="dashboard">
      <h1>Real-Time Dashboard</h1>

      <div className="stats">
        <div className="stat-card">
          <h3>Total Users</h3>
          <p className="stat-value">{data[data.length - 1]?.value || 0}</p>
        </div>
        {/* More stat cards... */}
      </div>

      <div className="chart-container">
        <LineChart width={800} height={400} data={data}>
          <CartesianGrid strokeDasharray="3 3" />
          <XAxis dataKey="timestamp" />
          <YAxis />
          <Tooltip />
          <Legend />
          <Line
            type="monotone"
            dataKey="value"
            stroke="#3498db"
            strokeWidth={2}
            dot={false}
          />
        </LineChart>
      </div>
    </div>
  );
};
```

**Key points about this code:**

1. 🔄 **Auto-refresh:** Data refreshes every 5 seconds
2. 🛡️ **Error handling:** Gracefully handles API failures
3. 📊 **Visualization:** Uses Recharts for the line chart
4. ♿ **Loading states:** Shows loading indicator on first load

---

#### Step 3: Add Styling

Create `src/styles.css`:

```css
/* Dashboard Styles */
.dashboard {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Inter', -apple-system, sans-serif;
}

.dashboard h1 {
  color: #2c3e50;
  margin-bottom: 30px;
  font-size: 32px;
  font-weight: 700;
}

/* Stats Grid */
.stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  margin-bottom: 40px;
}

.stat-card {
  background: white;
  padding: 24px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  transition: transform 0.2s;
}

.stat-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}

.stat-card h3 {
  color: #7f8c8d;
  font-size: 14px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin: 0 0 12px 0;
}

.stat-value {
  color: #2c3e50;
  font-size: 36px;
  font-weight: 700;
  margin: 0;
}

/* Chart Container */
.chart-container {
  background: white;
  padding: 24px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

/* Loading State */
.loading {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 400px;
  font-size: 18px;
  color: #7f8c8d;
}

/* Error State */
.error {
  background: #ffe5e5;
  border: 2px solid #ff6b6b;
  border-radius: 8px;
  padding: 24px;
  text-align: center;
}

.error h3 {
  color: #c92a2a;
  margin: 0 0 12px 0;
}

.error p {
  color: #e03131;
  margin: 0 0 20px 0;
}

.error button {
  background: #ff6b6b;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 600;
}

.error button:hover {
  background: #e03131;
}

/* Responsive Design */
@media (max-width: 768px) {
  .dashboard h1 {
    font-size: 24px;
  }

  .chart-container {
    overflow-x: auto;
  }

  .stats {
    grid-template-columns: 1fr;
  }
}
```

---

#### Step 4: Run Your Dashboard

Start the development server:

```bash
npm run dev
```

Open your browser to `http://localhost:5173` and you should see:

![Dashboard Running](https://picsum.photos/800/400)

---

#### Step 5: Deploy to Production

**Option A: Deploy to Vercel**

```bash
npm install -g vercel
vercel login
vercel --prod
```

**Option B: Deploy to Netlify**

```bash
npm run build
npx netlify deploy --prod --dir=dist
```

**Option C: Deploy to your own server**

```bash
# Build for production
npm run build

# Upload the dist/ folder to your server
rsync -avz dist/ user@server:/var/www/dashboard/

# Configure nginx
sudo nano /etc/nginx/sites-available/dashboard
```

Nginx configuration:

```nginx
server {
  listen 80;
  server_name dashboard.example.com;

  root /var/www/dashboard;
  index index.html;

  location / {
    try_files $uri $uri/ /index.html;
  }

  # Cache static assets
  location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
    expires 1y;
    add_header Cache-Control "public, immutable";
  }

  # Gzip compression
  gzip on;
  gzip_types text/plain text/css application/json application/javascript;
  gzip_min_length 1000;
}
```

---

#### Troubleshooting

<details>
<summary><strong>❌ Error: Module not found</strong></summary>

**Problem:** You see an error like `Cannot find module 'recharts'`

**Solution:**

```bash
# Clear node_modules and reinstall
rm -rf node_modules package-lock.json
npm install
```

If using npm workspaces or monorepo:

```bash
npm install recharts --workspace=my-dashboard
```

</details>

<details>
<summary><strong>⚠️ Warning: CORS errors</strong></summary>

**Problem:** Browser console shows CORS policy errors

**Solution:** Add CORS headers to your API server:

```javascript
// Express.js example
app.use((req, res, next) => {
  res.header('Access-Control-Allow-Origin', 'http://localhost:5173');
  res.header('Access-Control-Allow-Headers', 'Authorization, Content-Type');
  next();
});
```

Or use a proxy in `vite.config.ts`:

```typescript
export default {
  server: {
    proxy: {
      '/api': {
        target: 'https://api.example.com',
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, '')
      }
    }
  }
}
```

</details>

<details>
<summary><strong>🐌 Performance: Dashboard is slow</strong></summary>

**Problem:** Dashboard takes a long time to load or update

**Solutions:**

1. **Implement pagination:**

```typescript
const fetchData = async (page = 1, limit = 100) => {
  const response = await axios.get(
    `https://api.example.com/v1/metrics?page=${page}&limit=${limit}`
  );
  return response.data;
};
```

2. **Use React.memo for expensive components:**

```typescript
export const ChartComponent = React.memo(({ data }) => {
  return <LineChart data={data}>...</LineChart>;
});
```

3. **Debounce API calls:**

```typescript
import { debounce } from 'lodash';

const debouncedFetch = debounce(fetchData, 300);
```

</details>

---

#### Next Steps

Congratulations! 🎉 You've built a real-time dashboard. Here's what to learn next:

1. 📊 [Add more chart types](../advanced/charts.md) (bar, pie, scatter)
2. 🔔 [Implement notifications](../advanced/notifications.md) for threshold alerts
3. 🎨 [Customize themes](../user-guide/themes.md) with dark mode
4. 🔐 [Add authentication](../api-reference/authentication.md) with OAuth
5. 📱 [Make it responsive](../advanced/mobile.md) for mobile devices

**Need help?** Join our [Discord community](https://discord.gg/example) or check the [FAQ](../resources/faq.md).

---

## Comparison Table Example

### Pricing Plans Comparison

Compare our plans to find the perfect fit for your team:

| Feature | Free | Pro | Enterprise |
|---------|------|-----|------------|
| **Price** | $0/month | $49/month | Custom |
| **Users** | 1 user | Up to 10 users | Unlimited |
| **API Calls** | 100/hour | 1,000/hour | Unlimited |
| **Storage** | 1 GB | 50 GB | Custom |
| **Support** | Community | Email (24h) | Priority (1h) + Phone |
| **Dashboards** | 1 | Unlimited | Unlimited |
| **Custom Branding** | ❌ | ✅ | ✅ |
| **SSO** | ❌ | ❌ | ✅ |
| **SLA** | ❌ | 99.5% | 99.99% |
| **Backups** | Manual | Daily | Real-time |
| **Analytics** | Basic | Advanced | Advanced + Custom |
| **Webhooks** | ❌ | ✅ 10 | ✅ Unlimited |
| **White-label** | ❌ | ❌ | ✅ |
| **Training** | Self-service | Video guides | Dedicated onboarding |
| **Contract** | None | Monthly | Annual |

<div align="center">

**Ready to get started?**

[Start Free Trial](#) • [Contact Sales](#) • [Compare Features](#)

</div>

---

## Changelog Example

### Version 2.5.0 - January 22, 2026

#### 🚀 New Features

- **Real-time Collaboration:** Multiple users can now edit dashboards simultaneously
  - See live cursors from other team members
  - Automatic conflict resolution
  - Presence indicators showing who's online

- **Advanced Filtering:** New filter builder with 20+ operators
  ```javascript
  // Example usage
  filter({
    field: 'revenue',
    operator: 'greaterThan',
    value: 10000,
    logic: 'AND'
  })
  ```

- **Export to PDF:** Generate beautiful PDF reports
  - Customizable templates
  - Scheduled exports (Pro/Enterprise)
  - Email delivery

#### ✨ Improvements

- 📈 **Performance:** Dashboard loading is now 3x faster
  - Implemented virtual scrolling for large datasets
  - Optimized SQL queries (avg. 50ms → 15ms)
  - Added Redis caching layer

- 🎨 **UI Updates:**
  - Refreshed color palette for better accessibility (WCAG 2.1 AA)
  - New compact view option for tables
  - Improved mobile navigation

- 🔐 **Security Enhancements:**
  - Added support for hardware security keys (YubiKey, etc.)
  - Implemented rate limiting per IP address
  - Enhanced audit logging

#### 🐛 Bug Fixes

- Fixed issue where date filters wouldn't save ([#1234](https://github.com/example/issues/1234))
- Resolved memory leak in WebSocket connections ([#1245](https://github.com/example/issues/1245))
- Corrected timezone handling for users in GMT+12 and beyond
- Fixed chart tooltips being cut off on mobile devices

#### ⚠️ Breaking Changes

> **Important:** Please read before upgrading

1. **API Version:** Default API version changed from `v1` to `v2`
   ```diff
   - https://api.example.com/v1/users
   + https://api.example.com/v2/users
   ```

   **Migration guide:** [v1 to v2 migration](./migration-v1-v2.md)

2. **Authentication:** Deprecated basic auth (will be removed in v3.0.0)
   ```diff
   - Authorization: Basic base64(username:password)
   + Authorization: Bearer jwt_token
   ```

3. **Webhook payload format changed:**
   ```diff
   {
   -  "event": "user.created",
   -  "data": { ... }
   +  "type": "user.created",
   +  "attributes": { ... }
   }
   ```

#### 📦 Dependencies

Updated dependencies for security patches:

- axios: 1.3.0 → 1.6.5 (addresses CVE-2023-XXXXX)
- react: 18.2.0 → 18.3.0
- recharts: 2.5.0 → 2.10.0

#### 🔄 Deprecation Warnings

The following features will be removed in v3.0.0 (March 2026):

| Feature | Replacement | Migration Effort |
|---------|-------------|------------------|
| `GET /api/v1/users` | `GET /api/v2/users` | Low (15 min) |
| Basic auth | Bearer tokens | Medium (1 hour) |
| Old webhook format | New event format | High (3 hours) |

#### 📚 Documentation

- Added 15 new tutorials in the [Learning Center](./tutorials/index.md)
- Updated all API examples to use v2
- New video series: "Advanced Dashboard Techniques"

#### 🙏 Contributors

Big thanks to our community contributors:

- @johndoe for implementing real-time collaboration
- @janedoe for fixing the timezone bug
- @contributor for improving documentation

**Full Changelog:** [v2.4.0...v2.5.0](https://github.com/example/compare/v2.4.0...v2.5.0)

---

### Version 2.4.0 - December 15, 2025

#### Highlights

- Added GraphQL API support
- New dashboard templates library
- Enhanced mobile experience

[Read full changelog →](./v2.4.0.md)

---

## Error Documentation Example

### Common Errors and Solutions

#### Error: `AUTHENTICATION_FAILED`

**Full Error Message:**
```
{
  "error": {
    "code": "AUTHENTICATION_FAILED",
    "message": "Authentication failed: Invalid token",
    "details": {
      "reason": "TOKEN_EXPIRED",
      "expired_at": "2026-01-22T10:30:00Z"
    }
  }
}
```

**Causes:**

1. ❌ Token has expired (most common)
2. ❌ Token format is incorrect
3. ❌ Using wrong token type (API key vs JWT)
4. ❌ Token has been revoked

**Solutions:**

<details>
<summary><strong>Solution 1: Refresh your token</strong></summary>

If your token has expired, request a new one:

```javascript
async function refreshToken(refreshToken) {
  const response = await fetch('https://api.example.com/v2/auth/refresh', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      refresh_token: refreshToken
    })
  });

  const data = await response.json();
  return data.access_token;
}

// Usage
const newToken = await refreshToken(oldRefreshToken);
localStorage.setItem('access_token', newToken);
```

</details>

<details>
<summary><strong>Solution 2: Check token format</strong></summary>

Ensure your token follows the correct format:

**✅ Correct:**
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**❌ Incorrect:**
```
Authorization: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...  (missing "Bearer")
Authorization: Basic eyJhbGci...  (wrong auth type)
```

</details>

<details>
<summary><strong>Solution 3: Generate a new API key</strong></summary>

If your token has been revoked, generate a new one:

1. Go to [Settings → API Keys](https://app.example.com/settings/api-keys)
2. Click "Generate New Key"
3. Copy the key (it won't be shown again!)
4. Update your application configuration

```bash
# Update environment variable
export API_TOKEN="your_new_token_here"
```

</details>

**Prevention Tips:**

💡 Implement automatic token refresh before expiration:

```typescript
class TokenManager {
  private token: string;
  private refreshToken: string;
  private expiresAt: number;

  async getValidToken(): Promise<string> {
    // Refresh if expiring within 5 minutes
    const fiveMinutes = 5 * 60 * 1000;
    if (Date.now() + fiveMinutes > this.expiresAt) {
      await this.refresh();
    }
    return this.token;
  }

  private async refresh() {
    // Refresh logic here
  }
}
```

**Still having issues?**
- Check our [Authentication Guide](../api-reference/authentication.md)
- Contact [support@example.com](mailto:support@example.com)
- Join our [Discord](https://discord.gg/example) for quick help

---

## FAQ Example

### Frequently Asked Questions

#### General Questions

<details>
<summary><strong>What is Scopien AI?</strong></summary>

Scopien AI is an intelligent platform that transforms how you interact with Salesforce. Instead of clicking through menus, you simply describe what you need in natural language, and our AI executes it instantly.

**Key benefits:**
- ⚡ **10x faster** than traditional Salesforce workflows
- 🧠 **Natural language** interface - no training required
- 🔄 **Real-time** data synchronization
- 🔐 **Enterprise-grade** security

[Learn more →](./getting-started/introduction.md)

</details>

<details>
<summary><strong>How much does it cost?</strong></summary>

We offer three plans:

| Plan | Price | Best For |
|------|-------|----------|
| **Free** | $0/month | Individuals & hobbyists |
| **Pro** | $49/month | Small teams (up to 10 users) |
| **Enterprise** | Custom | Large organizations |

All plans include a **14-day free trial** with no credit card required.

[View pricing →](https://example.com/pricing)

</details>

<details>
<summary><strong>Is my data secure?</strong></summary>

Yes! Security is our top priority:

- 🔒 **Encryption:** AES-256 encryption at rest, TLS 1.3 in transit
- 🏢 **Compliance:** SOC 2 Type II, GDPR, HIPAA certified
- 🛡️ **Access Control:** Role-based permissions, SSO, 2FA
- 📊 **Audit Logs:** Complete audit trail of all actions
- 💾 **Backups:** Daily encrypted backups with 30-day retention

[Read our security whitepaper →](./advanced/security.md)

</details>

#### Technical Questions

<details>
<summary><strong>What programming languages do you support?</strong></summary>

We provide official SDKs for:

- JavaScript/TypeScript (npm: `scopien-js`)
- Python (pip: `scopien-python`)
- Ruby (gem: `scopien-ruby`)
- Java (Maven: `com.scopien:scopien-java`)
- Go (go get: `github.com/scopien/scopien-go`)
- PHP (composer: `scopien/scopien-php`)

Plus a REST API that works with any language.

[View SDK documentation →](./api-reference/sdks.md)

</details>

<details>
<summary><strong>Do you support webhooks?</strong></summary>

Yes! We support webhooks for real-time notifications:

**Supported events:**
- `user.created`, `user.updated`, `user.deleted`
- `order.placed`, `order.completed`, `order.cancelled`
- `payment.succeeded`, `payment.failed`
- And 50+ more events

**Example webhook setup:**

```javascript
// Configure webhook endpoint
POST /api/v2/webhooks
{
  "url": "https://your-app.com/webhook",
  "events": ["user.created", "order.placed"],
  "secret": "your_webhook_secret"
}
```

[Complete webhook guide →](./api-reference/webhooks.md)

</details>

#### Troubleshooting

<details>
<summary><strong>Why is my API request failing with 429 error?</strong></summary>

A `429 Too Many Requests` error means you've exceeded your rate limit.

**Rate limits by plan:**
- Free: 100 requests/hour
- Pro: 1,000 requests/hour
- Enterprise: Unlimited

**Solutions:**

1. **Implement exponential backoff:**
   ```javascript
   async function retryWithBackoff(fn, maxRetries = 3) {
     for (let i = 0; i < maxRetries; i++) {
       try {
         return await fn();
       } catch (error) {
         if (error.status !== 429 || i === maxRetries - 1) {
           throw error;
         }
         const delay = Math.pow(2, i) * 1000;
         await new Promise(resolve => setTimeout(resolve, delay));
       }
     }
   }
   ```

2. **Cache responses** to reduce API calls
3. **Upgrade your plan** for higher limits
4. **Batch requests** where possible

</details>

<details>
<summary><strong>The dashboard isn't loading. What should I check?</strong></summary>

**Quick diagnostics checklist:**

- [ ] Check your internet connection
- [ ] Clear browser cache (`Ctrl+Shift+Del` / `Cmd+Shift+Del`)
- [ ] Try incognito/private browsing mode
- [ ] Check [status page](https://status.example.com) for outages
- [ ] Verify API token is valid

**Still not working?**

1. Open browser console (`F12`)
2. Look for error messages
3. Take a screenshot
4. Contact support with the screenshot

[More troubleshooting tips →](./advanced/troubleshooting.md)

</details>

**Can't find your question?**

- 💬 [Ask in our community forum](https://community.example.com)
- 📧 [Email support](mailto:support@example.com)
- 📚 [Browse full documentation](./index.md)

---

## Best Practices Guide Example

### 🏆 API Best Practices

Follow these best practices to build robust, performant applications.

#### 1. Authentication

**✅ DO:**

```javascript
// Store tokens securely
localStorage.setItem('access_token', token);  // For web apps
// Or use httpOnly cookies for better security

// Include token in all requests
const headers = {
  'Authorization': `Bearer ${token}`,
  'Content-Type': 'application/json'
};
```

**❌ DON'T:**

```javascript
// Never commit tokens to version control
const API_TOKEN = 'abc123def456';  // Bad!

// Never pass tokens in URL
fetch(`https://api.example.com/users?token=${token}`);  // Bad!
```

#### 2. Error Handling

**✅ DO:**

```typescript
async function fetchUser(id: number) {
  try {
    const response = await fetch(`/api/users/${id}`);

    if (!response.ok) {
      const error = await response.json();
      throw new ApiError(error.message, response.status);
    }

    return await response.json();
  } catch (error) {
    if (error instanceof ApiError) {
      // Handle API errors specifically
      if (error.status === 404) {
        console.log('User not found');
      } else if (error.status === 429) {
        // Implement retry logic
      }
    } else {
      // Handle network errors
      console.error('Network error:', error);
    }
    throw error;
  }
}
```

**❌ DON'T:**

```javascript
// Don't ignore errors
fetch('/api/users')
  .then(res => res.json())
  .catch(() => {});  // Bad!

// Don't use generic error messages
catch (error) {
  alert('Something went wrong');  // Not helpful!
}
```

#### 3. Rate Limiting

**✅ DO:**

```typescript
class RateLimiter {
  private queue: Array<() => Promise<any>> = [];
  private processing = false;
  private requestsPerSecond = 10;

  async add<T>(fn: () => Promise<T>): Promise<T> {
    return new Promise((resolve, reject) => {
      this.queue.push(async () => {
        try {
          const result = await fn();
          resolve(result);
        } catch (error) {
          reject(error);
        }
      });
      this.process();
    });
  }

  private async process() {
    if (this.processing || this.queue.length === 0) return;

    this.processing = true;
    const fn = this.queue.shift()!;

    await fn();
    await new Promise(resolve =>
      setTimeout(resolve, 1000 / this.requestsPerSecond)
    );

    this.processing = false;
    this.process();
  }
}

// Usage
const limiter = new RateLimiter();
const users = await limiter.add(() => fetchUsers());
```

**❌ DON'T:**

```javascript
// Don't spam the API
for (let i = 0; i < 1000; i++) {
  fetch(`/api/users/${i}`);  // Bad! Will hit rate limit
}
```

---

**Last Updated:** January 22, 2026
**File Size:** ~28KB
**Reading Time:** 45 minutes

*End of real-world examples document 🚀*
