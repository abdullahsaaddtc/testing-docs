---
title: API Endpoints
description: Complete reference for all Scopien AI API endpoints
---

# API Endpoints

Complete reference for all available API endpoints in Scopien AI.

## Base URL

```
https://api.scopien.com/v1
```

## Response Format

All responses return JSON:

```json
{
  "data": { /* response data */ },
  "meta": {
    "requestId": "req_abc123",
    "timestamp": "2026-01-22T10:30:00Z"
  }
}
```

## Pagination

List endpoints support pagination:

```bash
GET /v1/opportunities?page=2&limit=50
```

**Response:**
```json
{
  "data": [ /* items */ ],
  "pagination": {
    "page": 2,
    "limit": 50,
    "total": 235,
    "totalPages": 5,
    "hasNext": true,
    "hasPrevious": true
  }
}
```

## Opportunities

### List Opportunities

```http
GET /v1/opportunities
```

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `page` | integer | Page number (default: 1) |
| `limit` | integer | Items per page (default: 25, max: 100) |
| `stage` | string | Filter by stage |
| `minAmount` | number | Minimum opportunity amount |
| `closeDateFrom` | date | Filter by close date (from) |
| `closeDateTo` | date | Filter by close date (to) |

**Example:**
```bash
curl "https://api.scopien.com/v1/opportunities?stage=Negotiation&minAmount=50000" \
  -H "Authorization: Bearer sk_live_abc123"
```

**Response:**
```json
{
  "data": [
    {
      "id": "opp_abc123",
      "name": "Acme Corp Deal",
      "amount": 75000,
      "stage": "Negotiation",
      "probability": 80,
      "closeDate": "2026-02-15",
      "accountId": "acc_xyz789",
      "ownerId": "usr_def456",
      "createdAt": "2026-01-10T09:00:00Z",
      "updatedAt": "2026-01-22T10:30:00Z"
    }
  ],
  "pagination": { /* ... */ }
}
```

### Get Opportunity

```http
GET /v1/opportunities/{id}
```

**Example:**
```bash
curl "https://api.scopien.com/v1/opportunities/opp_abc123" \
  -H "Authorization: Bearer sk_live_abc123"
```

### Create Opportunity

```http
POST /v1/opportunities
```

**Request Body:**
```json
{
  "name": "New Enterprise Deal",
  "amount": 100000,
  "stage": "Prospecting",
  "closeDate": "2026-03-31",
  "accountId": "acc_xyz789",
  "description": "Large enterprise opportunity"
}
```

**Response:**
```json
{
  "data": {
    "id": "opp_new123",
    "name": "New Enterprise Deal",
    "amount": 100000,
    "stage": "Prospecting",
    "closeDate": "2026-03-31",
    "createdAt": "2026-01-22T10:30:00Z"
  }
}
```

### Update Opportunity

```http
PATCH /v1/opportunities/{id}
```

**Request Body:**
```json
{
  "stage": "Closed Won",
  "amount": 110000
}
```

### Delete Opportunity

```http
DELETE /v1/opportunities/{id}
```

## Accounts

### List Accounts

```http
GET /v1/accounts
```

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `search` | string | Search by name |
| `industry` | string | Filter by industry |
| `minRevenue` | number | Minimum annual revenue |
| `state` | string | Filter by state |

### Get Account

```http
GET /v1/accounts/{id}
```

### Create Account

```http
POST /v1/accounts
```

**Request Body:**
```json
{
  "name": "TechStart Inc",
  "industry": "Technology",
  "website": "https://techstart.com",
  "phone": "+1-555-0123",
  "billingAddress": {
    "street": "123 Main St",
    "city": "San Francisco",
    "state": "CA",
    "postalCode": "94105",
    "country": "USA"
  }
}
```

### Update Account

```http
PATCH /v1/accounts/{id}
```

### Delete Account

```http
DELETE /v1/accounts/{id}
```

## Contacts

### List Contacts

