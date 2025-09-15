---
layout: single
title: "Using GitHub Spec Kit with Opencode CLI"
tags:
   - ai
   - github
   - api
   - development
   - specifications
   - software-engineering
   - productivity
   - opencode
   - cli
categories:
   - tech
   - programming
   - ai-tools
excerpt: "Learn how to use GitHub Spec Kit with Opencode CLI for enhanced spec-driven development. This feature request integration provides advanced AI assistance for specification management."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/09/github-spec-kit.png
  overlay_image: /assets/2025/09/github-spec-kit.png
---

**TL;DR**:

- **GitHub Spec Kit**: AI-powered specification-driven development tool with constitution-based principles
- **Opencode CLI Integration**: Feature request for enhanced AI assistance in spec development
- **Key Features**: Spec generation, AI agent integration, constitution compliance, parallel task execution
- **Prerequisites**: Opencode CLI, Astral UV, Git

<script type="module">
  import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10.6.1/dist/mermaid.esm.min.mjs';
  mermaid.initialize({ startOnLoad: true });
</script>

# Using GitHub Spec Kit with Opencode CLI: Feature Request Integration Guide

GitHub Spec Kit is a powerful toolkit that enables specification-driven development, allowing organizations to focus on product scenarios rather than undifferentiated code. This guide covers the integration with Opencode CLI, which is currently available as a feature request in the development branch and will be included in future official releases of GitHub Spec Kit.

