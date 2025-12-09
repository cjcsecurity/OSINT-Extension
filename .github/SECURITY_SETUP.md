# Security Setup Instructions

This document provides step-by-step instructions for enabling critical security features for the OSINT-Extension repository.

## Overview

The following security workflows have been implemented in this repository:
- **CodeQL Security Scanning** - Automated code analysis for security vulnerabilities
- **Dependency Review** - Checks for vulnerable dependencies and license compliance
- **Security Scan** - NPM audit and manifest validation

Additionally, manual configuration is required in GitHub repository settings to enable:
- Secret scanning with push protection
- Private vulnerability reporting

---

## 1. Enable Secret Scanning with Push Protection

Secret scanning helps prevent sensitive information (API keys, tokens, passwords) from being committed to the repository.

### Steps:

1. Navigate to your repository on GitHub
2. Click **Settings** (requires admin permissions)
3. In the left sidebar, click **Code security and analysis**
4. Locate the **Secret scanning** section
5. Click **Enable** next to "Secret scanning"
6. Click **Enable** next to "Push protection"
   - This will block commits containing detected secrets
   - Contributors will receive an alert before pushing

### What it does:
- Scans commits for known secret patterns (API keys, tokens, etc.)
- Blocks pushes containing secrets when push protection is enabled
- Sends alerts to repository administrators when secrets are detected

---

## 2. Enable CodeQL Code Scanning (Automated)

CodeQL code scanning is now **automatically enabled** through the workflow files added to this repository.

### What's Already Configured:

The `.github/workflows/codeql.yml` workflow file provides:
- Automated scanning on every push to `main` branch
- Scanning on all pull requests
- Weekly scheduled scans (every Monday at midnight UTC)
- Security and quality queries for JavaScript
- Results visible in the **Security** tab under **Code scanning alerts**

### Verify it's working:

1. Go to your repository on GitHub
2. Click the **Security** tab
3. Click **Code scanning** in the left sidebar
4. You should see CodeQL analysis results after the workflow runs

**Note:** The first scan will run automatically when you push these workflow files to the `main` branch.

---

## 3. Enable Private Vulnerability Reporting

Private vulnerability reporting allows security researchers to privately report security issues to you before they are publicly disclosed.

### Steps:

1. Navigate to your repository on GitHub
2. Click **Settings** (requires admin permissions)
3. In the left sidebar, click **Code security and analysis**
4. Locate the **Private vulnerability reporting** section
5. Click **Enable** next to "Private vulnerability reporting"

### What it does:
- Creates a private communication channel for security researchers
- Allows vulnerabilities to be reported without public disclosure
- Enables you to work on fixes before public announcement
- Integrates with GitHub Security Advisories

### After Enabling:

1. Security researchers can report vulnerabilities via the **Security** tab
2. You'll receive notifications of new reports
3. You can collaborate privately with reporters on fixes
4. You can create Security Advisories for coordinated disclosure

---

## 4. Review Dependabot Configuration

The repository already has a `dependabot.yml` configuration file. Ensure Dependabot is enabled:

### Steps:

1. Navigate to your repository on GitHub
2. Click **Settings** (requires admin permissions)
3. In the left sidebar, click **Code security and analysis**
4. Locate the **Dependabot** section
5. Ensure the following are **Enabled**:
   - Dependabot alerts
   - Dependabot security updates

### What it does:
- Automatically detects vulnerable dependencies
- Creates pull requests to update vulnerable packages
- Works with the dependency review workflow for comprehensive protection

---

## 5. Content Security Policy (CSP)

A strict Content Security Policy has been added to `manifest.json`:

```json
"content_security_policy": {
  "extension_pages": "default-src 'self'; style-src 'self' 'unsafe-inline'; script-src 'self'; object-src 'none'; base-uri 'self'; form-action 'none'"
}
```

### What this prevents:
- Loading external scripts (XSS protection)
- Loading external resources except styles
- Inline script execution (except from extension files)
- Form submissions to external domains
- Object/embed tags

### Important Notes:
- All JavaScript must be in separate `.js` files
- No inline `<script>` tags in HTML files
- External resources should be loaded through extension APIs, not directly

---

## 6. Workflow Summaries

### CodeQL Security Scanning (`codeql.yml`)
- **Triggers:** Push to main, pull requests, weekly on Mondays
- **Scans:** JavaScript code for security vulnerabilities and code quality issues
- **Permissions:** Read actions/contents, write security-events

### Dependency Review (`dependency-review.yml`)
- **Triggers:** Pull requests to main
- **Checks:** Dependency vulnerabilities, license compliance
- **Fails on:** Moderate+ severity vulnerabilities, GPL-3.0/AGPL-3.0 licenses

### Security Scan (`security-scan.yml`)
- **Triggers:** Push to main, pull requests
- **Jobs:**
  - NPM Audit: Checks for vulnerable npm packages
  - Manifest Validation: Checks for dangerous permissions and inline scripts

---

## Verification Checklist

After completing the setup, verify all security features are active:

- [ ] Secret scanning is enabled
- [ ] Push protection for secrets is enabled
- [ ] CodeQL workflow has run successfully (check Actions tab)
- [ ] Private vulnerability reporting is enabled
- [ ] Dependabot alerts are enabled
- [ ] Dependabot security updates are enabled
- [ ] Security tab shows code scanning results
- [ ] All workflow files are present in `.github/workflows/`

---

## Monitoring and Maintenance

### Regular Tasks:

1. **Review Security Alerts** - Check the Security tab weekly for:
   - CodeQL code scanning alerts
   - Dependabot alerts
   - Secret scanning alerts

2. **Review Pull Requests** - Ensure:
   - Dependency review passes
   - Security scans pass
   - CodeQL analysis completes without new issues

3. **Update Dependencies** - Regularly merge Dependabot PRs for:
   - Security updates (high priority)
   - Version updates (as needed)

4. **Review Permissions** - Periodically check `manifest.json` for:
   - Unnecessary permissions
   - Scope creep in permission requests

---

## Getting Help

- **GitHub Documentation:** https://docs.github.com/en/code-security
- **CodeQL Documentation:** https://codeql.github.com/docs/
- **Dependabot Documentation:** https://docs.github.com/en/code-security/dependabot

For questions specific to this repository, open an issue or contact the maintainers.
