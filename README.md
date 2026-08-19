# aws-grc-toolkit

A practical cloud GRC starter kit: technical AWS controls mapped to ISO/IEC 27001:2022 and SOC 2, a cloud risk register with worked examples, an audit evidence checklist, and a usable cloud security policy template.

## Why I built this

GRC work lives in the gap between "the auditor asks in framework language" and "the engineer answers in AWS console screenshots." After six years running the systems that auditors audit, patching them, backing them up, controlling who could log in, I built the translation layer I kept wishing existed: which AWS control satisfies which framework clause, what evidence proves it, and how the risks behind them should be written down.

## What this demonstrates

- Framework literacy: ISO/IEC 27001:2022 Annex A and SOC 2 Trust Services Criteria, mapped to real, deployed AWS controls (not abstract checklists)
- Risk thinking: a register with honest likelihood/impact reasoning and treatments that reference actual technical controls
- Audit fluency: knowing what evidence an auditor accepts and exactly where it lives in AWS
- The operator's advantage in GRC: every control in the mapping is one I have personally deployed (via my [aws-security-baseline](https://github.com/aminafara123/aws-security-baseline) project) or operated (via my [aws-admin-runbooks](https://github.com/aminafara123/aws-admin-runbooks))

## Contents

| Document | What it is |
|----------|-----------|
| [control-mapping.md](control-mapping.md) | 14 technical AWS controls → CIS area → ISO 27001:2022 Annex A → SOC 2 TSC → evidence to collect |
| [risk-register.md](risk-register.md) | Register template + 10 worked cloud-risk entries with treatments |
| [evidence-checklist.md](evidence-checklist.md) | What an auditor asks for, domain by domain, and where to get it in AWS |
| [cloud-security-policy.md](cloud-security-policy.md) | A concise, enforceable cloud security policy template |

## Honest scope note

The framework mappings are **indicative working mappings**, the kind a GRC analyst drafts for internal use, formal certification work is done against the official framework texts with a qualified auditor. That caveat is itself part of the craft: knowing the difference between a working mapping and an authoritative one.

## What I'd add next

- NIST CSF 2.0 as a third mapping column
- A vendor/third-party risk assessment template
- A statement-of-applicability (SoA) worked example
- Exception register with expiry-based re-review (accepted risks should never be immortal)

## About me

I'm Al Amin Bashir Afara, I spent seven years in IT (systems administration, infrastructure, and support) before completing a BSc in Computer Science, and I'm now focused on cloud operations, GRC, and FinOps on AWS. I'm open to remote roles in those fields.

GitHub: https://github.com/aminafara123 · LinkedIn: https://www.linkedin.com/in/aminafara · Email: aminafara123@gmail.com

## License

MIT, see [LICENSE](LICENSE).
