# Encryption at Rest Configuration

## Standards

| Data Type | Algorithm | Key Length | Key Management |
|-----------|-----------|------------|----------------|
| Database | AES-256-GCM | 256-bit | Cloud KMS or HSM |
| File storage | AES-256 | 256-bit | Cloud KMS |
| Backups | AES-256 | 256-bit | Separate key per backup |
| Secrets | AES-256-GCM | 256-bit | HashiCorp Vault or cloud secrets manager |

## Implementation Guidelines

1. **Never** implement custom encryption — use vetted libraries (libsodium, AWS Encryption SDK)
2. **Key rotation:** Rotate encryption keys annually or on suspected compromise
3. **Key access:** Log all key usage; alert on anomalous access patterns
4. **Envelope encryption:** Encrypt data with DEK, protect DEK with KMS master key

## Compliance Checklist

- [ ] All PII/sensitive fields encrypted at rest
- [ ] Database encryption enabled (transparent data encryption)
- [ ] Backup encryption enabled and tested
- [ ] Key management documented and audited
- [ ] Encryption status included in compliance reports

## Verification

```bash
# AWS RDS encryption check
aws rds describe-db-instances --query 'DBInstances[*].[DBInstanceIdentifier,StorageEncrypted]'

# S3 bucket encryption
aws s3api get-bucket-encryption --bucket your-bucket-name
```
