---
title: Security Best Practices
description: Security guidelines and best practices for Scopien AI
---

# Security Best Practices

Comprehensive security guidelines to keep your Scopien AI implementation secure.

## Security Overview

Scopien AI implements multiple layers of security:

```
User Layer          → MFA, SSO, RBAC
Application Layer   → OAuth 2.0, API Keys, Session Management
Transport Layer     → TLS 1.3, Certificate Pinning
Data Layer          → AES-256 Encryption, Key Rotation
Infrastructure      → SOC 2, HIPAA, ISO 27001
```

## Authentication Security

### Multi-Factor Authentication (MFA)

**Enable MFA for all users:**

1. Go to **Settings → Security → MFA**
2. Choose MFA method:
   - Authenticator App (recommended)
   - SMS
   - Hardware Token (U2F)
3. Scan QR code or enter key
4. Verify with test code
5. Save backup codes securely

**MFA Enforcement:**
```json
{
  "security": {
    "mfa": {
      "required": true,
      "methods": ["totp", "sms", "u2f"],
      "gracePeriod": "7 days"
    }
  }
}
```

### Single Sign-On (SSO)

**Configure SSO:**

```javascript
{
  "sso": {
    "provider": "okta",  // or "azure", "google", "saml"
    "domain": "company.okta.com",
    "clientId": "your_client_id",
    "enforceSSO": true,
    "justInTimeProvisioning": true
  }
}
```

**Benefits:**
- Centralized authentication
- Reduced password fatigue
- Better audit trails
- Faster onboarding/offboarding

### Password Policies

**Strong password requirements:**

```
Minimum length: 12 characters
Requirements:
  ✅ Uppercase letters
  ✅ Lowercase letters
  ✅ Numbers
  ✅ Special characters
  ✅ No common patterns
  ✅ No personal information

Password rotation: 90 days
Password history: Last 12 passwords
```

## API Security

### API Key Management

**Best Practices:**

```javascript
// ✅ Use environment variables
const API_KEY = process.env.SCOPIEN_API_KEY;

// ✅ Rotate keys regularly
const KEY_ROTATION_DAYS = 90;

// ✅ Use different keys per environment
const PROD_KEY = process.env.SCOPIEN_PROD_KEY;
const DEV_KEY = process.env.SCOPIEN_DEV_KEY;

// ❌ Never hardcode keys
const API_KEY = 'sk_live_abc123...';  // NEVER!

// ❌ Never commit keys to git
// Always add to .gitignore
```

**Key Rotation Process:**

```bash
# 1. Generate new key
curl -X POST https://api.scopien.com/v1/api-keys \
  -H "Authorization: Bearer ${CURRENT_KEY}" \
  -d '{"name": "Production Key v2"}'

# 2. Update application with new key
export SCOPIEN_API_KEY=sk_live_new123...

# 3. Test new key
curl https://api.scopien.com/v1/auth/test \
  -H "Authorization: Bearer ${SCOPIEN_API_KEY}"

# 4. Revoke old key
curl -X DELETE https://api.scopien.com/v1/api-keys/${OLD_KEY_ID} \
  -H "Authorization: Bearer ${SCOPIEN_API_KEY}"
```

### Rate Limiting

**Implement client-side rate limiting:**

```javascript
class RateLimiter {
  constructor(maxRequests, timeWindow) {
    this.maxRequests = maxRequests;
    this.timeWindow = timeWindow;
    this.requests = [];
  }

  async checkLimit() {
    const now = Date.now();
    this.requests = this.requests.filter(
      time => now - time < this.timeWindow
    );

    if (this.requests.length >= this.maxRequests) {
      const oldestRequest = this.requests[0];
      const waitTime = this.timeWindow - (now - oldestRequest);
      await sleep(waitTime);
      return this.checkLimit();
    }

    this.requests.push(now);
  }
}

// Usage
const limiter = new RateLimiter(100, 60000);  // 100 req/min

async function makeRequest(url) {
  await limiter.checkLimit();
  return fetch(url);
}
```

## Data Security

### Encryption

**Data at Rest:**
```
Algorithm: AES-256-GCM
Key Management: AWS KMS / Azure Key Vault
Key Rotation: Automatic, every 90 days
Backup Encryption: Same standards
```

**Data in Transit:**
```
Protocol: TLS 1.3
Cipher Suites: Only strong ciphers
Certificate: Let's Encrypt / DigiCert
HSTS: Enabled (max-age=31536000)
```

### Sensitive Data Handling

**Identify sensitive fields:**

