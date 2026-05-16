# Security Policy

## Reporting a Vulnerability

We take the security of the solana.com project seriously. If you discover a security vulnerability, please do not open a public issue. Instead, please report it responsibly by emailing us at [security@dmenterprises.com](mailto:security@dmenterprises.com).

### What to Include in Your Report

When reporting a vulnerability, please provide:

- **Description**: A clear description of the vulnerability
- **Affected Component**: Which part of the codebase is affected
- **Steps to Reproduce**: Detailed steps to reproduce the vulnerability
- **Potential Impact**: Description of the potential impact
- **Suggested Fix** (optional): If you have a suggested fix

### Response Timeline

- We will acknowledge receipt of your security report within **24 hours**
- We aim to provide an initial assessment within **72 hours**
- We will work to develop and test a patch
- We will notify you before publishing any security advisory

## Security Update Policy

### Versioning

We follow [Semantic Versioning](https://semver.org/):
- **MAJOR**: Breaking changes
- **MINOR**: New features (backwards compatible)
- **PATCH**: Bug fixes and security patches

### Supported Versions

| Version | Status | Support Until |
|---------|--------|---------------|
| 1.x | Active | Until next major release |

Security patches will be released for the current and previous minor versions only.

## Security Best Practices

When contributing to this project, please follow these security best practices:

1. **Dependencies**: Keep dependencies up to date. Run `npm audit` or equivalent regularly
2. **Secrets**: Never commit secrets, API keys, or passwords
3. **Code Review**: All code changes must be reviewed before merging
4. **Testing**: Write tests for security-sensitive code
5. **Input Validation**: Always validate and sanitize user input
6. **HTTPS**: Ensure all connections are secure
7. **Logging**: Avoid logging sensitive information

## Disclosure Policy

- We practice responsible disclosure
- We ask that you allow us reasonable time to patch vulnerabilities before public disclosure
- Once a patch is available, we will publish a security advisory with details of the fix
- We will credit the reporter (unless they prefer to remain anonymous)

## Security Headers

This project implements standard security headers to protect against common web vulnerabilities:

- Content Security Policy (CSP)
- X-Frame-Options
- X-Content-Type-Options
- Strict-Transport-Security (HSTS)

## Third-Party Security Tools

We use the following tools to maintain security:

- GitHub's Dependabot for dependency scanning
- GitHub's Secret scanning
- SAST tools for code analysis

## Contact

For questions about this security policy, please contact:
- **Email**: [security@dmenterprises.com](mailto:security@dmenterprises.com)
- **Repository**: [DMENTERPRISES/solana.com](https://github.com/DMENTERPRISES/solana.com)

---

**Last Updated**: 2026-05-16
