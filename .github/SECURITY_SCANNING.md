# Security Scanning Guide

This document describes the security scanning tools and processes implemented in this project.

## Overview

This project uses multiple layers of security scanning to identify and prevent vulnerabilities:

1. **Static Application Security Testing (SAST)**
2. **Dependency Scanning**
3. **Secret Scanning**
4. **Container/Filesystem Scanning**
5. **Code Quality Analysis**

## Automated Scans

### CodeQL Analysis
- **Frequency**: On every push to main/develop, pull requests, and weekly
- **Purpose**: Identifies security vulnerabilities in source code
- **Languages**: TypeScript/JavaScript
- **Scope**: Both frontend and backend packages
- **File**: `.github/workflows/codeql-analysis.yml`

### Dependency Review
- **Frequency**: On every pull request
- **Purpose**: Identifies vulnerable dependencies before merging
- **Fail Level**: Moderate severity and above
- **File**: `.github/workflows/dependency-review.yml`

### Security Scanning (npm audit, Trivy, Gitleaks)
- **Frequency**: On push, pull requests, and daily
- **Components**:
  - **npm/pnpm audit**: Scans npm dependencies for known vulnerabilities
  - **Trivy**: Comprehensive vulnerability scanner for containers and filesystems
  - **Gitleaks**: Detects hardcoded secrets and credentials
- **File**: `.github/workflows/security-scan.yml`

### Semgrep SAST
- **Frequency**: On push, pull requests, and daily
- **Purpose**: Static analysis for security patterns and anti-patterns
- **Configuration**: Auto-config with community rules
- **File**: `.github/workflows/semgrep.yml`

### Dependabot
- **Frequency**: Weekly (Mondays)
- **Purpose**: Automated dependency updates with security patches
- **Scope**: Backend, Frontend, and GitHub Actions
- **File**: `.github/dependabot.yml`

## Continuous Integration

The CI pipeline (`.github/workflows/ci.yml`) runs on every push and pull request:

### Backend Checks
- ESLint linting
- Prettier formatting
- Build verification
- Unit tests with coverage

### Frontend Checks
- ESLint linting
- Prettier formatting
- TypeScript type checking
- Build verification

## Security Results

Security scan results are available in multiple locations:

1. **GitHub Security Tab**: View CodeQL alerts, Dependabot alerts, and secret scanning alerts
2. **Pull Request Checks**: See scan results directly on PRs
3. **Workflow Artifacts**: Download detailed reports from workflow runs

## Responding to Security Alerts

### High/Critical Severity
1. Investigate immediately
2. Assess impact on the application
3. Apply patches or workarounds
4. Deploy fixes as soon as possible
5. Document the resolution

### Medium Severity
1. Review within 7 days
2. Plan remediation
3. Include in next release cycle

### Low Severity
1. Review within 30 days
2. Address during regular maintenance

## Local Security Scanning

Developers can run security scans locally:

### Backend
```bash
cd packages/backend

# Dependency audit
yarn audit

# Lint
yarn lint

# Tests
yarn test
```

### Frontend
```bash
cd packages/frontend

# Dependency audit
pnpm audit

# Lint
pnpm lint

# Type check
pnpm typecheck

# Tests (when available)
pnpm test
```

### Manual Trivy Scan
```bash
# Install Trivy
# https://aquasecurity.github.io/trivy/latest/getting-started/installation/

# Scan the repository
trivy fs .
```

### Manual Semgrep Scan
```bash
# Install Semgrep
# pip install semgrep

# Run scan
semgrep scan --config=auto
```

## Best Practices

1. **Never commit secrets**: Use environment variables
2. **Keep dependencies updated**: Review Dependabot PRs regularly
3. **Address high-severity alerts promptly**: Don't let them accumulate
4. **Review security tab weekly**: Stay aware of your security posture
5. **Use secure coding practices**: Follow OWASP guidelines
6. **Test security fixes**: Ensure fixes don't break functionality

## Additional Resources

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [GitHub Security Features](https://docs.github.com/en/code-security)
- [NestJS Security](https://docs.nestjs.com/security/authentication)
- [Next.js Security Headers](https://nextjs.org/docs/app/building-your-application/configuring/content-security-policy)

## Contact

For security concerns, see [SECURITY.md](../SECURITY.md)