```http
GET /v1/contacts
```

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `accountId` | string | Filter by account |
| `email` | string | Filter by email |
| `title` | string | Filter by job title |

### Get Contact

```http
GET /v1/contacts/{id}
```

**Response:**
```json
{
  "data": {
    "id": "con_abc123",
    "firstName": "John",
    "lastName": "Smith",
    "email": "john.smith@acme.com",
    "phone": "+1-555-0123",
    "title": "VP of Sales",
    "accountId": "acc_xyz789",
    "mailingAddress": {
      "street": "456 Business Ave",
      "city": "New York",
      "state": "NY",
      "postalCode": "10001",
      "country": "USA"
    }
  }
}
```

### Create Contact

```http
POST /v1/contacts
```

### Update Contact

```http
PATCH /v1/contacts/{id}
```

### Delete Contact

```http
DELETE /v1/contacts/{id}
```

## Leads

### List Leads

```http
GET /v1/leads
```

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `status` | string | Filter by status |
| `source` | string | Filter by lead source |
| `minScore` | number | Minimum lead score |
| `createdFrom` | date | Filter by creation date |

### Get Lead

```http
GET /v1/leads/{id}
```

### Create Lead

```http
POST /v1/leads
```

**Request Body:**
```json
{
  "firstName": "Jane",
  "lastName": "Doe",
  "company": "Innovation Labs",
  "email": "jane@innovationlabs.com",
  "phone": "+1-555-0199",
  "title": "CTO",
  "leadSource": "Website",
  "status": "New",
  "industry": "Technology",
  "description": "Interested in Enterprise plan"
}
```

### Convert Lead

```http
POST /v1/leads/{id}/convert
```

**Request Body:**
```json
{
  "createOpportunity": true,
  "opportunityName": "Innovation Labs Deal",
  "opportunityAmount": 50000
}
```

**Response:**
```json
{
  "data": {
    "contactId": "con_new123",
    "accountId": "acc_new456",
    "opportunityId": "opp_new789"
  }
}
```

### Update Lead

```http
PATCH /v1/leads/{id}
```

### Delete Lead

```http
DELETE /v1/leads/{id}
```

## Cases

### List Cases

```http
GET /v1/cases
```

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `status` | string | Filter by status |
| `priority` | string | Filter by priority |
| `accountId` | string | Filter by account |

### Get Case

```http
GET /v1/cases/{id}
```

### Create Case

```http
POST /v1/cases
```

**Request Body:**
```json
{
  "subject": "Billing Issue",
  "description": "Customer reports incorrect invoice amount",
  "accountId": "acc_xyz789",
  "contactId": "con_abc123",
  "priority": "High",
  "status": "New",
  "origin": "Email"
}
```

### Update Case

```http
PATCH /v1/cases/{id}
```

### Close Case

```http
POST /v1/cases/{id}/close
```

**Request Body:**
```json
{
  "resolution": "Issue resolved, invoice corrected"
}
```

## Tasks

### List Tasks

```http
GET /v1/tasks
```

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `status` | string | Filter by status |
| `priority` | string | Filter by priority |
| `dueDate` | date | Filter by due date |
| `assignedTo` | string | Filter by assignee |

### Get Task

```http
GET /v1/tasks/{id}
```

### Create Task

```http
POST /v1/tasks
```

**Request Body:**
```json
{
  "subject": "Follow up with client",
  "description": "Discuss pricing and next steps",
  "status": "Not Started",
  "priority": "Normal",
  "dueDate": "2026-01-25",
  "assignedTo": "usr_abc123",
  "relatedTo": {
    "type": "opportunity",
    "id": "opp_xyz789"
  }
}
```

### Update Task

```http
PATCH /v1/tasks/{id}
```

### Complete Task

```http
POST /v1/tasks/{id}/complete
```

## Reports

### Generate Report

```http
POST /v1/reports/generate
```

