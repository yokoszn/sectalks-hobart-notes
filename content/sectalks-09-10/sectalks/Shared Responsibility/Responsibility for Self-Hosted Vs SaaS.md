
**Shared Responsibility: Cloud Self-Hosted**
```
┌─────────────────────────────────────────────────────────┐
│         CLOUD PROVIDER (AWS/Azure/GCP/DO/Hostinger)     │
│         THEY MANAGE (Infrastructure Layer)              │
├─────────────────────────────────────────────────────────┤
│  ✓ Physical security (data centers)                     │
│  ✓ Power, cooling, hardware                             │
│  ✓ Network infrastructure                               │
│  ✓ Hypervisor security                                  │
│  ✓ DDoS protection (basic/standard tier)                │
│  ✓ Physical network security                            │
│  ✓ Compliance (their infrastructure)                    │
└─────────────────────────────────────────────────────────┘
                           ↕
┌─────────────────────────────────────────────────────────┐
│         YOU MANAGE (Everything Above VM)                │
├─────────────────────────────────────────────────────────┤
│  ✗ Virtual machines / containers                        │
│  ✗ Operating system security                            │
│  ✗ OS patches and updates                               │
│  ✗ Application installation                             │
│  ✗ Application configuration                            │
│  ✗ Application security                                 │
│  ✗ Network security groups / firewall rules             │
│  ✗ Load balancers                                       │
│  ✗ SSL/TLS certificates                                 │
│  ✗ Database setup and security                          │
│  ✗ Backup configuration                                 │
│  ✗ Monitoring and alerting                              │
│  ✗ Access control (IAM)                                 │
│  ✗ Compliance (your use of platform)                    │
│  ✗ ALL application-layer security                       │
└─────────────────────────────────────────────────────────┘

KEY INSIGHT: Cloud provider gives you a secure empty VM.
             Everything else is YOUR job.
```
### **INFRASTRUCTURE & PLATFORM SECURITY**

| Security Control           | SelfHosted | SaaS       | Notes                                         |
| -------------------------- | ---------- | ---------- | --------------------------------------------- |
| **Server hardening**       | ✅ YOU      | ❌ Vendor   | OS patching, kernel updates, security configs |
| **Container security**     | ✅ YOU      | ❌ Vendor   | Docker hardening, image scanning              |
| **Network segmentation**   | ✅ YOU      | ❌ Vendor   | VPC, subnets, firewall rules                  |
| **DDoS protection**        | ✅ YOU      | ❌ Vendor   | CloudFlare, AWS Shield, etc.                  |
| **WAF configuration**      | ✅ YOU      | ❌ Vendor   | ModSecurity, AWS WAF rules                    |
| **SSL/TLS management**     | ✅ YOU      | ❌ Vendor   | Certificate rotation, cipher config           |
| **Load balancing**         | ✅ YOU      | ❌ Vendor   | High availability setup                       |
| **Database hardening**     | ✅ YOU      | ❌ Vendor   | PostgreSQL security, encryption               |
| **Backup infrastructure**  | ✅ YOU      | ❌ Vendor   | Backup systems, retention, testing            |
| **Disaster recovery**      | ✅ YOU      | ❌ Vendor   | DR planning, failover testing                 |
| **Platform CVE patching**  | ✅ YOU      | ❌ Vendor   | Applying security updates                     |
| **Vulnerability scanning** | ✅ YOU      | ⚠️ Limited | You scan workflows, vendor scans infra        |
| **Physical security**      | ✅ YOU      | ❌ Vendor   | Data center access control                    |
| **Power/cooling**          | ✅ YOU      | ❌ Vendor   | Infrastructure reliability                    |


---

### **AUTHENTICATION & ACCESS CONTROL**

