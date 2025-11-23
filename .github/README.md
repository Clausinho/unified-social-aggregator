# GitHub Configuration

This directory contains GitHub-specific configuration files for CI/CD, security scanning, and automation.

## Contents

### Workflows (`.github/workflows/`)

1. **`codeql-analysis.yml`** - CodeQL security scanning for both frontend and backend
2. **`ci.yml`** - Continuous Integration pipeline with linting, building, and testing
3. **`dependency-review.yml`** - Automated dependency vulnerability review on PRs
4. **`security-scan.yml`** - Comprehensive security scanning (npm audit, Trivy, Gitleaks)
5. **`semgrep.yml`** - Static Application Security Testing (SAST) with Semgrep

### Configuration Files

- **`dependabot.yml`** - Automated dependency updates for packages and GitHub Actions
- **`PULL_REQUEST_TEMPLATE.md`** - Template for pull requests with security checklist
- **`SECURITY_SCANNING.md`** - Comprehensive guide to security scanning tools and processes

## Workflow Triggers

### On Push (main, develop)
- CodeQL Analysis
- CI (lint, build, test)
- Security Scanning
- Semgrep

### On Pull Request (main, develop)
- All push workflows
- Dependency Review

### Scheduled
- **Weekly (Mondays)**: CodeQL Analysis
- **Daily**: Security Scanning, Semgrep
- **Weekly (Mondays)**: Dependabot checks

## Security Scanning Layers

1. **Code Analysis**
   - CodeQL for vulnerability detection
   - Semgrep for security patterns
   - ESLint with security rules

2. **Dependency Management**
   - npm/pnpm audit for known vulnerabilities
   - Dependabot for automated updates
   - Dependency Review for PR validation

3. **Secret Detection**
   - Gitleaks for hardcoded secrets
   - GitHub Secret Scanning (if enabled)

4. **Vulnerability Scanning**
   - Trivy for comprehensive filesystem scanning
   - SARIF output for GitHub Security integration

## Badge Setup

Add these badges to your main README.md:

```markdown
[![CodeQL](https://github.com/Clausinho/unified-social-aggregator/workflows/CodeQL%20Security%20Scanning/badge.svg)](https://github.com/Clausinho/unified-social-aggregator/actions/workflows/codeql-analysis.yml)
[![CI](https://github.com/Clausinho/unified-social-aggregator/workflows/Continuous%20Integration/badge.svg)](https://github.com/Clausinho/unified-social-aggregator/actions/workflows/ci.yml)
[![Security Scan](https://github.com/Clausinho/unified-social-aggregator/workflows/Security%20Scanning/badge.svg)](https://github.com/Clausinho/unified-social-aggregator/actions/workflows/security-scan.yml)
```

## Optional Secrets

Some workflows reference optional secrets that can be configured in repository settings:

- `CODECOV_TOKEN` - Optional: For code coverage reporting to Codecov. The workflow will skip coverage upload if not configured.
- `GITLEAKS_LICENSE` - Optional: For Gitleaks Pro features. Gitleaks will work without this secret using the free version.

## Customization

### Adjusting Scan Frequency

Edit the `schedule.cron` values in workflow files:
- Daily: `'0 6 * * *'`
- Weekly: `'0 0 * * 1'`
- Monthly: `'0 0 1 * *'`

### Changing Severity Thresholds

In `dependency-review.yml` and `security-scan.yml`, adjust:
- `fail-on-severity: moderate` (options: low, moderate, high, critical)
- `audit-level: moderate` for npm/pnpm

### Adding Custom Semgrep Rules

Create `.semgrep/` directory with custom rule files, then update `semgrep.yml`:
```yaml
run: semgrep scan --config=auto --config=.semgrep/
```

## Maintenance

- Review Dependabot PRs weekly
- Check Security tab regularly for alerts
- Update workflow actions quarterly
- Review and update security policies annually

## Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [GitHub Security Features](https://docs.github.com/en/code-security)
- [CodeQL Documentation](https://codeql.github.com/docs/)
- [Semgrep Rules](https://semgrep.dev/explore)
