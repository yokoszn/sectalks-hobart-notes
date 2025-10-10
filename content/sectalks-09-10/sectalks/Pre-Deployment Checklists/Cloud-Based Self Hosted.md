---
title: "Cloud-Based Self Hosted"
date: 2025-10-08
draft: false
tags: ["checklist", "cloud", "deployment"]
---

```yaml
═══════════════════════════════════════════════════════════════
PHASE 1: CLOUD INFRASTRUCTURE SETUP              ⏱️ 25-35 hours
═══════════════════════════════════════════════════════════════

Cloud Account Setup:
  [ ] Cloud provider account created (AWS/Azure/GCP/DO/Hostinger)
  [ ] Billing alerts configured
  [ ] Root account MFA enabled
  [ ] IAM users created (no root account usage)
  [ ] IAM roles configured (least privilege)
  [ ] Cloud audit logging enabled (CloudTrail/Activity Log)
  [ ] Cost allocation tags configured
  [ ] Budget limits set

Network Architecture:
  [ ] VPC/Virtual Network created
  [ ] Subnets created (public/private)
  [ ] Internet Gateway configured
  [ ] NAT Gateway configured (for private subnet internet access)
  [ ] Route tables configured
  [ ] Network ACLs configured
  [ ] Security groups created
    [ ] Application security group (port 443 only from internet)
    [ ] Database security group (port 5432 from app only)
    [ ] SSH bastion security group (port 22 from your IP only)
  [ ] VPC flow logs enabled
  [ ] DDoS protection enabled (basic tier minimum)
  
Compute Resources:
  [ ] VM/Instance created (right-sized for workload)
      Recommended minimum:
        - n8n: 2 vCPU, 4GB RAM ($20-40/month)
        - Flowwise: 2 vCPU, 4GB RAM ($20-40/month)
        - Tines alternative: 4 vCPU, 8GB RAM ($40-80/month)
  [ ] Instance in private subnet (if using bastion)
  [ ] Elastic IP / Reserved IP assigned (if needed)
  [ ] Storage configured (encrypted volumes)
  [ ] Automated snapshots configured
  [ ] SSH key pair created (password auth disabled)
  [ ] Instance metadata service secured (IMDSv2 on AWS)
  
Database Setup:
  [ ] Choose: Managed database vs self-managed
  
  IF MANAGED DATABASE (Recommended - AWS RDS/DigitalOcean Managed DB):
    [ ] PostgreSQL 15+ instance created
    [ ] Instance sized appropriately (db.t3.small minimum)
    [ ] Encryption at rest enabled
    [ ] Automated backups enabled (7-30 days)
    [ ] In private subnet
    [ ] Security group allows only app server
    [ ] Connection pooling configured
    [ ] Monitoring enabled
    [ ] Cost: +$15-30/month
    
  IF SELF-MANAGED DATABASE:
    [ ] PostgreSQL installed on separate VM
    [ ] Database hardening completed (see next section)
    [ ] Backups configured to object storage
    
Load Balancer (if needed for HA):
  [ ] Application Load Balancer created
  [ ] SSL certificate provisioned (ACM/Let's Encrypt)
  [ ] Health checks configured
  [ ] Target group created
  [ ] HTTPS listener configured (TLS 1.2+ only)
  [ ] HTTP → HTTPS redirect configured
  
Object Storage:
  [ ] S3 bucket / Object storage created (for backups)
  [ ] Bucket encryption enabled
  [ ] Versioning enabled
  [ ] Lifecycle policies configured
  [ ] Access logging enabled
  [ ] Bucket policy configured (private)
  
DNS & CDN:
  [ ] Domain registered / configured
  [ ] DNS records created
  [ ] CloudFlare configured (or equivalent)
    [ ] SSL/TLS mode: Full (strict)
    [ ] WAF enabled
    [ ] Rate limiting configured
    [ ] DDoS protection enabled (proxy mode)
  [ ] CAA records configured

═══════════════════════════════════════════════════════════════
PHASE 2: OPERATING SYSTEM HARDENING             ⏱️ 15-20 hours
═══════════════════════════════════════════════════════════════

OS Installation & Updates:
  [ ] Ubuntu 22.04 LTS / Debian 12 installed (or latest stable)
  [ ] System fully updated (apt update && apt upgrade)
  [ ] Unattended upgrades configured (automatic security patches)
  [ ] Reboot on kernel update configured
  [ ] Package sources verified (official repos only)
  
User & Access Management:
  [ ] Root login disabled via SSH
  [ ] Sudo user created for administration
  [ ] SSH key-based auth only (passwords disabled)
  [ ] SSH port changed from 22 (optional, security by obscurity)
  [ ] SSH config hardened:
    [ ] PermitRootLogin no
    [ ] PasswordAuthentication no
    [ ] PubkeyAuthentication yes
    [ ] MaxAuthTries 3
    [ ] ClientAliveInterval 300
    [ ] ClientAliveCountMax 2
  [ ] Fail2ban installed and configured
    [ ] SSH jail enabled
    [ ] HTTP jail configured
    [ ] Email alerts configured

Firewall Configuration:
  [ ] UFW (Uncomplicated Firewall) installed
  [ ] Default deny incoming, allow outgoing
  [ ] Allow SSH (from your IP only if possible)
  [ ] Allow HTTP (80) - for Let's Encrypt
  [ ] Allow HTTPS (443)
  [ ] UFW enabled and active
  [ ] iptables rules backed up
  
System Hardening:
  [ ] Unnecessary services disabled
    [ ] Avahi daemon disabled
    [ ] Bluetooth disabled
    [ ] Print services disabled
  [ ] Kernel parameters tuned (/etc/sysctl.conf):
    [ ] IPv4 forwarding disabled (unless needed)
    [ ] SYN cookies enabled
    [ ] ICMP redirects disabled
    [ ] Reverse path filtering enabled
  [ ] File system hardening:
    [ ] /tmp mounted with noexec,nosuid,nodev
    [ ] Separate partition for /var (if possible)
  [ ] Auditd installed for system auditing
  [ ] AppArmor / SELinux enabled
  
Security Monitoring:
  [ ] AIDE (file integrity monitoring) installed
  [ ] ClamAV antivirus installed (optional)
  [ ] rkhunter (rootkit scanner) installed
  [ ] Logwatch installed for daily email reports
  [ ] NTP configured (time synchronization)
  
System Logging:
  [ ] rsyslog configured
  [ ] Logs rotated (logrotate)
  [ ] Remote logging configured (to SIEM or S3)
  [ ] Log retention policy set (90 days minimum)

═══════════════════════════════════════════════════════════════
PHASE 3: CONTAINER & APPLICATION SETUP          ⏱️ 20-30 hours
═══════════════════════════════════════════════════════════════

Docker Installation & Hardening:
  [ ] Docker CE installed (latest stable)
  [ ] Docker daemon configured securely:
    [ ] User namespaces enabled
    [ ] Seccomp enabled
    [ ] AppArmor profile loaded
    [ ] Live restore enabled
    [ ] Log driver configured (json-file with rotation)
    [ ] Resource limits set (memory, CPU)
  [ ] Docker socket not exposed to network
  [ ] Non-root user added to docker group (admin user only)
  [ ] Docker Bench Security audit run (passing)
  
Container Security:
  [ ] All images from trusted registries only
  [ ] Images scanned with Trivy/Grype
  [ ] No :latest tags (pin specific versions)
  [ ] Regular image update schedule defined
  [ ] Container resource limits configured
  [ ] No privileged containers
  [ ] Read-only root filesystem where possible
  [ ] Capabilities dropped (cap_drop: ALL, then add specific)
  [ ] Security options configured (no-new-privileges)
  
Application Deployment (n8n example):
  [ ] docker-compose.yml created
  [ ] Environment file created (.env)
  [ ] Secrets NOT in .env (use Docker secrets or external vault)
  [ ] Encryption key generated (32+ random characters)
  [ ] Database connection string configured
  [ ] Strong admin password set (never default)
  [ ] SSL/TLS configured
  [ ] CORS configured restrictively
  [ ] API rate limiting enabled
  [ ] Webhook URLs configured (https only)
  [ ] Deployment tested (docker-compose up)
  [ ] Health check endpoint verified
  [ ] Logs written to stdout (captured by Docker)
  
Database Configuration (if self-managed):
  [ ] PostgreSQL installed in container or separate VM
  [ ] Data directory on persistent volume
  [ ] postgresql.conf hardened:
    [ ] SSL required (ssl = on)
    [ ] Listen on localhost only (or private network)
    [ ] max_connections tuned
    [ ] shared_buffers tuned
    [ ] work_mem tuned
  [ ] pg_hba.conf configured (no trust auth)
  [ ] Strong postgres password set
  [ ] Database user created (not postgres superuser)
  [ ] Database created with proper ownership
  [ ] Regular VACUUM scheduled
  [ ] Connection pooling (PgBouncer) configured

Reverse Proxy (Nginx/Caddy):
  [ ] Nginx installed (in container or on host)
  [ ] SSL certificate obtained (Certbot for Let's Encrypt)
  [ ] Auto-renewal configured (certbot renew)
  [ ] Nginx configuration hardened:
    [ ] TLS 1.2+ only (no TLS 1.0/1.1)
    [ ] Strong cipher suites only
    [ ] HSTS header enabled
    [ ] X-Frame-Options: DENY
    [ ] X-Content-Type-Options: nosniff
    [ ] X-XSS-Protection: 1; mode=block
    [ ] CSP header configured
  [ ] Rate limiting configured
  [ ] Request size limits set
  [ ] Proxy timeouts configured
  [ ] Access logs configured
  [ ] Error logs configured
  [ ] Fail2ban jail for nginx configured

═══════════════════════════════════════════════════════════════
PHASE 4: PLATFORM-SPECIFIC HARDENING           ⏱️ 15-25 hours
═══════════════════════════════════════════════════════════════

Application Configuration (n8n/Flowwise):
  [ ] Latest stable version deployed
  [ ] All environment variables configured
  [ ] Encryption key stored in secrets manager (AWS Secrets/Vault)
  [ ] Database encryption enabled
  [ ] External secrets manager integrated (if available)
  [ ] Debug mode DISABLED
  [ ] Error messages sanitized (no stack traces to users)
  [ ] File upload restrictions configured
  [ ] API authentication required
  [ ] Webhook signature verification enabled
  [ ] CORS policy restrictive
  [ ] Session timeout configured (1 hour maximum)
  [ ] Concurrent session limits
  [ ] Admin panel access restricted (IP whitelist if possible)
  
Credential Management:
  [ ] Platform credential vault configured
  [ ] No credentials hardcoded in workflows
  [ ] Credentials encrypted with strong key
  [ ] Credential rotation schedule defined
  [ ] Test credentials vs production separated
  [ ] Credential access logging enabled
  [ ] Read-only credentials used where possible
  
CVE Mitigation (if older version or patches pending):
  [ ] CVE-2023-27562 (n8n): Path traversal blocked at proxy
  [ ] CVE-2023-27564 (n8n): .svg auth bypass blocked
  [ ] CVE-2024-31621 (Flowwise): Case-bypass blocked
  [ ] CVE-2025-58434 (Flowwise): forgot-password endpoint blocked
  [ ] WAF rules configured for known vulnerabilities
  [ ] IDS/IPS signatures added

═══════════════════════════════════════════════════════════════
PHASE 5: AUTHENTICATION & ACCESS               ⏱️ 10-15 hours
═══════════════════════════════════════════════════════════════

[IDENTICAL TO SaaS - NO DIFFERENCE]
See previous checklist - same 30 items for:
  - SSO/SAML configuration
  - MFA enforcement
  - User roles and permissions
  - Service accounts
  - Access reviews

═══════════════════════════════════════════════════════════════
PHASE 6: MONITORING & LOGGING                  ⏱️ 20-30 hours
═══════════════════════════════════════════════════════════════

Application Monitoring:
  [ ] Prometheus installed (or cloud monitoring)
  [ ] Node exporter configured
  [ ] Container metrics collected
  [ ] Application metrics exposed (/metrics endpoint)
  [ ] Grafana dashboard configured
  [ ] Alerting rules configured
  [ ] Alert routing configured (email/Slack/PagerDuty)
  
System Monitoring:
  [ ] CPU utilization monitoring
  [ ] Memory utilization monitoring
  [ ] Disk space monitoring
  [ ] Network traffic monitoring
  [ ] SSL certificate expiration monitoring
  [ ] Service uptime monitoring
  [ ] External uptime monitoring (UptimeRobot/Pingdom)
  
Log Management:
  [ ] Application logs centralized
  [ ] System logs centralized
  [ ] Container logs centralized
  [ ] Database logs captured
  [ ] Nginx access/error logs captured
  [ ] WAF logs captured (if CloudFlare, via API)
  [ ] Logs shipped to SIEM or S3
  [ ] Log retention configured (1 year minimum)
  [ ] Log integrity protected
  [ ] PII scrubbing configured
  
Security Monitoring:
  [ ] Failed login attempts monitored
  [ ] Privilege escalation attempts monitored
  [ ] Unusual workflow execution patterns detected
  [ ] Credential access patterns monitored
  [ ] File integrity monitoring alerts
  [ ] Intrusion detection system configured
  [ ] Security events correlated
  [ ] Threat intelligence integrated (if available)

Alerting Configuration:
  [ ] Critical: Failed SSH login attempts (>3 in 10 min)
  [ ] Critical: Root command execution
  [ ] Critical: Database connection from unknown IP
  [ ] Critical: SSL certificate expires in <7 days
  [ ] High: Unusual API usage patterns
  [ ] High: High memory/CPU usage (>80% for 10 min)
  [ ] High: Disk space critical (<10% free)
  [ ] Medium: Failed application logins (>5 in 10 min)
  [ ] Medium: Unusual workflow executions
  [ ] Info: Backup completion
  [ ] Info: System updates available

═══════════════════════════════════════════════════════════════
PHASE 7: BACKUP & DISASTER RECOVERY            ⏱️ 12-16 hours
═══════════════════════════════════════════════════════════════

Backup Strategy:
  [ ] Database backups automated (daily)
  [ ] Database backup script:
    [ ] Dumps database to file
    [ ] Compresses backup
    [ ] Encrypts backup (gpg or AWS KMS)
    [ ] Uploads to S3/object storage
    [ ] Rotates old backups (30 days)
    [ ] Verifies backup integrity
    [ ] Logs backup status
    [ ] Alerts on failure
  [ ] Application data backed up (workflows, configs)
  [ ] Container volumes backed up
  [ ] Encryption keys backed up (offline/external)
  [ ] Backup stored in different region (geo-redundancy)
  [ ] Backup retention policy: 7 daily, 4 weekly, 12 monthly
  [ ] Backup restore procedure documented
  [ ] Backup restore TESTED (quarterly minimum)
  [ ] Backup monitoring configured
  
Disaster Recovery:
  [ ] DR plan documented
  [ ] RTO/RPO defined (e.g., 4 hours / 1 hour)
  [ ] Infrastructure as code (Terraform/CloudFormation)
  [ ] Application deployment automated (docker-compose)
  [ ] Database restore procedure documented
  [ ] SSL certificate backup/restore procedure
  [ ] DNS failover configured (if HA required)
  [ ] Runbook for common failures
  [ ] DR test scheduled (annually minimum)
  [ ] Contact list for incident response
  [ ] Escalation procedures defined

═══════════════════════════════════════════════════════════════
PHASE 8: SECURITY TESTING & VALIDATION        ⏱️ 10-15 hours
═══════════════════════════════════════════════════════════════

Automated Security Scanning:
  [ ] Container images scanned (Trivy)
  [ ] OS vulnerabilities scanned (Lynis)
  [ ] SSL/TLS tested (testssl.sh)
  [ ] Web application scanned (Nikto/Nuclei)
  [ ] OWASP ZAP baseline scan run
  [ ] Dependency vulnerabilities checked (npm audit)
  [ ] Security headers validated
  [ ] Port scan run (nmap) - only expected ports open
  [ ] Cloud security posture checked (ScoutSuite)
  
Manual Security Testing:
  [ ] Authentication bypass attempts (failed)
  [ ] Authorization bypass attempts (failed)
  [ ] SQL injection attempts (blocked)
  [ ] Command injection attempts (blocked)
  [ ] Path traversal attempts (blocked)
  [ ] XSS attempts (blocked)
  [ ] CSRF protection validated
  [ ] Rate limiting validated
  [ ] Session management tested
  [ ] Password policy enforced
  [ ] MFA cannot be bypassed
  [ ] Backup restore tested
  [ ] Disaster recovery tested
  
Compliance Validation:
  [ ] CIS benchmark audit run
  [ ] Docker bench security passed
  [ ] Security checklist 100% complete
  [ ] Penetration test scheduled (annual)
  [ ] Security documentation complete

═══════════════════════════════════════════════════════════════
PHASE 9: DOCUMENTATION                         ⏱️ 12-16 hours
═══════════════════════════════════════════════════════════════

[Same as SaaS but with additional infrastructure docs]
  [ ] Architecture diagram (cloud + application)
  [ ] Network diagram (VPC, subnets, security groups)
  [ ] Data flow diagram
  [ ] Infrastructure as code repository
  [ ] Runbooks (deployment, backup, restore, scaling)
  [ ] Incident response plan
  [ ] Disaster recovery plan
  [ ] Security policies
  [ ] Acceptable use policy
  [ ] Admin procedures
  [ ] User training materials
  [ ] Compliance documentation
  [ ] Vendor contacts (cloud provider support)
  [ ] Credential inventory
  [ ] Cost optimization guide

═══════════════════════════════════════════════════════════════
TOTAL CLOUD SELF-HOSTED CHECKLIST: ~180 items
ESTIMATED EFFORT: 160-230 hours before production
═══════════════════════════════════════════════════════════════
```