| Security Control             | SelfHosted   | SaaS         | Notes                                            |
| ---------------------------- | ------------ | ------------ | ------------------------------------------------ |
| **SSO/SAML integration**     | ✅ YOU        | ✅ YOU        | Same config effort, but vendor provides endpoint |
| **MFA enforcement**          | ✅ YOU        | ✅ YOU        | Must configure & enforce in both cases           |
| **Password policies**        | ✅ YOU        | ✅ YOU        | Complexity, rotation, history                    |
| **Session management**       | ⚠️ Configure | ⚠️ Configure | Timeout values, concurrent sessions              |
| **User provisioning**        | ✅ YOU        | ✅ YOU        | Adding/removing users                            |
| **Role-based access (RBAC)** | ✅ YOU        | ✅ YOU        | Defining and assigning roles                     |
| **Least privilege design**   | ✅ YOU        | ✅ YOU        | Ensuring minimal permissions                     |
| **Admin account security**   | ✅ YOU        | ✅ YOU        | Separate admin accounts, audit                   |
| **Service accounts**         | ✅ YOU        | ✅ YOU        | For automation/integrations                      |
| **Access reviews**           | ✅ YOU        | ✅ YOU        | Quarterly user access audits                     |
| **Failed login monitoring**  | ✅ YOU        | ⚠️ YOU       | Easier with SaaS (built-in logs)                 |
| **Account lockout policies** | ⚠️ Configure | ⚠️ Configure | After failed attempts                            |
| **IP whitelisting**          | ✅ YOU        | ⚠️ Limited   | Easier self-hosted, tier-dependent SaaS          |
| **API key management**       | ✅ YOU        | ✅ YOU        | Generation, rotation, revocation                 |

**Key Point**: Almost NO difference - you own this in both cases

---

### **CREDENTIAL & SECRETS MANAGEMENT**

|Security Control|Self-Hosted|SaaS|Notes|
|---|---|---|---|
|**Credential storage**|✅ YOU|⚠️ Shared|Self-hosted: your vault. SaaS: vendor's vault|
|**Encryption key management**|✅ YOU|❌ Vendor|CMEK available in enterprise SaaS tiers|
|**Secrets vault integration**|✅ YOU|⚠️ Limited|HashiCorp Vault easier self-hosted|
|**Credential rotation**|✅ YOU|✅ YOU|Same process in both|
|**Credential inventory**|✅ YOU|✅ YOU|Documenting what's stored where|
|**Least privilege credentials**|✅ YOU|✅ YOU|Read-only where possible|
|**Credential access audit**|✅ YOU|✅ YOU|Who accessed what, when|
|**Hardcoded secret prevention**|✅ YOU|✅ YOU|Code review, scanning|
|**Time-limited tokens**|⚠️ Implement|⚠️ Implement|Short-lived access tokens|
|**Credential sharing policies**|✅ YOU|✅ YOU|Who can share, how|
|**Emergency credential revocation**|✅ YOU|✅ YOU|Incident response procedure|

**Key Point**: ZERO difference - you own credential security completely

---

### **WORKFLOW & APPLICATION SECURITY**

|Security Control|Self-Hosted|SaaS|Notes|
|---|---|---|---|
|**Workflow code review**|✅ YOU|✅ YOU|Security review before production|
|**Input validation**|✅ YOU|✅ YOU|Sanitizing user inputs|
|**Output sanitization**|✅ YOU|✅ YOU|Preventing injection attacks|
|**Error handling**|✅ YOU|✅ YOU|Proper error messages, no info leak|
|**Workflow approval process**|✅ YOU|✅ YOU|Production deployment gates|
|**Version control**|✅ YOU|⚠️ YOU|Easier with self-hosted Git integration|
|**Testing environment**|✅ YOU|⚠️ YOU|May need separate SaaS account|
|**Workflow documentation**|✅ YOU|✅ YOU|Documenting business logic|
|**Dependency management**|✅ YOU|❌ Vendor|Managing libraries, node modules|
|**API security**|✅ YOU|⚠️ Shared|Rate limiting, auth on endpoints|
|**Injection prevention**|✅ YOU|✅ YOU|SQL injection, command injection|
|**Data validation**|✅ YOU|✅ YOU|Type checking, range validation|
|**Sensitive data handling**|✅ YOU|✅ YOU|Masking, encryption, deletion|
|**Third-party integrations**|✅ YOU|✅ YOU|OAuth scope, API permissions|

**Key Point**: Almost identical - application security is YOUR job

---

### **MONITORING, LOGGING & INCIDENT RESPONSE**

