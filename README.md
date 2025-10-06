# 🏗️ Infrastructure as Code Learning Template

[![Task](https://img.shields.io/badge/Task-Enabled-brightgreen?logo=task)](https://taskfile.dev)
[![SOPS](https://img.shields.io/badge/SOPS-Encrypted-blue?logo=mozilla)](https://github.com/mozilla/sops)
[![Docker](https://img.shields.io/badge/Docker-Compose-blue?logo=docker)](https://docs.docker.com/compose/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

A comprehensive learning template for Infrastructure as Code (IaC) concepts using Docker Compose, SOPS secret management, and Task automation. Perfect for learning modern infrastructure patterns and best practices.

## 🎆 Features

- **🔐 Secret Management**: SOPS encryption with Age keys
- **🚀 Task Automation**: Comprehensive task runner configuration
- **🐳 Multi-Service Stack**: Web, API, Database, Cache, Monitoring
- **📊 Monitoring**: Prometheus metrics + Grafana dashboards
- **🌐 Load Balancing**: Traefik reverse proxy (advanced profile)
- **📦 Package Management**: Homebrew automation for macOS
- **📁 Volume Persistence**: Data persistence across container restarts
- **🔗 Service Discovery**: Internal networking and communication

## 🚀 Quick Start

### Prerequisites

- **macOS**: Run `task brew:install` (installs all required tools)
- **Other platforms**: Install manually:
  - [Docker](https://docs.docker.com/get-docker/)
  - [Docker Compose](https://docs.docker.com/compose/install/)
  - [Task](https://taskfile.dev/installation/)
  - [SOPS](https://github.com/mozilla/sops)
  - [Age](https://github.com/FiloSottile/age)
  - [direnv](https://direnv.net/docs/installation.html)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/ShockStricken/iac-learning-template.git
cd iac-learning-template

# 2. Bootstrap the environment (macOS)
task bootstrap

# 3. Edit secrets (will create and encrypt)
task sops:edit -- secrets/secret.sops.env

# 4. Start the basic stack
task examples:basic

# 5. Open in browser
open http://localhost:8080
```

## 📊 Service Stack

| Service | Port | Description | Health Check |
|---------|------|-------------|-------------|
| **Web** | 8080 | Nginx static content | ✅ HTTP |
| **API** | 3000 | Node.js REST API | ✅ HTTP |
| **Database** | 5432 | PostgreSQL 15 | ✅ pg_isready |
| **Cache** | 6379 | Redis 7 | ✅ ping |
| **Prometheus** | 9090 | Metrics collection | ✅ HTTP |
| **Grafana** | 3001 | Dashboards | ✅ HTTP |
| **Traefik** | 80/8081 | Load balancer (advanced) | ✅ HTTP |

## 🔐 Secret Management

This template uses **SOPS** (Secrets OPerationS) with **Age** encryption for secure secret management.

### Key Commands

```bash
# Generate Age encryption key
task sops:keygen

# Edit encrypted secrets file
task sops:edit -- secrets/secret.sops.env

# View decrypted secrets (for debugging)
task sops:decrypt -- secrets/secret.sops.env

# Check SOPS health
task sops:health

# Encrypt any unencrypted .sops.* files
task sops:encrypt
```

### Secret Integration

Secrets are automatically decrypted and injected into Docker Compose:

```yaml
# docker-compose.yml
environment:
  - POSTGRES_PASSWORD=${DATABASE_PASSWORD}  # From SOPS
  - APP_SECRET_KEY=${APP_SECRET_KEY}        # From SOPS
```

## 🚀 Learning Examples

### Basic Examples

```bash
# Start simple web + database stack
task examples:basic

# Explore secret management
task examples:secrets

# Add monitoring (Prometheus + Grafana)
task examples:monitoring
```

### Intermediate Examples

```bash
# Learn Docker networking
task examples:networking

# Explore data persistence
task examples:persistence

# Practice horizontal scaling
task examples:scaling
```

### Advanced Examples

```bash
# Full stack with load balancer
task examples:advanced

# Backup and recovery strategies
task examples:backup

# Security hardening patterns
task examples:security
```

## 🚀 Task Automation

This project uses [Task](https://taskfile.dev) for automation. View all available tasks:

```bash
# List all tasks
task --list

# Core operations
task bootstrap          # Full setup
task health            # System health check
task clean             # Clean up resources

# Docker Compose operations
task compose:up        # Start stack
task compose:down      # Stop stack
task compose:ps        # List services
task compose:logs      # View logs
task compose:restart   # Restart service

# Secret management
task sops:keygen       # Generate Age key
task sops:edit         # Edit secrets
task sops:health       # Check SOPS status

# Learning examples
task examples:list     # List all examples
task examples:basic    # Basic stack
task examples:advanced # Advanced features

# macOS tool installation
task brew:install      # Install CLI tools
task brew:check        # Check tool status
```

## 🌐 Development Workflow

### 1. Environment Setup

```bash
# Automatic environment loading with direnv
echo 'eval "$(direnv hook zsh)"' >> ~/.zshrc
source ~/.zshrc

# The .envrc file automatically sets:
export SOPS_AGE_KEY_FILE="$(pwd)/.secrets/age.key"
export COMPOSE_FILE="$(pwd)/docker-compose.yml"
```

### 2. Service Profiles

```bash
# Default profile (basic services)
task compose:up

# Utilities profile (adds backup services)
COMPOSE_PROFILES=utilities task compose:up

# Advanced profile (adds load balancer)
COMPOSE_PROFILES=advanced task compose:up
```

### 3. Scaling and Load Testing

```bash
# Scale application horizontally
docker-compose up -d --scale app=3

# Scale web tier
docker-compose up -d --scale web=2

# Check scaling status
task compose:ps
```

## 📊 Monitoring and Observability

### Prometheus Metrics

Access Prometheus at http://localhost:9090

- Container metrics (cAdvisor)
- Application metrics (custom endpoints)
- Infrastructure health monitoring

### Grafana Dashboards

Access Grafana at http://localhost:3001

- **Default login**: admin / [encrypted in SOPS]
- Pre-configured Prometheus datasource
- Infrastructure overview dashboard
- Application performance monitoring

## 🔗 Networking Architecture

```
┌──────────────────────────────────────────────────────┐
│                     app-network                      │
│  ┏━━━━━━━┓  ┏━━━━━━━┓  ┏━━━━━━━━━━┓  ┏━━━━━━━┓  │
│  ┃  Web  ┃  ┃  App  ┃  ┃ Database ┃  ┃ Cache ┃  │
│  ┃ :8080 ┃  ┃ :3000 ┃  ┃   :5432  ┃  ┃ :6379 ┃  │
│  ┗━━━━━━━┛  ┗━━━━━━━┛  ┗━━━━━━━━━━┛  ┗━━━━━━━┛  │
└──────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────┐
│                 monitoring-network                   │
│    ┏━━━━━━━━━━━━━━┓    ┏━━━━━━━━━━━━━━┓    │
│    ┃  Prometheus  ┃    ┃   Grafana   ┃    │
│    ┃    :9090     ┃    ┃    :3001    ┃    │
│    ┗━━━━━━━━━━━━━━┛    ┗━━━━━━━━━━━━━━┛    │
└──────────────────────────────────────────────────────┘
```

## 📁 Data Persistence

### Named Volumes

- **postgres-data**: Database persistence
- **redis-data**: Cache persistence  
- **grafana-data**: Dashboard configuration
- **prometheus-data**: Metrics storage

### Volume Management

```bash
# List volumes
docker volume ls

# Inspect volume
docker volume inspect iac-learning-template_postgres-data

# Backup volume
docker run --rm -v iac-learning-template_postgres-data:/data -v $(pwd)/backups:/backup alpine tar czf /backup/postgres-backup.tar.gz -C /data .

# Restore volume
docker run --rm -v iac-learning-template_postgres-data:/data -v $(pwd)/backups:/backup alpine tar xzf /backup/postgres-backup.tar.gz -C /data
```

## 🔒 Security Best Practices

### Secret Management

- ✅ SOPS encryption for all sensitive data
- ✅ Age key-based encryption (modern, secure)
- ✅ No plain text secrets in repository
- ✅ Automated secret injection into containers

### Network Security

- ✅ Isolated Docker networks
- ✅ Internal service communication
- ✅ Minimal port exposure
- ✅ Health check integration

### Container Security

- ✅ Non-root user execution where possible
- ✅ Read-only file systems (selected services)
- ✅ Resource limits and constraints
- ✅ Regular image updates

## 🔧 Troubleshooting

### Common Issues

**SOPS decryption fails**
```bash
# Check Age key exists
ls -la .secrets/age.key

# Verify SOPS configuration
task sops:health

# Regenerate Age key if needed
task sops:keygen
```

**Services won't start**
```bash
# Check Docker daemon
docker info

# View service logs
task compose:logs -- <service-name>

# Check service health
docker-compose ps
```

**Port conflicts**
```bash
# Find process using port
lsof -i :8080

# Kill process
kill -9 <PID>

# Or change ports in docker-compose.yml
```

### Health Checks

```bash
# Overall system health
task health

# SOPS health
task sops:health

# Docker Compose status
task compose:ps

# Individual service health
docker-compose exec web wget --spider http://localhost/
docker-compose exec database pg_isready -U iac_user
docker-compose exec cache redis-cli ping
```

## 📚 Learning Resources

### Infrastructure as Code

- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [SOPS Secret Management](https://github.com/mozilla/sops)
- [Task Runner](https://taskfile.dev/)
- [Age Encryption](https://github.com/FiloSottile/age)

### Monitoring

- [Prometheus Getting Started](https://prometheus.io/docs/prometheus/latest/getting_started/)
- [Grafana Tutorials](https://grafana.com/tutorials/)
- [Docker Metrics with cAdvisor](https://github.com/google/cadvisor)

### Security

- [Docker Security Best Practices](https://docs.docker.com/engine/security/)
- [Container Security](https://kubernetes.io/docs/concepts/security/)
- [OWASP Container Security](https://owasp.org/www-project-container-security/)

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

If you have questions or need help:

- 🐛 [Open an issue](https://github.com/ShockStricken/iac-learning-template/issues)
- 💬 [Start a discussion](https://github.com/ShockStricken/iac-learning-template/discussions)
- 📚 [Read the documentation](https://github.com/ShockStricken/iac-learning-template/wiki)

---

**🎉 Happy learning!** This template provides a solid foundation for understanding Infrastructure as Code concepts. Start with the basic examples and gradually work your way up to advanced patterns.
