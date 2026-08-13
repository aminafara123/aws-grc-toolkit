# Cloud risk register: template and worked examples

**Scales:** Likelihood and Impact each 1–5 (1 = rare/negligible, 5 = almost certain/severe). Rating = L × I: 1–6 Low, 8–12 Medium, 15–25 High. **Treatments:** Mitigate, Accept, Transfer, Avoid. Every non-accepted risk names its mitigating control; every accepted risk gets an expiry date for re-review, accepted risks must never be immortal.

| ID | Risk | L | I | Rating | Treatment | Mitigating control / rationale | Owner | Review |
|----|------|---|---|--------|-----------|-------------------------------|-------|--------|
| R-01 | Leaked IAM access key (committed to a repo, pasted in chat) grants an outsider API access | 3 | 5 | **15 High** | Mitigate | Minimize long-lived keys (OIDC for CI, see aws-devsecops-pipeline); 90-day age + unused-credential checks; gitleaks secret scanning; documented revocation step in offboarding runbook | Cloud admin | Quarterly |
| R-02 | S3 bucket unintentionally made public exposes data | 2 | 5 | **10 Medium** | Mitigate | Account-level public access block; Config rules `s3-bucket-public-read/write-prohibited`; auto-remediation re-applies the block on change events | Cloud admin | Quarterly |
| R-03 | Privileged console user without MFA is phished | 3 | 5 | **15 High** | Mitigate | MFA required by policy; `iam-user-mfa-enabled` Config rule; monthly credential-report review; root has hardware-token MFA and no keys | Cloud admin | Quarterly |
| R-04 | Backups exist but restores fail when needed (silent backup rot) | 2 | 5 | **10 Medium** | Mitigate | Quarterly restore drill with measured RTO (Runbook 03); failed drill = high-priority ticket by definition | Cloud admin | Quarterly (drill) |
| R-05 | Ex-employee retains access after departure | 2 | 4 | **8 Medium** | Mitigate | Documented offboarding runbook with same-day console disable, key deactivation soak, and verification step; monthly review reconciles leavers vs. IAM | Cloud admin + HR | Monthly |
| R-06 | Unpatched instance exploited via known CVE | 3 | 4 | **12 Medium** | Mitigate | Monthly SSM patch cycle with compliance reporting; out-of-band patching on critical CVEs; unmanaged-node check in monthly maintenance | Cloud admin | Monthly |
| R-07 | Runaway cloud spend (crypto-mining compromise, forgotten resources, misconfigured scaling) | 3 | 3 | **9 Medium** | Mitigate | Budget alarms at 50/80/100%; GuardDuty (mining detection); monthly cost review with 20%-variance rule; teardown discipline for lab resources | FinOps/admin | Monthly |
| R-08 | CloudTrail disabled or log bucket tampered with, blinding detection and audit | 1 | 5 | **5 Low** | Mitigate | Log-file validation on; bucket policy restricts writes to the CloudTrail service principal; `cloud-trail-enabled` Config rule; threat hunt exists for trail-stop events | Cloud admin | Quarterly |
| R-09 | Single-region dependency: regional AWS outage takes the workload down | 2 | 4 | **8 Medium** | **Accept** (expiry: 2027-02) | Multi-region architecture doubles cost and complexity for a workload whose users tolerate hours of downtime; documented restore-from-backup path is the fallback. Re-evaluate if availability commitments change | Owner/mgmt | At expiry |
| R-10 | Third-party SaaS holding our data suffers a breach (vendor risk) | 2 | 4 | **8 Medium** | Transfer + Mitigate | Contract/DPA terms (transfer); vendor list with data-classification per vendor; least data shared; review vendor SOC 2 reports annually | GRC | Annually |

## Notes on method (what an interviewer will ask)

- **Why is R-01 likelihood 3, not 2?** Because key leakage is the single most common real-world AWS compromise vector, and the account has human users. Likelihood scores should reflect observed reality, not optimism.
- **Why accept R-09 instead of mitigating?** Treatment must be proportionate to the business's actual availability needs. Writing down the acceptance, with an expiry, is the control.
- **What makes this register alive rather than shelf-ware?** The Review column, the expiry on acceptances, and the fact that every mitigation names a control that demonstrably exists (see [control-mapping.md](control-mapping.md)). A register that references controls nobody deployed is fiction with a table border.
