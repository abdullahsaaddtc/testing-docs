---
title: Installation Guide
description: Step-by-step guide to install and set up Scopien AI
---

# Installation Guide

This guide will walk you through the process of installing and setting up Scopien AI for your organization.

## Prerequisites

Before you begin, ensure you have:

- ✅ Salesforce org (Professional, Enterprise, or Unlimited edition)
- ✅ System Administrator access to Salesforce
- ✅ Modern web browser (Chrome, Firefox, Safari, or Edge)
- ✅ Valid email address for account creation

## Installation Methods

### Method 1: Cloud-Based (Recommended)

The fastest way to get started with Scopien AI.

#### Step 1: Create Your Account

1. Visit [https://app.scopien.com/signup](https://app.scopien.com/signup)
2. Fill in your details:
   ```
   - Full Name
   - Email Address
   - Company Name
   - Password (min 8 characters)
   ```
3. Click "Create Account"
4. Verify your email address

#### Step 2: Choose Your Plan

Select the plan that fits your needs:

| Plan | Users | Features | Price |
|------|-------|----------|-------|
| Starter | 1-5 | Basic AI, 10k requests/mo | $99/mo |
| Professional | 5-25 | Advanced AI, 50k requests/mo | $299/mo |
| Enterprise | Unlimited | Custom AI, Unlimited requests | Custom |

#### Step 3: Connect Salesforce

1. Click "Connect Salesforce" on your dashboard
2. Choose your Salesforce environment:
   - Production
   - Sandbox
3. Log in with your Salesforce credentials
4. Authorize Scopien AI permissions
5. Wait for initial data sync (2-5 minutes)

### Method 2: On-Premise Installation

For organizations requiring on-premise deployment.

#### System Requirements

**Server:**
- CPU: 4+ cores
- RAM: 16GB minimum
- Storage: 100GB SSD
- OS: Ubuntu 20.04+ or RHEL 8+

**Network:**
- Outbound HTTPS (443) to Salesforce
- Inbound HTTPS (443) for web access

#### Installation Steps

1. **Download Installation Package**
   ```bash
   wget https://downloads.scopien.com/enterprise/scopien-ai-latest.tar.gz
   tar -xzf scopien-ai-latest.tar.gz
   cd scopien-ai
   ```

2. **Configure Environment**
   ```bash
   cp .env.example .env
   nano .env
   ```

   Update these variables:
   ```env
   DATABASE_URL=postgresql://user:pass@localhost/scopien
   REDIS_URL=redis://localhost:6379
   SALESFORCE_CLIENT_ID=your_client_id
   SALESFORCE_CLIENT_SECRET=your_client_secret
   JWT_SECRET=your_random_secret
   ```

3. **Run Installation Script**
   ```bash
   sudo ./install.sh
   ```

4. **Start Services**
   ```bash
   sudo systemctl start scopien
   sudo systemctl enable scopien
   ```

5. **Verify Installation**
   ```bash
   curl http://localhost:3000/health
   ```

   Expected response:
   ```json
   {
     "status": "healthy",
     "version": "1.0.0",
     "services": {
       "database": "connected",
       "redis": "connected",
       "ai": "ready"
     }
   }
   ```

## Post-Installation Setup

### Configure SSO (Optional)

For enterprise deployments with Single Sign-On:

1. Navigate to Settings > Security
2. Click "Configure SSO"
3. Select your identity provider:
   - Okta
   - Azure AD
   - Google Workspace
   - Custom SAML 2.0
4. Follow provider-specific instructions
5. Test SSO connection

### Set Up Team Members

1. Go to Settings > Team
2. Click "Invite Member"
3. Enter email addresses
4. Assign roles:
   - **Admin**: Full access
   - **Manager**: View and edit
   - **User**: View only
5. Send invitations

### Configure Permissions

Define what Scopien AI can access in Salesforce:

1. Go to Settings > Salesforce Permissions
2. Select objects to enable:
   - [x] Accounts
   - [x] Contacts
   - [x] Opportunities
   - [x] Cases
   - [x] Leads
   - [ ] Custom Objects (optional)
3. Set field-level permissions
4. Save configuration

## Verification Checklist

After installation, verify everything is working:

- [ ] Can log in to Scopien AI dashboard
- [ ] Salesforce connection shows "Active"
- [ ] Can see Salesforce data in dashboard
- [ ] AI Assistant responds to queries
- [ ] Team members received invitations
- [ ] SSO login works (if configured)
- [ ] Mobile app connects successfully

## Troubleshooting

### Connection Issues

**Problem**: Cannot connect to Salesforce

**Solutions**:
1. Verify Salesforce credentials
2. Check IP whitelist in Salesforce
3. Ensure correct environment (Production vs Sandbox)
4. Review Connected App settings

### Authentication Errors

**Problem**: "Invalid credentials" error

**Solutions**:
1. Reset password and try again
2. Clear browser cache and cookies
3. Check if account is locked
4. Contact support if issue persists

### Sync Problems

**Problem**: Data not syncing from Salesforce

**Solutions**:
1. Check Salesforce API limits
2. Verify object permissions
3. Review sync logs in Settings > Logs
4. Manually trigger sync

## Next Steps

Now that Scopien AI is installed:

1. [Complete the Quick Start Guide →](quick-start.md)
2. [Learn about the Dashboard →](../user-guide/dashboard.md)
3. [Connect your first integration →](../user-guide/salesforce-integration.md)

## Support

Need help with installation?

- 📧 Email: support@scopien.com
- 💬 Live Chat: Available 24/7 in app
- 📞 Phone: +1 (800) SCOPIEN
- 📚 Knowledge Base: https://help.scopien.com

---

**Installation Time**: ~15 minutes for cloud, ~1 hour for on-premise
