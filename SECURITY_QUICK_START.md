# Security Quick Start Guide

A quick reference for developers working with this project's security infrastructure.

## Daily Development

### Before Committing Code

```bash
# Backend
cd packages/backend
yarn lint          # Check for linting errors
yarn build         # Verify build works
yarn test          # Run tests

# Frontend
cd packages/frontend
pnpm lint          # Check for linting errors
pnpm typecheck     # TypeScript validation
pnpm build         # Verify build works
```

### Environment Setup

1. **Never commit `.env` files**
2. **Always use `.env.example` as template**
3. **Never hardcode secrets in code**

```bash
# Backend
cp packages/backend/.env.example packages/backend/.env
# Edit .env with your local credentials

# Frontend
cp packages/frontend/.env.example packages/frontend/.env
# Edit .env with your local credentials
```

## Automated Scans

### What Gets Scanned Automatically

✅ Every Push/PR:
- Code quality (ESLint)
- Build verification
- CodeQL security analysis
- Dependency vulnerabilities
- Secret detection
- SAST with Semgrep

✅ Daily:
- Full security scan
- Dependency audits
- Container/filesystem scan

✅ Weekly:
- Dependency updates (Dependabot)

### Viewing Scan Results

1. **Security Tab**: Check https://github.com/Clausinho/unified-social-aggregator/security
2. **Actions Tab**: View workflow runs and details
3. **PR Checks**: See results directly on pull requests

## Common Security Tasks

### Check for Vulnerabilities

```bash
# Quick local check - Backend
cd packages/backend
npm audit --audit-level=moderate

# Quick local check - Frontend  
cd packages/frontend
pnpm audit --audit-level=moderate
```

### Update Dependencies

```bash
# Backend
cd packages/backend
yarn upgrade-interactive --latest

# Frontend
cd packages/frontend
pnpm update --interactive --latest
```

### Fix Linting Issues

```bash
# Backend
cd packages/backend
yarn lint --fix

# Frontend
cd packages/frontend
pnpm lint --fix
```

## Responding to Alerts

### High/Critical Severity
1. **Investigate immediately**
2. **Review the advisory**
3. **Test the fix locally**
4. **Update dependency or apply patch**
5. **Verify no breaking changes**
6. **Commit and push**

### Medium/Low Severity
1. **Create an issue**
2. **Schedule for next sprint**
3. **Bundle with other updates**

## Security Checklist for PRs

- [ ] No secrets or API keys in code
- [ ] All new dependencies audited
- [ ] Tests pass locally
- [ ] Linting passes
- [ ] Build succeeds
- [ ] No new security warnings
- [ ] Environment variables documented
- [ ] Security implications considered

## Quick Commands Reference

### Backend (NestJS)
```bash
# Development
yarn start:dev

# Build
yarn build

# Test
yarn test
yarn test:cov  # with coverage

# Lint
yarn lint
yarn format
```

### Frontend (Next.js)
```bash
# Development
pnpm dev

# Build
pnpm build

# Lint
pnpm lint
pnpm format:check

# Type check
pnpm typecheck
```

## Help & Resources

- **Security Policy**: [SECURITY.md](SECURITY.md)
- **Detailed Scanning Guide**: [SECURITY_SCANNING.md](.github/SECURITY_SCANNING.md)
- **Scan Results**: [SCANNING_RESULTS.md](SCANNING_RESULTS.md)
- **GitHub Actions**: [GitHub README](.github/README.md)

## Emergency Contacts

For security emergencies:
1. Do NOT create a public issue
2. Follow process in [SECURITY.md](SECURITY.md)
3. Contact maintainers privately

## Best Practices

### DO ✅
- Use environment variables for secrets
- Keep dependencies updated
- Review Dependabot PRs promptly
- Run linters before committing
- Write tests for new features
- Document security considerations

### DON'T ❌
- Commit secrets or API keys
- Ignore security warnings
- Disable security checks
- Skip linting/testing
- Use untrusted dependencies
- Hardcode credentials

## Workflow Status

Check current status of all workflows:
https://github.com/Clausinho/unified-social-aggregator/actions

Badge should be green: [![CI](https://github.com/Clausinho/unified-social-aggregator/workflows/Continuous%20Integration/badge.svg)](https://github.com/Clausinho/unified-social-aggregator/actions/workflows/ci.yml)
