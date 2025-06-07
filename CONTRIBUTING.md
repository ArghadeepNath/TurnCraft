# 🤝 Contributing to the Git-Backed Minecraft World Hosting System

Thank you for your interest in contributing to our project! This document provides guidelines and instructions for contributing to the various components of the Git-Backed Turn-Based Minecraft World Hosting System.

## 🌟 Code of Conduct

- Be respectful and inclusive in all interactions
- Provide constructive feedback
- Focus on what is best for the community
- Show empathy towards other community members

## 🛠️ Development Setup

### Prerequisites

- Java Development Kit (JDK) 17 or newer
- Git
- IDE with Gradle support (IntelliJ IDEA recommended)
- Node.js 14+ (for Coordination Server)
- Python 3.8+ (alternative for Coordination Server)
- Minecraft Forge MDK

## 📝 Coding Standards

### Git World Sync Mod (Java)

- Follow standard Java naming conventions
- Use 4 spaces for indentation
- Add JavaDoc comments for public methods
- Follow Forge's event handling best practices
- Use `final` where appropriate
- Prefer composition over inheritance

### Coordination Server

- Follow PEP 8 style guide
- Use type hints
- Write docstrings for modules, classes, and functions
- Use snake_case for variables and functions

## 🔄 Git Workflow

1. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes**
   - Write clean, readable code
   - Include comments where necessary
   - Add tests for your changes

3. **Commit your changes**
   ```bash
   git commit -m "feat: add new feature" # Follow conventional commits
   ```

4. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

5. **Open a pull request**
   - Provide a clear description of the changes
   - Reference any related issues
   - Wait for code review

### Branch Naming Conventions

- `feature/` - New features or enhancements
- `bugfix/` - Bug fixes
- `docs/` - Documentation changes
- `test/` - Testing related changes
- `refactor/` - Code refactoring
- `chore/` - Maintenance tasks

## 📋 Pull Request Process

1. Ensure all tests pass
2. Update documentation if necessary
3. Add your changes to the CHANGELOG.md under Unreleased section
4. Get approval from at least one maintainer
5. Once approved, a maintainer will merge your PR

## 🐛 Reporting Issues

When reporting issues, please include:

1. **Description** - Clear and concise explanation of the issue
2. **Steps to reproduce** - Detailed steps to reproduce the bug
3. **Expected behavior** - What you expected to happen
4. **Actual behavior** - What actually happened
5. **Environment details**:
   - Minecraft version
   - Forge version
   - Operating system
   - Mod version
   - Other mods installed (if applicable)
   - Java version
6. **Screenshots** - If applicable
7. **Log files** - Relevant logs (latest.log, debug.log)

## 🔍 Code Review Process

All submissions require review. We use GitHub pull requests for this purpose:

1. Maintainers will review your code for:
   - Functionality
   - Performance
   - Security considerations
   - Code style
   - Test coverage
2. Address any feedback or requested changes
3. Once approved, your code will be merged

## ✅ Testing Guidelines

### Git World Sync Mod

- Write unit tests for utility classes
- Write integration tests for Git operations
- Test both server and client functionality
- Test with various world sizes
- Test error handling scenarios

### Coordination Server

- Write unit tests for services and controllers
- Write integration tests for API endpoints
- Test error handling and edge cases
- Test authentication and authorization

## 📚 Documentation Guidelines

- Update README.md with any new features or changes
- Document public APIs with proper comments
- Update wiki pages if applicable
- Include example usage for new features
- Keep code comments up-to-date

## 🏆 Recognition

Contributors will be recognized in:
- CONTRIBUTORS.md file
- Release notes
- Project website (when available)

## 📣 Communication Channels

- **GitHub Discussions** - For feature requests and general discussion
- **Discord Server** - For real-time communication (link in README)
- **Issue Tracker** - For bug reports and feature tracking

## 🛠️ Component-Specific Guidelines

### Git World Sync Mod

- Ensure backward compatibility where possible
- Test with both client and server environments
- Consider performance impacts on various hardware specifications
- Follow Forge's event handling patterns

### Coordination Server

- Design RESTful API endpoints
- Document API changes in OpenAPI format
- Follow security best practices for authentication
- Implement proper error handling and status codes

## 🔐 Security Considerations

- Never commit API keys or credentials
- Use secure authentication methods
- Sanitize user inputs
- Report security vulnerabilities privately to maintainers

## 🙏 Thank You

Your contributions help make this project better for everyone! We appreciate the time and effort you put into improving the Git-Backed Turn-Based Minecraft World Hosting System.

---

<div align="center">
<i>Happy coding! 💻</i>
</div>