|Security Control|Self-Hosted|SaaS|Notes|
|---|---|---|---|
|**Audit logging enabled**|⚠️ Configure|⚠️ Configure|Must enable in both|
|**Log export to SIEM**|✅ YOU|⚠️ YOU|Easier self-hosted, SaaS tier-dependent|
|**Log retention policy**|✅ YOU|⚠️ Vendor|SaaS retention limited by tier|
|**Security event alerting**|✅ YOU|⚠️ YOU|Custom alerts easier self-hosted|
|**Failed login alerts**|✅ YOU|⚠️ Built-in|Often included in SaaS|
|**Privilege escalation alerts**|✅ YOU|⚠️ Limited|Better visibility self-hosted|
|**Unusual activity detection**|✅ YOU|⚠️ Limited|Baseline detection|
|**Infrastructure monitoring**|✅ YOU|❌ Vendor|CPU, memory, disk - vendor's job in SaaS|
|**Application monitoring**|✅ YOU|⚠️ Shared|Workflow performance|
|**Incident response plan**|✅ YOU|✅ YOU|Your data = your incident|
|**Forensic evidence collection**|✅ YOU|⚠️ Limited|Less visibility in SaaS|
|**Security metrics/KPIs**|✅ YOU|⚠️ Limited|Dashboards from available logs|
|**Compliance reporting**|✅ YOU|⚠️ Shared|Vendor provides some reports|

**Key Point**: Partial reduction - you still need to monitor and respond

---

### **COMPLIANCE & GOVERNANCE**

|Security Control|Self-Hosted|SaaS|Notes|
|---|---|---|---|
|**Data classification**|✅ YOU|✅ YOU|Identifying sensitive data|
|**Privacy impact assessment**|✅ YOU|✅ YOU|GDPR, CCPA requirements|
|**Data processing agreement**|❌ N/A|✅ YOU|DPA with SaaS vendor|
|**Data residency**|✅ YOU|⚠️ Vendor|Self-hosted = full control, SaaS = region choice|
|**Right to audit vendor**|❌ N/A|⚠️ Contract|Negotiate in SaaS contract|
|**Compliance certifications**|✅ YOU|⚠️ Vendor|You get audited vs inheriting vendor's|
|**Data subject requests**|✅ YOU|⚠️ Shared|GDPR deletion, export|
|**Breach notification**|✅ YOU|⚠️ Shared|72-hour rule applies to both|
|**Data retention policies**|✅ YOU|⚠️ Configure|Set in both, easier self-hosted|
|**Audit trail completeness**|✅ YOU|⚠️ Limited|More complete self-hosted|
|**Third-party risk assessment**|⚠️ Integrations|✅ Vendor + Integrations|Add vendor to risk register|
|**Vendor security assessment**|❌ N/A|✅ YOU|Must assess SaaS vendor|
|**Exit strategy/data portability**|❌ N/A|✅ YOU|Must plan for SaaS migration|



---

### **OPERATIONAL SECURITY**

|Security Control|Self-Hosted|SaaS|Notes|
|---|---|---|---|
|**Change management**|✅ YOU|⚠️ Shared|Workflow changes vs platform changes|
|**Patch management**|✅ YOU|❌ Vendor|Security updates|
|**Vulnerability scanning**|✅ YOU|❌ Vendor|Platform vulnerabilities|
|**Penetration testing**|✅ YOU|⚠️ Vendor|Vendor tests platform, you test workflows|
|**Security training**|✅ YOU|✅ YOU|User awareness programs|
|**Acceptable use policy**|✅ YOU|✅ YOU|What users can/can't do|
|**Secure development practices**|✅ YOU|✅ YOU|For workflow development|
|**Code review process**|✅ YOU|✅ YOU|Before production deployment|
|**Secrets in version control**|✅ YOU|✅ YOU|Prevention, scanning|
|**Dependency vulnerabilities**|✅ YOU|❌ Vendor|Libraries, packages|
|**Configuration drift**|✅ YOU|❌ Vendor|Infrastructure as code|
|**Capacity planning**|✅ YOU|❌ Vendor|Scaling resources|
|**Performance monitoring**|✅ YOU|⚠️ Shared|Workflow optimization|
|**Cost monitoring**|✅ YOU|✅ YOU|Resource usage, billing alerts|


