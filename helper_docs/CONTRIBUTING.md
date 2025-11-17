# Contributing to FlowHub

Thank you for your interest in contributing to FlowHub! This document outlines how you can help.

## Current Status

**We're actively developing v1 and v2.** We welcome:
- ✅ **Issues**: Bug reports, feature requests, questions, documentation improvements
- ⏳ **Pull Requests**: Will be accepted after v1 stabilizes (coming soon)

## How to Contribute

### Reporting Issues

Found a bug? Have a feature request? Questions about the project?

1. **Check existing issues** to see if it's already reported
2. **Open a new issue** with:
   - Clear title and description
   - Steps to reproduce (for bugs)
   - Expected vs actual behavior
   - Environment details (OS, Docker version, etc.)

### Code Contributions

**Note**: PRs will be accepted after v1 stabilizes. For now, feel free to:
- Fork the repository
- Make your changes
- Open an issue describing what you'd like to contribute
- We'll review and provide feedback

### Documentation

Documentation improvements are always welcome:
- Fix typos
- Clarify unclear sections
- Add examples
- Improve structure

Open an issue or PR (when PRs are enabled) with your changes.

## Development Setup

See [`QUICKSTART.md`](QUICKSTART.md) for local setup instructions.

## Code Standards

### Selector Policy

**All code must follow the selector policy**: [`SELECTOR_POLICY.md`](SELECTOR_POLICY.md)

- Prefer semantic selectors (aria-labelledby, aria-label, sr-only)
- Use data-testid as fallback
- CSS/XPath only as last resort (with tickets)

### Testing

All code changes should include tests:
- Unit tests for new functions/utilities
- Integration tests for new features
- API tests for new endpoints
- E2E tests for new user flows

Run tests before submitting:
```bash
pytest
```

### Code Style

- Follow existing code patterns
- Use meaningful variable names
- Add comments for complex logic
- Keep functions focused and small

## Pull Request Process

**Note**: PRs will be enabled after v1 stabilizes. When enabled:

1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b feature/your-feature-name`
3. **Make your changes**
4. **Add tests** for new functionality
5. **Update documentation** if needed
6. **Run tests**: Ensure all tests pass
7. **Commit**: Use clear, descriptive commit messages
8. **Push**: Push to your fork
9. **Open PR**: Provide clear description of changes

### PR Checklist

- [ ] Code follows selector policy
- [ ] Tests added/updated and passing
- [ ] Documentation updated
- [ ] No breaking changes (or clearly documented)
- [ ] Follows existing code patterns

## Project Structure

Understanding the project structure helps with contributions:

```
LinkedIn/
├── purpose/              # Project purpose and story
├── infrastructure/      # Infrastructure documentation
├── complete-flows/      # 18 user flows overview
├── software-development-life-cycle/  # BRD, PRD, FRD, System Design
└── docs/                # Additional documentation
```

## Questions?

- **Open an issue** with the `question` label
- **Check the FAQ**: [`FAQ.md`](FAQ.md)
- **Read the docs**: Start with [`README.md`](../README.md)

## Code of Conduct

**Coming soon**: We're working on a code of conduct. For now, be respectful, inclusive, and constructive.

## Recognition

Contributors will be recognized in:
- README.md (contributors section)
- Release notes
- Project documentation

---

**Thank you for contributing to FlowHub! Your help makes this project better for the entire QA community.**

