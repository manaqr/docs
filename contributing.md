---
layout: default
title: Contributing
---

# Contributing to manaqr

Thank you for your interest in contributing to manaqr! This guide will help you get started with contributing to the project.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Submitting Changes](#submitting-changes)
- [Style Guidelines](#style-guidelines)
- [Community](#community)

## Code of Conduct

By participating in this project, you agree to abide by our Code of Conduct:

- **Be respectful** - Treat everyone with respect and kindness
- **Be inclusive** - Welcome newcomers and diverse perspectives
- **Be collaborative** - Work together and help each other
- **Be professional** - Keep discussions focused and constructive

We do not tolerate harassment, discrimination, or inappropriate behavior of any kind.

## How Can I Contribute?

There are many ways to contribute to manaqr:

### 🐛 Reporting Bugs

Found a bug? Please report it!

1. Check if the bug has already been reported in [Issues](https://github.com/manaqr/docs/issues)
2. If not, create a new issue with:
   - Clear title and description
   - Steps to reproduce
   - Expected vs actual behavior
   - Screenshots if applicable
   - Environment details (OS, browser, SDK version)

**Bug Report Template**:

```markdown
**Description**
A clear description of the bug.

**Steps to Reproduce**
1. Go to '...'
2. Click on '...'
3. See error

**Expected Behavior**
What you expected to happen.

**Actual Behavior**
What actually happened.

**Environment**
- OS: [e.g., macOS 13.0]
- Browser: [e.g., Chrome 115]
- SDK Version: [e.g., 1.2.3]

**Screenshots**
If applicable, add screenshots.

**Additional Context**
Any other relevant information.
```

### 💡 Suggesting Features

Have an idea for a new feature?

1. Check if it's already been suggested in [Issues](https://github.com/manaqr/docs/issues)
2. If not, create a feature request with:
   - Clear title and description
   - Use case and benefits
   - Proposed implementation (if you have ideas)
   - Examples or mockups

**Feature Request Template**:

```markdown
**Feature Description**
A clear description of the feature.

**Use Case**
Why is this feature needed? What problem does it solve?

**Proposed Solution**
How you envision this feature working.

**Alternatives Considered**
Alternative solutions you've considered.

**Additional Context**
Any other relevant information, mockups, or examples.
```

### 📝 Improving Documentation

Documentation improvements are always welcome!

- Fix typos or clarify instructions
- Add examples or use cases
- Translate documentation
- Create tutorials or guides

Documentation lives in the [docs repository](https://github.com/manaqr/docs).

### 🔧 Contributing Code

Ready to contribute code? Great!

1. Check [open issues](https://github.com/manaqr/docs/issues) for tasks to work on
2. Comment on the issue to let others know you're working on it
3. Fork the repository
4. Create a feature branch
5. Make your changes
6. Submit a pull request

See [Development Setup](#development-setup) below for details.

### 📣 Spreading the Word

Help us grow the community:

- Star the repository on GitHub
- Share manaqr on social media
- Write blog posts or tutorials
- Answer questions in [Discussions](https://github.com/manaqr/docs/discussions)
- Speak about manaqr at meetups or conferences

## Getting Started

### Prerequisites

- Git
- Node.js 16+ (for JavaScript SDK)
- Python 3.8+ (for Python SDK)
- Text editor or IDE

### Finding Issues

Look for issues labeled:
- `good first issue` - Great for newcomers
- `help wanted` - We need your help
- `documentation` - Documentation improvements
- `bug` - Bug fixes
- `enhancement` - New features

## Development Setup

### Forking and Cloning

1. Fork the repository on GitHub
2. Clone your fork:

```bash
git clone https://github.com/YOUR-USERNAME/manaqr.git
cd manaqr
```

3. Add upstream remote:

```bash
git remote add upstream https://github.com/manaqr/manaqr.git
```

### Node.js SDK Setup

```bash
cd sdk/nodejs
npm install
```

### Python SDK Setup

```bash
cd sdk/python
pip install -e ".[dev]"
```

### Running Tests

#### Node.js

```bash
npm test
npm run test:watch  # Watch mode
npm run test:coverage  # With coverage
```

#### Python

```bash
pytest
pytest --watch  # Watch mode
pytest --cov  # With coverage
```

### Running Linters

#### Node.js

```bash
npm run lint
npm run lint:fix  # Auto-fix issues
```

#### Python

```bash
flake8
black .  # Auto-format
pylint manaqr
```

### Building Documentation

```bash
cd docs
bundle install
bundle exec jekyll serve
```

Visit http://localhost:4000 to view the documentation.

## Submitting Changes

### Branching Strategy

Create a feature branch from `main`:

```bash
git checkout -b feature/my-feature
# or
git checkout -b fix/my-bugfix
```

Branch naming conventions:
- `feature/` - New features
- `fix/` - Bug fixes
- `docs/` - Documentation changes
- `refactor/` - Code refactoring
- `test/` - Test improvements

### Making Changes

1. Make your changes in logical commits
2. Write clear commit messages
3. Add tests for new functionality
4. Update documentation if needed
5. Ensure all tests pass
6. Ensure code passes linting

### Commit Messages

Write clear, descriptive commit messages:

```
<type>: <subject>

<body>

<footer>
```

**Types**:
- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation changes
- `style` - Code style changes (formatting)
- `refactor` - Code refactoring
- `test` - Test changes
- `chore` - Build/tooling changes

**Example**:

```
feat: add support for gradient backgrounds

- Add gradientType option (linear, radial)
- Add gradientStartColor and gradientEndColor
- Update documentation with examples
- Add tests for gradient functionality

Closes #123
```

### Pull Request Process

1. **Update your branch** with latest upstream:

```bash
git fetch upstream
git rebase upstream/main
```

2. **Push to your fork**:

```bash
git push origin feature/my-feature
```

3. **Create pull request** on GitHub with:
   - Clear title and description
   - Link to related issue(s)
   - Screenshots/examples if applicable
   - Checklist of changes

**Pull Request Template**:

```markdown
## Description
Brief description of the changes.

## Related Issues
Closes #123

## Changes Made
- [ ] Feature 1
- [ ] Feature 2
- [ ] Updated documentation
- [ ] Added tests

## Screenshots
If applicable, add screenshots.

## Checklist
- [ ] Tests pass locally
- [ ] Code follows style guidelines
- [ ] Documentation updated
- [ ] No breaking changes (or documented)
```

4. **Wait for review** - Maintainers will review your PR
5. **Address feedback** - Make requested changes
6. **Merge** - Once approved, maintainers will merge

## Style Guidelines

### JavaScript/Node.js

- Use ES6+ features
- 2 spaces for indentation
- Semicolons required
- Use `const` over `let`, avoid `var`
- Use arrow functions for callbacks
- Use template literals for string interpolation

```javascript
// Good
const client = new Manaqr({ apiKey: 'key' });

const qr = await client.create({
  type: 'url',
  data: `https://example.com/${id}`
});

// Bad
var client = new Manaqr({ apiKey: 'key' });

client.create({
  type: 'url',
  data: 'https://example.com/' + id
}, function(err, qr) {
  // ...
});
```

### Python

- Follow PEP 8
- 4 spaces for indentation
- Use type hints
- Use f-strings for formatting
- Document with docstrings

```python
# Good
def create_qr(
    client: Client,
    url: str,
    name: str
) -> QRCode:
    """Create a QR code.
    
    Args:
        client: The manaqr client
        url: Target URL
        name: QR code name
        
    Returns:
        Created QR code object
    """
    return client.create(
        type='url',
        data=url,
        name=name
    )

# Bad
def create_qr(client, url, name):
    return client.create(type='url', data=url, name=name)
```

### Documentation

- Use clear, concise language
- Include code examples
- Add links to related sections
- Use proper markdown formatting
- Test all code examples

### Testing

- Write tests for new features
- Maintain high code coverage (>80%)
- Use descriptive test names
- Test edge cases and error conditions

```javascript
// Good
describe('QR Code Creation', () => {
  it('should create a URL QR code with valid data', async () => {
    const qr = await client.create({
      type: 'url',
      data: 'https://example.com'
    });
    
    expect(qr.id).toBeDefined();
    expect(qr.type).toBe('url');
    expect(qr.imageUrl).toMatch(/^https:\/\//);
  });
  
  it('should throw error with invalid URL', async () => {
    await expect(
      client.create({ type: 'url', data: 'invalid' })
    ).rejects.toThrow('Invalid URL');
  });
});
```

## Community

### Getting Help

- 💬 [GitHub Discussions](https://github.com/manaqr/docs/discussions) - Ask questions, share ideas
- 📧 Email: developers@manaqr.io
- 💼 [LinkedIn](https://linkedin.com/company/manaqr)
- 🐦 [Twitter](https://twitter.com/manaqr)

### Stay Updated

- Watch the repository for updates
- Subscribe to our [blog](https://blog.manaqr.io)
- Join our [newsletter](https://manaqr.io/newsletter)

### Recognition

Contributors are recognized in:
- README.md contributors section
- Release notes
- Annual contributor highlights

## License

By contributing, you agree that your contributions will be licensed under the same license as the project (MIT License).

## Questions?

Don't hesitate to ask! We're here to help:

- Create an issue with the `question` label
- Ask in [Discussions](https://github.com/manaqr/docs/discussions)
- Email: developers@manaqr.io

Thank you for contributing to manaqr! 🎉

---

**Previous**: [← FAQ](faq.md)
