# Contributing to CovViz

Thank you for your interest in contributing to CovViz! This document provides guidelines and instructions for contributing.

## Code of Conduct

This project adheres to the Contributor Covenant Code of Conduct. By participating, you are expected to uphold this code. Please report unacceptable behavior to support@icsam.dev.

## How to Contribute

### Reporting Bugs

Before creating a bug report, please check the [issue list](https://github.com/Samuel-Moussa/CovViz/issues) to ensure the issue hasn't already been reported.

**When filing a bug report, include:**
- Clear, descriptive title
- Exact steps to reproduce the problem
- Specific examples to demonstrate the steps
- Description of the observed behavior
- Explanation of the expected behavior
- Screenshots or error logs (if applicable)
- Your environment (browser, OS, QuestaSim version)
- CovViz version (shown in header)

### Suggesting Enhancements

Enhancement suggestions are tracked as GitHub issues. When creating an enhancement suggestion, include:
- Clear, descriptive title
- Detailed description of the suggested enhancement
- Specific examples to demonstrate the use case
- Explanation of why this enhancement would be useful
- List of similar features in other tools (if applicable)

### Pull Requests

1. **Fork the repository** and create your branch from `main`
2. **Create a feature branch**: `git checkout -b feature/your-feature-name`
3. **Make your changes** following the code style guidelines below
4. **Test your changes** thoroughly
5. **Commit your changes**: `git commit -m 'Add feature: description'`
6. **Push to your fork**: `git push origin feature/your-feature-name`
7. **Submit a pull request** with a clear description

## Development Setup

### Prerequisites
- Modern web browser (Chrome, Firefox, Safari, Edge)
- Text editor or IDE (VS Code recommended)
- Git

### Local Setup
```bash
# Clone the repository
git clone https://github.com/Samuel-Moussa/CovViz.git
cd CovViz

# Open in browser
open CovViz-Enhanced-Fixed.html
# or
firefox CovViz-Enhanced-Fixed.html
```

### Testing Changes
1. Open CovViz in browser
2. Load test coverage reports from `examples/` folder
3. Verify all features work as expected
4. Test with multiple browsers if possible
5. Check browser console for errors (F12)

## Code Style Guidelines

### JavaScript
- Use consistent indentation (2 spaces)
- Use meaningful variable names
- Add comments for complex logic
- Keep functions focused and single-purpose
- Use `const` by default, `let` for variables that change
- Avoid global variables

### HTML/CSS
- Use semantic HTML elements
- Keep CSS organized and commented
- Use CSS variables for theming
- Maintain responsive design principles
- Test on multiple screen sizes

### Documentation
- Use clear, concise language
- Include code examples where helpful
- Keep documentation up-to-date with code changes
- Use Markdown formatting consistently

## Commit Message Guidelines

Follow this format for commit messages:

```
<type>: <subject>

<body>

<footer>
```

**Type** should be one of:
- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, missing semicolons, etc.)
- `refactor`: Code refactoring without feature changes
- `perf`: Performance improvements
- `test`: Adding or updating tests
- `chore`: Build process, dependencies, or tooling changes

**Subject** should:
- Use imperative mood ("add feature" not "added feature")
- Not capitalize first letter
- Not end with a period
- Be 50 characters or less

**Body** should:
- Explain what and why, not how
- Wrap at 72 characters
- Be separated from subject by blank line

**Footer** should:
- Reference any related issues: `Fixes #123` or `Related to #456`

### Example
```
fix: correct bin status parsing for ZERO bins

The parser was incorrectly identifying zero-hit bins due to a regex
false-positive. This commit implements stricter pattern matching to
distinguish between bin names and coverpoint metric lines.

Fixes #42
```

## Pull Request Process

1. **Update documentation** if your changes affect user-facing features
2. **Update CHANGELOG.md** with a summary of your changes
3. **Test thoroughly** with multiple coverage report formats
4. **Ensure no console errors** in browser developer tools
5. **Request review** from maintainers
6. **Address feedback** and push updates to your branch
7. **Squash commits** if requested by maintainers

## Documentation

When contributing documentation:
- Use Markdown format
- Include code examples where helpful
- Keep language clear and concise
- Update table of contents if adding new sections
- Link to related documentation

## Testing

### Manual Testing Checklist
- [ ] Load sample coverage report from `examples/`
- [ ] Verify all dashboard sections render correctly
- [ ] Check KPI cards display correct values
- [ ] Verify alerts are triggered appropriately
- [ ] Test multi-file merge functionality
- [ ] Export to CSV and JSON
- [ ] Test on Chrome, Firefox, Safari, Edge
- [ ] Check responsive design on mobile
- [ ] Verify no console errors (F12)

### Browser Compatibility
CovViz should work on:
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## Performance Considerations

When making changes:
- Minimize DOM manipulation
- Avoid unnecessary re-renders
- Use efficient algorithms
- Test with large coverage reports (>100 MB)
- Monitor browser memory usage

## Security Considerations

- Don't store sensitive data in local storage
- Validate all user inputs
- Avoid eval() and similar unsafe functions
- Be cautious with external dependencies
- Report security issues privately to support@icsam.dev

## Release Process

Maintainers follow this process for releases:

1. Update CHANGELOG.md with all changes
2. Update version number in HTML file header
3. Create a git tag: `git tag v4.1`
4. Push tag to repository: `git push origin v4.1`
5. Create GitHub release with changelog
6. Announce release to community

## Questions?

- Check [documentation](docs/)
- Search [existing issues](https://github.com/Samuel-Moussa/CovViz/issues)
- Email: support@icsam.dev
- Website: https://icsam.dev

## Recognition

Contributors will be recognized in:
- CHANGELOG.md
- GitHub contributors page
- Project website

Thank you for contributing to CovViz!
