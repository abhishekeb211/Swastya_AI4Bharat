# Contributing to Swasthya

Thank you for your interest in contributing to Swasthya! This document provides guidelines for contributing to the project.

---

## Code of Conduct

We are committed to providing a welcoming and inclusive environment. Please be respectful and professional in all interactions.

---

## How to Contribute

### Reporting Bugs
1. Check if the bug has already been reported in Issues
2. Create a new issue with detailed description
3. Include steps to reproduce, expected vs actual behavior
4. Add relevant labels (bug, priority, etc.)

### Suggesting Features
1. Check if the feature has been requested
2. Create a new issue with [Feature Request] prefix
3. Describe the feature and its benefits
4. Discuss with maintainers before implementation

### Code Contributions

#### 1. Fork the Repository
```bash
git clone https://github.com/your-username/swasthya
cd swasthya
```

#### 2. Create a Branch
```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/bug-description
```

#### 3. Make Changes
- Follow coding standards (see below)
- Write tests for new features
- Update documentation
- Ensure all tests pass

#### 4. Commit Changes
```bash
git add .
git commit -m "feat: add new feature"
# Follow conventional commits format
```

#### 5. Push and Create PR
```bash
git push origin feature/your-feature-name
```
Then create a Pull Request on GitHub

---

## Coding Standards

### JavaScript/TypeScript
- Use ESLint and Prettier
- Follow Airbnb style guide
- Use TypeScript for type safety
- Write JSDoc comments

### Python
- Follow PEP 8
- Use Black for formatting
- Use type hints
- Write docstrings

### Git Commits
Follow Conventional Commits:
- `feat:` new feature
- `fix:` bug fix
- `docs:` documentation
- `test:` tests
- `refactor:` code refactoring
- `chore:` maintenance

---

## Testing

### Unit Tests
```bash
npm test
# or
pytest
```

### Integration Tests
```bash
npm run test:integration
```

### E2E Tests
```bash
npm run test:e2e
```

---

## Documentation

- Update README.md if needed
- Add API documentation for new endpoints
- Update architecture diagrams if applicable
- Write user guides for new features

---

## Review Process

1. Automated checks must pass (CI/CD)
2. Code review by at least one maintainer
3. All comments addressed
4. Documentation updated
5. Tests passing

---

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

## Questions?

Contact us at: contribute@swasthya.health
