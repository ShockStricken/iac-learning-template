# Contributing to IaC Learning Template

Thank you for your interest in contributing to the Infrastructure as Code Learning Template! This document provides guidelines and information for contributors.

## 🎯 Project Goals

This project aims to:
- Provide a comprehensive learning environment for IaC concepts
- Demonstrate modern infrastructure patterns and best practices
- Offer hands-on experience with Docker Compose, SOPS, and Task automation
- Maintain security-first approach to infrastructure management

## 🤝 How to Contribute

### Types of Contributions

We welcome:
- 🐛 Bug reports and fixes
- 📚 Documentation improvements
- ✨ New learning examples and scenarios
- 🔧 Task automation enhancements
- 🔐 Security improvements
- 📊 Monitoring and observability features

### Getting Started

1. **Fork the repository**
   ```bash
   # Click "Fork" on GitHub, then clone your fork
   git clone https://github.com/YOUR_USERNAME/iac-learning-template.git
   cd iac-learning-template
   ```

2. **Set up development environment**
   ```bash
   # Bootstrap the environment
   task bootstrap
   
   # Verify setup
   task health
   ```

3. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

4. **Make your changes**
   - Follow existing patterns and conventions
   - Add documentation for new features
   - Include examples where appropriate

5. **Test your changes**
   ```bash
   # Test basic functionality
   task examples:basic
   
   # Test advanced features
   task examples:advanced
   
   # Verify all examples work
   task examples:list
   ```

6. **Commit and push**
   ```bash
   git add .
   git commit -m "feat: add new learning example for X"
   git push origin feature/your-feature-name
   ```

7. **Create a Pull Request**
   - Use the GitHub interface to create a PR
   - Provide a clear description of your changes
   - Reference any related issues

## 📋 Development Guidelines

### Code Style

- **Task files**: Follow YAML best practices, use clear task names
- **Docker Compose**: Use consistent service naming and health checks
- **Documentation**: Use clear, concise language with emojis for visual appeal
- **Scripts**: Include error handling and clear output messages

### File Organization

```
├── .taskfiles/           # Task modules
│   ├── brew/             # macOS tool installation
│   ├── compose/          # Docker Compose operations
│   ├── sops/             # Secret management
│   └── examples/         # Learning examples
├── examples/             # Generated example configurations
├── secrets/              # Encrypted secrets (SOPS)
├── docker-compose.yml    # Main service definition
└── Taskfile.yml         # Main task configuration
```

### Adding New Examples

When adding learning examples:

1. **Create task in `.taskfiles/examples/Taskfile.yml`**
   ```yaml
   your-example:
     desc: "Description of what this example teaches"
     cmds:
       - task: generate  # Generate any needed files
       - |
         echo "🚀 Starting your example..."
         # Add implementation
   ```

2. **Add to examples list**
   ```yaml
   list:
     cmds:
       - |
         echo "📚 Advanced Examples:"
         echo "   • task examples:your-example - Your description"
   ```

3. **Include documentation**
   - Update README.md with the new example
   - Add any configuration files to `examples/`
   - Provide clear usage instructions

### Security Considerations

- **Never commit secrets**: Use SOPS encryption for all sensitive data
- **Follow principle of least privilege**: Minimize exposed ports and permissions
- **Update dependencies**: Keep Docker images and tools up to date
- **Validate inputs**: Add appropriate validation to scripts and tasks

## 🧪 Testing

### Manual Testing

```bash
# Test basic functionality
task examples:basic
open http://localhost:8080

# Test secret management
task sops:health
task examples:secrets

# Test monitoring
task examples:monitoring
open http://localhost:9090  # Prometheus
open http://localhost:3001  # Grafana

# Test advanced features
task examples:advanced
open http://localhost:8081  # Traefik dashboard
```

### Validation Checklist

- [ ] All services start without errors
- [ ] Health checks pass for all services
- [ ] SOPS encryption/decryption works
- [ ] Examples generate correctly
- [ ] Documentation is clear and accurate
- [ ] No secrets are committed in plain text

## 📝 Documentation

### Writing Style

- Use clear, beginner-friendly language
- Include practical examples
- Add emoji for visual organization
- Provide troubleshooting guidance
- Link to external resources where helpful

### Documentation Structure

- **README.md**: Main project documentation
- **Task descriptions**: Clear, actionable descriptions
- **Code comments**: Explain complex configurations
- **Examples**: Include usage examples in documentation

## 🐛 Reporting Issues

When reporting bugs:

1. **Search existing issues** first
2. **Use issue templates** if available
3. **Provide clear reproduction steps**
4. **Include system information**:
   - Operating system
   - Docker version
   - Task version
   - SOPS version
5. **Add relevant logs** and error messages

### Issue Labels

- `bug`: Something isn't working
- `enhancement`: New feature or improvement
- `documentation`: Documentation improvements
- `good first issue`: Good for newcomers
- `help wanted`: Community assistance needed

## 🏆 Recognition

Contributors will be:
- Listed in the project contributors
- Mentioned in release notes for significant contributions
- Given credit in documentation updates

## 📞 Getting Help

If you need help:

- 💬 [Start a discussion](https://github.com/ShockStricken/iac-learning-template/discussions)
- 🐛 [Open an issue](https://github.com/ShockStricken/iac-learning-template/issues)
- 📚 Check the [project documentation](https://github.com/ShockStricken/iac-learning-template/wiki)

## 📄 License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

Thank you for helping make Infrastructure as Code learning more accessible! 🚀
