## **THE HARSH REALITIES (What Often Gets Skipped)**

### **Self-Hosted: Common Shortcuts That Create Risk**

```yaml
❌ WHAT TEAMS SKIP (Then Regret Later):
    
  "the N8n Db is fine for now":
    - File-based database = single file to exfiltrate
    - No encryption at rest
    - Easy to compromise via path traversal
    - Contains ALL credentials and workflows
    
  "We don't need network segmentation yet":
    - Platform can directly access production
    - Compromise = immediate prod access
    - No blast radius containment
    
  "WAF is too complex right now":
    - Known attacks succeed (path traversal, injection)
    - No rate limiting
    - DDoS takes down service
    
  "We'll set up monitoring later":
    - Breach goes undetected for months
    - No forensic evidence
    - Can't determine blast radius
    
  "Encryption key? We used 'changeme'":
    - All credentials easily decrypted
    - Data breach becomes credential breach
    - Every integrated system compromised
```

### **SaaS: Common Shortcuts That Create Risk**

```yaml
❌ WHAT TEAMS SKIP (Then Regret Later):
  
  "We don't need SSO/MFA":
    - Users share passwords
    - Password reuse from personal accounts
    - Phishing attack succeeds
    - Cost: $150k+ incident response
    
  "Basic tier is good enough":
    - No audit logs = can't detect compromise
    - No IP restrictions = accessible from anywhere
    - No SSO = password management nightmare
    - Compliance audit fails
    
  "We'll store creds in workflow fields":
    - Credentials in plaintext in exports
    - Visible to anyone with workflow access
    - Logs contain credentials
    - Backup contains credentials
    
  "We don't need testing/staging":
    - Prod workflows break during changes
    - Security issues go undetected
    - No safe place to test
    
  "Vendor says they're secure, that's enough":
    - No SOC 2 review
    - Vendor has undisclosed breach
    - Your data exposed
    - No recourse in contract
    
  "We'll export workflows... eventually":
    - Vendor goes bankrupt
    - Locked out of platform
    - Weeks to rebuild in alternative
    - Business disruption
```

---
