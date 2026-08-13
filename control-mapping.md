# Control mapping: AWS technical controls to ISO/IEC 27001:2022 & SOC 2

Every control in the first column is deployed in my own AWS account (via aws-security-baseline) or operated as a documented procedure (aws-admin-runbooks). Mappings are **indicative working mappings** for internal GRC use, formal engagements map against the official framework texts.

| # | AWS control (implemented) | CIS AWS area | ISO 27001:2022 Annex A (indicative) | SOC 2 TSC (indicative) | Evidence to collect |
|---|---------------------------|--------------|--------------------------------------|------------------------|---------------------|
| 1 | Multi-region CloudTrail with log file validation, hardened S3 bucket | Logging | A.8.15 Logging | CC7.2 | Trail configuration screenshot; a validated digest file; bucket policy showing TLS-only + service-scoped writes |
| 2 | GuardDuty enabled, high-severity findings alerting to a human via SNS | Monitoring | A.8.16 Monitoring activities; A.5.7 Threat intelligence | CC7.2, CC7.3 | Detector status; a sample alert email; evidence findings are reviewed (ticket refs) |
| 3 | AWS Config recorder + 8 managed compliance rules | Monitoring / configuration | A.8.9 Configuration management | CC7.1 | Recorder status; rule list with compliance state; a remediated-noncompliance ticket |
| 4 | Security Hub with CIS AWS Foundations Benchmark standard | Governance | A.5.36 Compliance with policies, rules and standards | CC4.1 | CIS score report, month-over-month trend |
| 5 | IAM account password policy (14+ chars, complexity, reuse 24, max age 90) | IAM | A.5.17 Authentication information | CC6.1 | `aws iam get-account-password-policy` output |
| 6 | MFA enforced for console users (Config rule + monthly credential-report review) | IAM | A.8.5 Secure authentication | CC6.1 | Credential report excerpt; the monthly review note (Runbook 05) |
| 7 | Access key hygiene: 90-day age flag, unused-credential flag, no root keys | IAM | A.5.16 Identity management; A.5.18 Access rights | CC6.2, CC6.3 | Credential report; auditor tool output (aws-security-auditor IAM checks) |
| 8 | Documented joiner/leaver process with 5-day key-deactivation soak | IAM | A.5.18 Access rights | CC6.2 | Completed on/offboarding tickets vs. runbook steps (Runbook 01) |
| 9 | Account-level S3 public access block | Storage | A.8.12 Data leakage prevention | CC6.7 | `get-public-access-block` output; Config rule state |
| 10 | Encryption at rest: EBS-by-default, RDS storage encryption, S3 SSE | Storage | A.8.24 Use of cryptography | CC6.1; Confidentiality C1.1 | Region EBS-default-encryption setting; per-resource encryption attributes |
| 11 | Network segmentation: tiered VPC, chained security groups, no world-open admin ports | Networking | A.8.20 Networks security; A.8.22 Segregation of networks | CC6.6 | Security-group rule export; VPC diagram (aws-secure-web-infra) |
| 12 | Monthly patching via SSM Patch Manager with compliance reporting | Vulnerability mgmt | A.8.8 Management of technical vulnerabilities | CC7.1 | Patch compliance report before/after; maintenance ticket (Runbook 02) |
| 13 | Automated backups + quarterly restore drills with measured RTO | Resilience | A.8.13 Information backup | Availability A1.2 | Backup job history; drill record with restore times (Runbook 03) |
| 14 | Budget alarms + monthly cost review | Operational governance | (Supports A.8.6 Capacity management, cost is not an ISO security control) | (Supports availability commitments) | Budget config; monthly review note |

## How to read row 14 (and why it's honest)

Not everything an operator does maps to a security framework, and forcing a mapping is how control matrices lose credibility. The budget alarm row is kept, clearly labeled, because a GRC analyst must know *when to say "this is out of scope."*

## Gaps this mapping exposes (self-assessment)

An auditor working from this matrix would next ask about: formal risk assessment cadence (→ [risk-register.md](risk-register.md) is the artifact, but it needs a review schedule), security awareness training records (A.6.3), supplier management (A.5.19–5.23), and incident management procedure beyond alarm response (A.5.24–5.28). Naming your own gaps before the auditor does is the job.
