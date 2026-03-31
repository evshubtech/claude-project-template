---
name: deploy-checklist
description: Pre-deployment verification checklist to reduce risk of incidents. Invoke with /deploy-checklist before any deployment to staging or production.
---

# /deploy-checklist

## Pre-Deploy

### Code & Tests
- [ ] All tests passing in CI
- [ ] No known critical bugs in the release
- [ ] Feature flags configured for large changes

### Infrastructure (read: cloud + devops skills)
- [ ] `terraform plan` reviewed — no unexpected changes
- [ ] IAM permissions validated
- [ ] Secrets updated in secret manager if needed
- [ ] Budget alerts still configured (read: finops skill)

### Database (read: data skill)
- [ ] Migrations tested in staging
- [ ] Rollback script prepared
- [ ] Migration is backward-compatible during rollout window

### Observability
- [ ] Health check endpoint working
- [ ] Alerts configured for new services/endpoints
- [ ] Runbook updated if operational behavior changed

## Deploy
```bash
[deploy to staging]
[run smoke tests]
# After approval:
[deploy to prod]
```

## Post-Deploy (first 15 minutes)
- [ ] Health check green
- [ ] Error rate normal
- [ ] Key flows tested manually
- [ ] No unexpected cost spikes

## Rollback
```bash
[rollback command]
```