**Request Body:**
```json
{
  "type": "pipeline",
  "filters": {
    "stage": ["Negotiation", "Proposal"],
    "closeDateFrom": "2026-01-01",
    "closeDateTo": "2026-03-31"
  },
  "groupBy": "stage",
  "format": "json"
}
```

**Response:**
```json
{
  "data": {
    "reportId": "rpt_abc123",
    "summary": {
      "totalOpportunities": 45,
      "totalValue": 2850000,
      "avgDealSize": 63333
    },
    "breakdown": [
      {
        "stage": "Negotiation",
        "count": 28,
        "value": 1750000
      },
      {
        "stage": "Proposal",
        "count": 17,
        "value": 1100000
      }
    ]
  }
}
```

### Get Report

```http
GET /v1/reports/{id}
```

### Export Report

```http
GET /v1/reports/{id}/export?format=csv
```

**Supported formats:**
- `json`
- `csv`
- `pdf`
- `xlsx`

## AI Assistant

### Query AI

```http
POST /v1/ai/query
```

**Request Body:**
```json
{
  "query": "Show me all opportunities closing this month over $50k",
  "context": {
    "userId": "usr_abc123",
    "conversationId": "conv_xyz789"
  }
}
```

**Response:**
```json
{
  "data": {
    "response": "I found 12 opportunities closing this month over $50k...",
    "results": [
      /* opportunity objects */
    ],
    "suggestions": [
      "Filter by industry",
      "Show probability breakdown",
      "Export to CSV"
    ]
  }
}
```

### Get Conversation History

```http
GET /v1/ai/conversations/{id}
```

## Analytics

### Get Dashboard Metrics

```http
GET /v1/analytics/dashboard
```

**Response:**
```json
{
  "data": {
    "revenue": {
      "actual": 2500000,
      "forecast": 3200000,
      "target": 3000000
    },
    "pipeline": {
      "total": 5200000,
      "weighted": 4160000,
      "deals": 127
    },
    "winRate": {
      "current": 35.5,
      "previous": 32.1,
      "change": 3.4
    }
  }
}
```

### Get Pipeline Metrics

```http
GET /v1/analytics/pipeline
```

### Get Team Performance

```http
GET /v1/analytics/team
```

## Batch Operations

### Batch Create

```http
POST /v1/batch/create
```

**Request Body:**
```json
{
  "resource": "contacts",
  "records": [
    {
      "firstName": "Alice",
      "lastName": "Johnson",
      "email": "alice@example.com"
    },
    {
      "firstName": "Bob",
      "lastName": "Williams",
      "email": "bob@example.com"
    }
  ]
}
```

### Batch Update

```http
POST /v1/batch/update
```

### Batch Delete

```http
POST /v1/batch/delete
```

## Error Responses

### Error Format

```json
{
  "error": {
    "code": "validation_error",
    "message": "Invalid request parameters",
    "details": [
      {
        "field": "amount",
        "message": "Amount must be a positive number"
      }
    ]
  }
}
```

### HTTP Status Codes

| Code | Meaning |
|------|---------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 429 | Rate Limit Exceeded |
| 500 | Internal Server Error |

## Rate Limiting

See [Authentication](authentication.md#rate-limiting) for rate limit details.

## SDK Support

Official SDKs available:

```bash
# JavaScript/Node.js
npm install @scopien/sdk

# Python
pip install scopien-sdk

# Ruby
gem install scopien

# PHP
composer require scopien/sdk
```

**Example (JavaScript):**
```javascript
import Scopien from '@scopien/sdk';

const client = new Scopien({
  apiKey: process.env.SCOPIEN_API_KEY
});

const opportunities = await client.opportunities.list({
  stage: 'Negotiation',
  minAmount: 50000
});
```

## Next Steps

- [Learn about Webhooks →](webhooks.md)
- [View Authentication Guide →](authentication.md)
- [Explore SDK Documentation →](https://github.com/scopien/sdk)

---

**API Questions?** Visit our [API Forum](https://community.scopien.com/api) or contact api@scopien.com
