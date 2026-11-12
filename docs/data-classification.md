# Data Classification Policy

## Classification Levels

| Level | Label | Description | Examples |
|-------|-------|-------------|---------|
| 1 | Public | Non-sensitive, intentionally public | Marketing content, public docs |
| 2 | Internal | For employees only | Internal wikis, non-sensitive reports |
| 3 | Confidential | Sensitive business data | Customer data, contracts |
| 4 | Restricted | Highly sensitive, regulated | PII, credentials, health data, financials |

## Handling Requirements

| Level | Encryption at Rest | Encryption in Transit | Access Control | Audit Logging |
|-------|-------------------|----------------------|----------------|---------------|
| Public | Optional | Recommended | None | Not required |
| Internal | Recommended | Required | Role-based | Recommended |
| Confidential | Required | Required | Role-based, MFA | Required |
| Restricted | Required (AES-256) | Required (TLS 1.2+) | Need-to-know, MFA | Required |

## Developer Guidelines

1. Identify data classification before designing new features
2. Apply minimum-necessary access principle
3. Never log Restricted data in plaintext
4. Use secrets manager for credentials (never hardcode)
5. Sanitize PII from error messages and stack traces
