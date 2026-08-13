# Cloud Security Policy: template

> **How to use:** replace bracketed items, delete this note, and have the owner named in §1 approve it. A policy nobody approved, or that promises controls that don't exist, fails audit, keep every "must" in this document true. This template is deliberately short: a policy people can read gets followed; a 40-page one gets acknowledged unread.

**Document owner:** [name/role] · **Approved by:** [name, date] · **Review cadence:** annually and after material change · **Next review:** [date]

## 1. Purpose and scope

This policy defines the minimum security requirements for all [Organization] cloud environments (currently AWS account(s) [IDs]). It applies to every person and system with access to those environments.

## 2. Roles

- **Cloud administrator**, operates the environment, implements controls, executes the runbooks referenced here.
- **Control owner ([role])**, accountable that each §4 control exists and operates; approves exceptions.
- **All users**, comply with §3; report suspected incidents immediately to [channel].

## 3. Rules for everyone with access

1. Individual accounts only; no shared credentials. MFA is mandatory for all console access.
2. The root account is not used for daily work; it has hardware MFA and no access keys.
3. Access keys are never placed in code, tickets, chat, or screenshots. Suspected exposure is reported immediately, reporting is never penalized; concealment is.
4. Access follows least privilege via group/role membership; direct policy attachments to users are prohibited.
5. Production data is not copied to personal devices or unauthorized services.

## 4. Required technical controls

| Control | Standard | Implementation |
|---------|----------|----------------|
| Audit logging | All API activity logged, multi-region, tamper-evident, retained ≥ [365] days | CloudTrail + validation (aws-security-baseline) |
| Threat detection | Continuous, with high-severity findings alerting a human | GuardDuty → SNS |
| Configuration compliance | Continuous evaluation against defined rules; deviations ticketed | AWS Config rules; Security Hub CIS benchmark |
| Identity hygiene | Password policy (≥14 chars, complexity, reuse 24, max age 90); keys rotated ≤ 90 days; quarterly access review | IAM policy + monthly credential-report review |
| Public exposure | No publicly accessible storage without a documented exception | Account-level S3 public access block |
| Encryption | At rest by default (EBS, RDS, S3); TLS in transit | Provider-native encryption |
| Patching | Monthly cycle; critical CVEs out-of-band ≤ [7] days | SSM Patch Manager (Runbook 02) |
| Backup & recovery | Daily backups, retention [35] days; restore tested quarterly with recorded RTO | AWS Backup + drills (Runbook 03) |
| Cost guardrails | Budget alarms; monthly spend review | AWS Budgets (supports availability of funds ≠ security, retained for governance) |

## 5. Change and incident handling

- Production changes go through [ticketing/change process]; emergency changes are documented retroactively within 1 working day.
- Suspected incidents: report to [channel] immediately. Stabilize using the alarm-response runbook; do not destroy evidence (no terminating compromised instances before snapshot/isolation). Post-incident notes are mandatory.

## 6. Exceptions

Any deviation from §3–§5 requires a written exception in the risk register with: rationale, compensating measures, approver, and an **expiry date** (max 12 months). Expired exceptions are violations.

## 7. Enforcement and review

Compliance is verified via the monthly maintenance review and the continuous controls in §4. Willful violations follow [HR/disciplinary process]. This policy is reviewed annually by the document owner; the revision history lives in version control.

---
*Revision history: v0.1 [date], initial draft.*
