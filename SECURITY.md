# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability, please report it privately:

1. **Do not** open a public issue
2. Email the maintainers or use GitHub's private vulnerability reporting
3. Include details about the vulnerability and steps to reproduce

We'll respond within 48 hours and work with you to understand and address the issue.

## Supported Versions

We provide security updates for the latest major version of each package.

| Package | Supported |
|---------|-----------|
| Deepstaging | Latest major |
| Deepstaging.Roslyn | Latest major |
| Deepstaging.Roslyn.* | Latest major |

## Security Considerations

### Source Generators

Source generators run at compile time with the same permissions as the build process. When using Deepstaging packages:

- Only use packages from trusted sources (NuGet.org)
- Review generated code if you have security concerns
- Keep packages updated

### Dependencies

We monitor dependencies for known vulnerabilities and update promptly when issues are discovered.
