---
title: "Basic Pen Tests (Future)"
date: 2025-10-08
draft: false
tags: ["pentesting", "testing"]
---

```markdown
## Test Case 1: Credential Extraction (n8n)
**Objective**: Verify credentials cannot be extracted from platform

Steps:
1. Authenticate as low-privilege user
2. Attempt database file access via path traversal
3. Try to export workflows with embedded credentials
4. Check if API tokens visible in logs
5. Attempt to escalate privileges via mass assignment

Expected: All attempts blocked, logged, and alerted

## Test Case 2: Authentication Bypass (both platforms)
**Objective**: Verify all authentication mechanisms functional

Steps:
1. Access API endpoints without auth
2. Test case manipulation bypasses
3. Try SQL injection in login forms
4. Test session fixation
5. Verify MFA cannot be bypassed

Expected: All access denied without valid authentication

## Test Case 3: Lateral Movement Prevention
**Objective**: Verify network isolation limits blast radius

Steps:
1. Gain shell access on platform container (simulated compromise)
2. Attempt to connect to production database directly
3. Try to access AWS metadata service
4. Scan internal network from compromised container
5. Attempt outbound connections to C2 servers

Expected: Network policies block all lateral movement

## Test Case 4: Secrets Exfiltration
**Objective**: Verify secrets protection mechanisms

Steps:
1. Export all workflows
2. Search exports for plaintext credentials
3. Dump application database
4. Check for secrets in logs
5. Review workflow execution history

Expected: No plaintext secrets found anywhere
```