**Important Note**: This integration is currently a feature request and not yet available in the official GitHub Spec Kit release. It requires using the development fork by [@aemr3](https://github.com/aemr3) until the feature is merged into the main repository. The integration will be officially supported in upcoming releases.

## What is GitHub Spec Kit?

Before diving into the Opencode integration, let's understand what makes GitHub Spec Kit valuable:

### Core Features
- **AI-Powered Spec Generation**: Automatically creates detailed specifications from high-level requirements
- **Constitution-Based Development**: Follows proven architectural principles and anti-abstraction rules
- **Multi-Agent Support**: Works with Claude, Gemini, Copilot, and soon Opencode CLI
- **Parallel Task Execution**: Breaks down complex projects into manageable, executable tasks
- **Research Integration**: Automatically researches technologies and implementation details

### Development Phases
- **Phase 0**: Research and technology evaluation
- **Phase 1**: Data modeling, contracts, and quickstart guides
- **Phase 2**: Task breakdown and implementation planning

### Constitution Principles
The Spec Kit follows a "constitution" of development principles:
- **Simplicity First**: Limit projects to ≤3 components, avoid future-proofing
- **Anti-Abstraction**: Use frameworks directly, maintain single model representations
- **Integration-First**: Define contracts and tests before implementation
- **Library-First**: Every feature begins as a standalone library

## Why Use Opencode CLI?

Opencode CLI offers unique advantages for Spec Kit integration:

### Advanced AI Capabilities
- **Enhanced Code Understanding**: Superior parsing and comprehension of complex codebases
- **Context-Aware Generation**: Better handling of project-specific contexts and requirements
- **Multi-Language Support**: Extensive support for various programming languages and frameworks
- **Enterprise-Ready**: Designed for professional development environments

### Integration Benefits
- **Seamless Workflow**: Direct integration with Spec Kit's specification pipeline
- **Automated Research**: Enhanced research capabilities for technology evaluation
- **Quality Assurance**: Improved validation of specifications and implementation plans
- **Scalability**: Better performance with large-scale projects and complex requirements

## Prerequisites

Before starting the integration, ensure you have the following:

### System Requirements
- **Opencode CLI** (latest development version recommended)
- **Astral UV** package manager (for Spec Kit installation)
- **Git** for repository operations
- **Python 3.8+** (for UV compatibility)
- **At least 1GB free disk space** (for configurations and temporary files)
- **Supported OS**: Linux, macOS, or Windows (WSL2)

### Verify Prerequisites

```bash
# Check Opencode CLI installation
opencode --version

# Check UV installation
uv --version

# Check Python version
python --version
# Should show Python 3.8+

# Check Git
git --version
```

## Step-by-Step Integration Guide

### Step 1: Prepare Your Environment

```bash
# Ensure you're in a clean directory
mkdir spec-kit-opencode-integration
cd spec-kit-opencode-integration

# Verify Opencode CLI is accessible
opencode --version

# Verify UV is installed
uv --version
```

### Step 2: Initialize Spec Project

#### Option A: New Project Directory

To create a new project with GitHub Spec Kit configured for Opencode CLI:

```bash
uvx --from git+https://github.com/aemr3/spec-kit.git specify init --ai opencode <PROJECT_NAME>
cd <PROJECT_NAME>
```

Replace `<PROJECT_NAME>` with your desired project name.

#### Option B: Current Directory

If you prefer to initialize in your current working directory:

```bash
uvx --from git+https://github.com/aemr3/spec-kit.git specify init --ai opencode --here
```

### Step 3: Verify Configuration

```bash
# Check that the project was initialized correctly
ls -la

# Verify Opencode CLI configuration
opencode config list

# Check for AGENTS.md file
ls -la AGENTS.md
```

### Step 4: Test Integration

```bash
# Launch Opencode CLI in the project
opencode

# Within Opencode CLI, test Spec Kit commands
/specify Create a simple web application with user authentication

/plan Use React for frontend, Node.js for backend, PostgreSQL for database

/tasks
```

## Integration with AI Applications

### Opencode CLI Integration

The integration allows you to use Spec Kit commands directly within Opencode CLI:

1. **Launch Opencode CLI** in your configured project directory
2. **Use Spec Kit Commands**:
   - `/specify` - Create specifications
   - `/plan` - Generate implementation plans
   - `/tasks` - Break down into executable tasks
   - `/implement` - Execute implementation

### Configuration for Opencode CLI

Ensure your Opencode CLI settings support the integration:

```bash
# Check current configuration
opencode config list

# Set appropriate model for spec work
opencode config set model opencode-pro

# Enable long context if available
opencode config set context-window 128k
```

### Other Compatible Tools

While primarily designed for Opencode CLI, the setup is compatible with:
- **Claude Code**: Use `--ai claude` instead of `--ai opencode`
- **GitHub Copilot**: Use `--ai copilot` for VS Code integration
- **Cursor**: Use `--ai cursor` for Cursor IDE integration

## Advanced Configuration

### Environment Variables

```bash
# Spec Kit Configuration
export SPEC_KIT_AI_AGENT=opencode
export SPEC_KIT_SCRIPT_TYPE=sh
export SPEC_KIT_DEBUG=true

# Opencode CLI Configuration
export OPENCODE_MODEL=opencode-pro
export OPENCODE_CONTEXT_WINDOW=128k
export OPENCODE_TIMEOUT=600000  # 10 minutes
```

### Custom Configuration Files

Create a `.env` file in your project root:

```bash
# .env file for Spec Kit + Opencode integration
SPEC_KIT_AI_AGENT=opencode
SPEC_KIT_CONSTITUTION_STRICT=true
OPENCODE_MODEL=opencode-pro
OPENCODE_MAX_TOKENS=32000
```

### Performance Tuning

```bash
# Optimize for large specifications
export OPENCODE_MAX_TOKENS=64000
export SPEC_KIT_RESEARCH_PARALLEL=true
export SPEC_KIT_TASK_BATCH_SIZE=5
```

## Troubleshooting Common Issues

### Initialization Failures

```bash
# If UVX fails to download
uv cache clean
uvx --from git+https://github.com/aemr3/spec-kit.git specify init --ai opencode --debug

# If Git clone fails
git config --global http.sslVerify false
# Then retry the command
```

### Configuration Issues

```bash
# If AGENTS.md is not generated correctly
ls -la AGENTS.md
# Check if the file exists and has content

# Reset Opencode CLI configuration
opencode config reset
opencode config set model opencode-pro
```

### Integration Problems

```bash
# If Opencode CLI doesn't recognize Spec Kit commands
opencode config list
# Verify configuration is loaded

# Restart Opencode CLI
pkill -f opencode
opencode
```

### Template Issues

```bash
# If templates fail to download
# This is expected until the feature is merged
# Use the development fork as shown in commands above

# Check available templates
uvx --from git+https://github.com/aemr3/spec-kit.git specify check
```

## Usage Examples

### Basic Spec Creation

```bash
# Create a simple API specification
/specify Build a REST API for user management with authentication

# Generate implementation plan
/plan Use FastAPI for backend, PostgreSQL for data, JWT for auth

# Break into tasks
/tasks
```

### Advanced Research Integration

```bash
# Research and plan a complex system
/specify Create a microservices architecture for an e-commerce platform

/plan Research Kubernetes deployment, service mesh, API gateway

# The system will automatically research technologies
```

### Constitution Compliance

```bash
# Check constitution compliance
/specify Build a monolithic application with 6 services

# System will warn about violating simplicity principle
# Suggest consolidating to fewer services
```

## Performance Optimization

### For Large Projects

```bash
# Enable parallel research
export SPEC_KIT_RESEARCH_PARALLEL=true

# Increase Opencode context window
export OPENCODE_CONTEXT_WINDOW=256k

# Batch task processing
export SPEC_KIT_TASK_BATCH_SIZE=10
```

### Memory Management

```bash
# For systems with limited RAM
export OPENCODE_MAX_TOKENS=16000
export SPEC_KIT_MEMORY_LIMIT=512MB

# Clean up old research data
rm -rf .spec-kit/cache/old_research/
```

### Network Optimization

```bash
# Use local model if available
export OPENCODE_MODEL=local-opencode

# Reduce API calls
export SPEC_KIT_CACHE_RESEARCH=true
export SPEC_KIT_REUSE_RESEARCH=7days
```

## Security Considerations

### API Key Protection

```bash
# Never commit API keys
echo ".env" >> .gitignore

# Use environment variables
export OPENCODE_API_KEY="your-secure-key"
export SPEC_KIT_API_KEYS="encrypted"

# Rotate keys regularly
opencode config rotate-key
```

### Data Isolation

```bash
# Keep specs in separate directories
mkdir specs/
mv *.md specs/

# Encrypt sensitive specifications
gpg --encrypt specs/confidential-spec.md
```

### Access Controls

```bash
# Set proper permissions
chmod 600 AGENTS.md
chmod 700 .spec-kit/

# Use SSH keys for Git
ssh-keygen -t ed25519 -C "spec-kit-opencode-integration"
```

## Maintenance and Updates

### Regular Maintenance

```bash
# Update Spec Kit (when available)
uvx --from git+https://github.com/github/spec-kit.git specify update

# Update Opencode CLI
opencode update

# Clean caches
uv cache clean
opencode cache clear
```

### Constitution Updates

```bash
# Check for constitution updates (when available)
curl -s https://raw.githubusercontent.com/github/spec-kit/main/memory/constitution.md > constitution_latest.md
diff memory/constitution.md constitution_latest.md

# Update if needed
cp constitution_latest.md memory/constitution.md
```

### Backup and Recovery

```bash
# Backup configuration
tar -czf spec-kit-backup-$(date +%Y%m%d).tar.gz AGENTS.md .spec-kit/ specs/

# Restore from backup
tar -xzf spec-kit-backup-20250916.tar.gz
```

## Integration Examples

### Development Workflow Integration

```bash
# Git integration
/specify Implement CI/CD pipeline for the application

# Code review preparation
/specify Add comprehensive test coverage for user authentication

# Documentation generation
/specify Create API documentation for all endpoints
```

### Team Collaboration

```bash
# Multi-developer specs
/specify Design microservices architecture for team of 5 developers

# Sprint planning
/specify Break down Q4 features into 2-week sprints

# Knowledge sharing
/specify Document development best practices and coding standards
```

### CI/CD Integration

```bash
# Automated spec validation
/specify Create automated testing pipeline for specifications

# Deployment specs
/specify Design blue-green deployment strategy for zero-downtime updates
```

## Monitoring and Analytics

### Usage Tracking

```bash
# Monitor Spec Kit usage
tail -f .spec-kit/logs/usage.log

# Track Opencode CLI metrics
opencode stats

# Analyze constitution compliance
grep "constitution" .spec-kit/logs/*.log
```

### Performance Metrics

```bash
# Spec generation time
time /specify Create a complex enterprise application spec

# Research efficiency
grep "research" .spec-kit/logs/performance.log

# Task completion rates
/spec-kit/scripts/analyze_tasks.py
```

### Quality Assurance

```bash
# Validate specifications
/spec-kit/scripts/validate_specs.py specs/

# Check constitution compliance
/spec-kit/scripts/check_constitution.py

# Generate quality reports
/spec-kit/scripts/generate_report.py
```

## Conclusion

The integration of GitHub Spec Kit with Opencode CLI represents the future of AI-powered specification-driven development. While currently available as a feature request in the development branch, this integration promises to deliver enhanced AI assistance for specification management and implementation.

**Important**: This feature is currently in development and requires using the [@aemr3](https://github.com/aemr3) fork. It will be officially available in future releases of GitHub Spec Kit.

### Key Benefits of This Integration

- **Advanced AI Assistance**: Opencode's sophisticated models provide superior code generation and understanding
- **Constitution Compliance**: Ensures all development follows proven architectural principles
- **Parallel Processing**: Efficiently handles complex projects through task breakdown
- **Research Automation**: Automatically researches technologies and implementation details
- **Enterprise-Ready**: Designed for professional development environments

### Next Steps

1. **Monitor Development**: Follow the [GitHub Spec Kit repository](https://github.com/github/spec-kit) for official release updates
2. **Test the Feature**: Use the development fork commands provided above
3. **Provide Feedback**: Contribute to the feature request discussion
4. **Prepare Environment**: Ensure your setup is ready for the official release
5. **Stay Updated**: Watch for announcements about Opencode CLI support

The combination of GitHub Spec Kit's specification-driven approach with Opencode CLI's advanced AI capabilities will significantly enhance development productivity and code quality.

## Further Reading

### Official Documentation
- **[GitHub Spec Kit Repository](https://github.com/github/spec-kit)** - Complete source code and documentation
- **[Spec-Driven Development Guide](https://github.com/github/spec-kit/blob/main/spec-driven.md)** - Comprehensive methodology
- **[Opencode CLI Documentation](https://github.com/opencode/cli)** - CLI usage and configuration

### Related Articles
- **[Installing Astral UV](/posts/2025-09-04-install-astral-uv)** - Package manager setup
- **[Using GitHub Spec Kit with QWEN Code CLI](/posts/2025-09-15-use-github-spec-kit-with-qwen-code-cli)** - Alternative integration
- **[Mastering Spec-Driven Development](/posts/2025-09-08-mastering-spec-driven-development-with-github-spec-kit)** - Advanced Spec Kit usage

### Community Resources
- **[GitHub Spec Kit Issues](https://github.com/github/spec-kit/issues)** - Bug reports and feature requests
- **[Opencode CLI Discussions](https://github.com/opencode/cli/discussions)** - Community support
- **[Spec-Driven Development Community](https://github.com/github/spec-kit/discussions)** - Best practices and tips

For the latest updates on Opencode CLI support, follow the [GitHub Spec Kit repository](https://github.com/github/spec-kit) and participate in the community discussions.