# Audit evidence checklist: what the auditor asks, where it lives in AWS

Organized by audit domain. "Ask" is what an ISO 27001 / SOC 2 auditor typically requests; "Where" is the exact AWS location or command. Rule of thumb: **system-generated beats screenshot, screenshot beats assertion, assertion alone is not evidence.**

## Access control

| Ask | Where |
|-----|-------|
| Full user list with last-activity | IAM credential report (IAM → Credential report, or `aws iam generate-credential-report` / `get-credential-report`) |
| Evidence MFA is enforced | Credential report `mfa_active` column; Config rule `iam-user-mfa-enabled` compliance state |
| Password policy | `aws iam get-account-password-policy` |
| Privileged-access list and justification | IAM groups with admin policies + membership; direct-attachment check (auditor tool IAM-005) |
| Joiner/leaver samples | On/offboarding tickets cross-referenced against runbook steps and CloudTrail `CreateUser`/`DeleteUser` events |
| Access reviews happen | Monthly maintenance notes (Runbook 05) showing credential-report review with dated findings |

## Logging & monitoring

| Ask | Where |
|-----|-------|
| Audit logging enabled, tamper-evident | CloudTrail trail config (multi-region, validation on); a digest file; log-bucket policy |
| Log retention meets policy | S3 lifecycle rule on the log bucket (365 days in my baseline) |
| Security monitoring is active and reviewed | GuardDuty detector status; sample alert; tickets referencing findings |
| Alerting reaches humans | SNS topic subscription confirmation; a received alert email |

## Configuration & vulnerability management

| Ask | Where |
|-----|-------|
| Configuration baseline monitored | AWS Config recorder status + rule compliance dashboard |
| Benchmark posture | Security Hub CIS score, exported month over month |
| Patch process and its results | Patch Manager compliance report; monthly patching tickets (Runbook 02); baseline definition |
| Vulnerability scanning cadence | (Gap in my lab: no Inspector, name it before the auditor does, with the plan) |

## Data protection

| Ask | Where |
|-----|-------|
| Encryption at rest | EBS default-encryption setting (`aws ec2 get-ebs-encryption-by-default`); RDS `StorageEncrypted`; S3 bucket encryption config |
| Public exposure prevention | Account public access block (`aws s3control get-public-access-block`); Config rule states |
| Encryption in transit | TLS-only bucket policies; ALB listener config (ACM cert in production) |

## Resilience

| Ask | Where |
|-----|-------|
| Backups configured and succeeding | AWS Backup job history; RDS `LatestRestorableTime` |
| Restores actually tested | Quarterly drill records with measured restore times (Runbook 03), auditors specifically ask for *tested*, and most shops fail here |

## Governance

| Ask | Where |
|-----|-------|
| A security policy exists and is current | [cloud-security-policy.md](cloud-security-policy.md) with owner + review date filled in |
| Risks are assessed and treated | [risk-register.md](risk-register.md) with review dates and expiring acceptances |
| Exceptions are managed | Exception entries in the register (e.g. R-09) with expiry, not verbal "we know about that" |

## Evidence hygiene rules

1. Date everything; an undated screenshot is weak evidence.
2. Prefer exports/CLI output with the account ID visible (redact appropriately for external sharing).
3. Collect evidence *as the process runs* (tickets, monthly notes), reconstructing evidence at audit time is the classic GRC failure mode, and it shows.