```javascript
const SENSITIVE_FIELDS = [
  'ssn',
  'creditCardNumber',
  'bankAccount',
  'password',
  'apiKey',
  'secret'
];

function sanitizeLog(data) {
  const sanitized = { ...data };

  SENSITIVE_FIELDS.forEach(field => {
    if (sanitized[field]) {
      sanitized[field] = '***REDACTED***';
    }
  });

  return sanitized;
}

// Usage
console.log(sanitizeLog({
  name: 'John',
  email: 'john@example.com',
  ssn: '123-45-6789'  // Will be redacted
}));
```

**Field-Level Encryption:**

```javascript
const crypto = require('crypto');

class FieldEncryption {
  constructor(key) {
    this.key = Buffer.from(key, 'hex');
  }

  encrypt(text) {
    const iv = crypto.randomBytes(16);
    const cipher = crypto.createCipheriv('aes-256-gcm', this.key, iv);

    let encrypted = cipher.update(text, 'utf8', 'hex');
    encrypted += cipher.final('hex');

    const authTag = cipher.getAuthTag();

    return {
      iv: iv.toString('hex'),
      data: encrypted,
      authTag: authTag.toString('hex')
    };
  }

  decrypt(encrypted) {
    const decipher = crypto.createDecipheriv(
      'aes-256-gcm',
      this.key,
      Buffer.from(encrypted.iv, 'hex')
    );

    decipher.setAuthTag(Buffer.from(encrypted.authTag, 'hex'));

    let decrypted = decipher.update(encrypted.data, 'hex', 'utf8');
    decrypted += decipher.final('utf8');

    return decrypted;
  }
}
```

## Access Control

### Role-Based Access Control (RBAC)

**Define roles:**

```javascript
const ROLES = {
  ADMIN: {
    name: 'Administrator',
    permissions: ['*']  // All permissions
  },
  MANAGER: {
    name: 'Manager',
    permissions: [
      'read:all',
      'write:team',
      'delete:own',
      'manage:team'
    ]
  },
  USER: {
    name: 'User',
    permissions: [
      'read:own',
      'write:own',
      'delete:own'
    ]
  },
  GUEST: {
    name: 'Guest',
    permissions: [
      'read:public'
    ]
  }
};
```

**Implement permission checks:**

```javascript
function checkPermission(user, action, resource) {
  const role = ROLES[user.role];

  // Check wildcard permission
  if (role.permissions.includes('*')) {
    return true;
  }

  // Check specific permission
  const requiredPermission = `${action}:${resource}`;
  if (role.permissions.includes(requiredPermission)) {
    return true;
  }

  // Check action:all permission
  if (role.permissions.includes(`${action}:all`)) {
    return true;
  }

  return false;
}

// Usage
if (checkPermission(user, 'delete', 'opportunity')) {
  // Allow deletion
} else {
  throw new Error('Insufficient permissions');
}
```

### IP Allowlisting

**Configure IP restrictions:**

```json
{
  "security": {
    "ipAllowlist": {
      "enabled": true,
      "ips": [
        "203.0.113.0/24",
        "198.51.100.42"
      ],
      "allowInternalIPs": false
    }
  }
}
```

**Implement IP validation:**

```javascript
const ipRangeCheck = require('ip-range-check');

function validateIP(clientIP, allowedRanges) {
  return allowedRanges.some(range =>
    ipRangeCheck(clientIP, range)
  );
}

// Middleware
app.use((req, res, next) => {
  const clientIP = req.ip;
  const allowed = validateIP(clientIP, ALLOWED_IPS);

  if (!allowed) {
    return res.status(403).json({
      error: 'Access denied from this IP'
    });
  }

  next();
});
```

## Audit & Monitoring

### Activity Logging

**Log all security-relevant events:**

```javascript
const LOG_EVENTS = [
  'user.login',
  'user.logout',
  'user.failed_login',
  'user.password_changed',
  'user.mfa_enabled',
  'api_key.created',
  'api_key.revoked',
  'permission.changed',
  'data.accessed',
  'data.modified',
  'data.deleted'
];

function logSecurityEvent(event, details) {
  const log = {
    timestamp: new Date().toISOString(),
    event: event,
    userId: details.userId,
    ip: details.ip,
    userAgent: details.userAgent,
    resource: details.resource,
    action: details.action,
    result: details.result
  };

  // Send to logging service
  logger.security(log);

  // Alert on critical events
  if (isCriticalEvent(event)) {
    alertSecurityTeam(log);
  }
}
```

**Audit trail example:**

```
Security Audit Log
────────────────────────────────────────────
2026-01-22 10:45:23 | user.login
  User: john@acme.com
  IP: 203.0.113.42
  Result: SUCCESS
  MFA: Verified

2026-01-22 10:30:15 | data.accessed
  User: sarah@acme.com
  Resource: opportunities
  Action: LIST
  Count: 125 records

2026-01-22 09:15:08 | api_key.created
  User: admin@acme.com
  Key: sk_live_***abc123
  Scopes: read, write
  IP: 198.51.100.10
```

### Anomaly Detection

**Detect suspicious activity:**

```javascript
class AnomalyDetector {
  constructor() {
    this.thresholds = {
      failedLogins: 5,
      apiCalls: 1000,
      dataExports: 3
    };
  }

  async checkFailedLogins(userId) {
    const count = await getFailedLoginCount(userId, '1 hour');

    if (count >= this.thresholds.failedLogins) {
      await this.triggerAlert({
        type: 'suspicious_activity',
        reason: 'excessive_failed_logins',
        userId: userId,
        count: count
      });
    }
  }

  async checkAPIUsage(apiKey) {
    const calls = await getAPICallCount(apiKey, '1 hour');

    if (calls >= this.thresholds.apiCalls) {
      await this.triggerAlert({
        type: 'unusual_api_usage',
        apiKey: apiKey,
        calls: calls
      });
    }
  }

  async triggerAlert(details) {
    // Notify security team
    await notifySecurityTeam(details);

    // Optionally lock account
    if (details.type === 'suspicious_activity') {
      await lockAccount(details.userId);
    }
  }
}
```

## Incident Response

### Security Incident Plan

**1. Detection**
```
• Monitor alerts
• Review logs
• User reports
• System anomalies
```

**2. Containment**
```
• Isolate affected systems
• Revoke compromised credentials
• Block suspicious IPs
• Disable affected features
```

**3. Investigation**
```
• Collect evidence
• Analyze logs
• Identify root cause
• Assess impact
```

**4. Remediation**
```
• Patch vulnerabilities
• Update credentials
• Apply security fixes
• Restore from backup if needed
```

**5. Recovery**
```
• Restore services
• Monitor for recurrence
• Update security measures
• Document lessons learned
```

**6. Communication**
```
• Notify affected users
• Report to authorities if required
• Update security documentation
• Conduct post-mortem
```

### Breach Notification

**Required notifications:**

```javascript
const BREACH_NOTIFICATION = {
  threshold: {
    recordCount: 500,
    sensitiveData: true
  },
  timeline: {
    internalNotification: '1 hour',
    customerNotification: '72 hours',
    regulatoryNotification: '72 hours'
  },
  recipients: [
    'security@company.com',
    'legal@company.com',
    'ciso@company.com'
  ]
};
```

## Compliance

### SOC 2 Type II

**Key controls:**
- Access controls
- Encryption
- Change management
- Backup & recovery
- Incident response
- Vendor management

### GDPR Compliance

**Data subject rights:**
```javascript
// Right to access
GET /v1/gdpr/data-export

// Right to erasure
DELETE /v1/gdpr/delete-account

// Right to portability
GET /v1/gdpr/data-export?format=json

// Right to rectification
PATCH /v1/users/{id}/personal-data
```

### HIPAA (Healthcare)

**Required for PHI:**
- Encryption at rest and in transit
- Access controls and audit logs
- Business Associate Agreement (BAA)
- Regular risk assessments
- Incident response procedures

## Security Checklist

### Development
- [ ] Use parameterized queries (prevent SQL injection)
- [ ] Validate all user input
- [ ] Implement CSRF protection
- [ ] Use secure session management
- [ ] Keep dependencies updated
- [ ] Run security scans (SAST/DAST)
- [ ] Code review for security

### Deployment
- [ ] Use HTTPS everywhere
- [ ] Configure security headers
- [ ] Disable unnecessary services
- [ ] Apply least privilege principle
- [ ] Enable audit logging
- [ ] Set up monitoring and alerts
- [ ] Regular security updates

### Operations
- [ ] Rotate credentials regularly
- [ ] Review access permissions
- [ ] Monitor security logs
- [ ] Conduct security training
- [ ] Perform security audits
- [ ] Test incident response
- [ ] Update security documentation

## Security Resources

### Reporting Security Issues

**Contact:**
- Email: security@scopien.com
- PGP Key: Available at /security/pgp-key
- Bug Bounty: https://scopien.com/security/bounty

**Response SLA:**
- Critical: 4 hours
- High: 24 hours
- Medium: 72 hours
- Low: 7 days

### Security Updates

Subscribe to security advisories:
- Email: security-announce@scopien.com
- RSS: https://scopien.com/security/feed
- Status Page: https://status.scopien.com

## Next Steps

- [Set Up Custom Workflows →](custom-workflows.md)
- [Review Troubleshooting Guide →](troubleshooting.md)
- [Learn about API Security →](../api-reference/authentication.md)

---

**Security Questions?** Contact security@scopien.com | Report vulnerabilities responsibly
