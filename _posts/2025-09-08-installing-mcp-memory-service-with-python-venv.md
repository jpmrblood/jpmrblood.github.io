---
layout: single
title: Installing MCP Memory Service with Python venv on Local Machine
tags:
  - mcp-memory-service
  - python
  - venv
  - claude-desktop
  - ai-memory
  - semantic-search
  - local-installation
categories:
  - tech
  - ai
  - programming
  - tutorial
excerpt: "Complete step-by-step guide to install MCP Memory Service using Python virtual environment on your local machine. Enable semantic memory search for Claude Desktop and other AI applications."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/08/robotic-ai.png
  overlay_image: /assets/2025/08/robotic-ai.png
---

**TL;DR**:

- **MCP Memory Service**: Universal AI memory service with semantic search for Claude Desktop, VS Code, and 13+ AI applications
- **Python venv Installation**: Isolated environment setup for clean, conflict-free installation
- **Key Features**: Semantic search, multi-client support, autonomous consolidation, SQLite-vec backend
- **Prerequisites**: Python 3.8+, git, uv package manager

<script type="module">
  import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10.6.1/dist/mermaid.esm.min.mjs';
  mermaid.initialize({ startOnLoad: true });
</script>

# Installing MCP Memory Service with Python venv: Complete Local Setup Guide

The MCP Memory Service is a powerful universal memory service that provides semantic search and persistent storage for AI assistants. It works seamlessly with Claude Desktop, VS Code, Cursor, and over 13 other AI applications, enabling them to maintain context across conversations and access previously stored knowledge.

This comprehensive guide will walk you through installing the MCP Memory Service using Python's virtual environment (venv) on your local machine. This approach ensures a clean, isolated installation that won't conflict with your system Python or other projects.

## What is MCP Memory Service?

Before diving into the installation, let's understand what makes this service valuable:

### Core Features
- **Semantic Memory Search**: Uses vector embeddings for intelligent, natural language search
- **Multi-Client Support**: Works with Claude Desktop, Claude Code, VS Code, Cursor, and more
- **Autonomous Consolidation**: Automatically organizes and consolidates memories
- **Flexible Storage**: Supports SQLite-vec, ChromaDB, and Cloudflare backends
- **Time-Based Queries**: Search memories using natural language ("yesterday", "last week")
- **Tag-Based Organization**: Smart categorization and tagging system

### Why Use Python venv?
- **Isolation**: Keeps dependencies separate from system Python
- **Reproducibility**: Ensures consistent environment across different machines
- **Clean Uninstall**: Easy to remove if needed
- **Version Control**: Prevents conflicts with other Python projects

## Prerequisites

Before starting the installation, ensure you have the following:

### System Requirements
- **Python 3.8 or higher** (Python 3.11+ recommended)
- **Git** for cloning the repository
- **uv package manager** (modern Python package manager)
- **At least 2GB free disk space** (for models and data)
- **Supported OS**: Linux, macOS, or Windows (WSL2)

### Verify Prerequisites

```bash
# Check Python version
python --version
# Should show Python 3.8+ (preferably 3.11+)

# Check if git is installed
git --version

# Install uv if not present
curl -LsSf https://astral.sh/uv/install.sh | sh
# Or using pip
pip install uv

# Verify uv installation
uv --version
```

## Step-by-Step Installation Guide

### Step 1: Clone the Repository

```bash
# Navigate to your preferred directory
cd ~/projects  # or any directory you prefer

# Clone the MCP Memory Service repository
git clone https://github.com/doobidoo/mcp-memory-service.git

# Navigate into the project directory
cd mcp-memory-service
```

### Step 2: Create Python Virtual Environment

```bash
# Create a virtual environment in the project directory
python -m venv venv

# Activate the virtual environment
# On Linux/macOS:
source venv/bin/activate

# On Windows:
# venv\Scripts\activate

# Verify activation (you should see (venv) in your prompt)
which python
# Should point to: /path/to/mcp-memory-service/venv/bin/python
```

### Step 3: Install Dependencies with uv

```bash
# Install dependencies using uv (recommended)
uv pip install -e .

# Alternative: Install with pip
# pip install -e .
```

### Step 4: Initialize the Service

```bash
# Run the installation script
python install.py

# Or manually initialize
uv run memory init
```

### Step 5: Configure Storage Backend

The service supports multiple storage backends. For local installation, SQLite-vec is recommended:

```bash
# Set environment variables for SQLite-vec backend
export MCP_MEMORY_STORAGE_BACKEND=sqlite_vec

# Optional: Enable HTTP API for web dashboard
export MCP_HTTP_ENABLED=true
export MCP_HTTP_PORT=8000

# Optional: Set API key for security
export MCP_API_KEY="your-secure-api-key-here"
```

### Step 6: Test the Installation

```bash
# Test basic functionality
uv run memory health

# Store a test memory
uv run memory store "Test memory: Installation completed successfully"

# Search for the memory
uv run memory recall "installation test"

# Check service status
uv run memory status
```

## Integration with AI Applications

### Claude Desktop Integration

1. **Locate Claude Desktop Configuration**:
   ```bash
   # On macOS
   ~/Library/Application Support/Claude/claude_desktop_config.json
   
   # On Windows
   %APPDATA%\Claude\claude_desktop_config.json
   
   # On Linux
   ~/.config/Claude/claude_desktop_config.json
   ```

2. **Add MCP Configuration**:
   ```json
   {
     "mcpServers": {
       "memory": {
         "command": "uv",
         "args": ["--directory", "/path/to/mcp-memory-service", "run", "memory", "server"],
         "env": {
           "MCP_MEMORY_STORAGE_BACKEND": "sqlite_vec"
         }
       }
     }
   }
   ```

3. **Restart Claude Desktop** to load the new configuration.

### VS Code Integration

1. **Install the MCP extension** for VS Code
2. **Configure the memory service** in VS Code settings
3. **Restart VS Code** to enable the integration

### Cursor Integration

1. **Open Cursor settings**
2. **Add MCP server configuration**
3. **Point to your local MCP Memory Service**
4. **Restart Cursor** to activate

## Advanced Configuration

### Environment Variables

```bash
# Storage Configuration
export MCP_MEMORY_STORAGE_BACKEND=sqlite_vec  # or chromadb, cloudflare
export MCP_MEMORY_DB_PATH=./data/memory.db

# HTTP API Configuration
export MCP_HTTP_ENABLED=true
export MCP_HTTP_HOST=localhost
export MCP_HTTP_PORT=8000
export MCP_API_KEY=your-secure-key

# Performance Tuning
export MCP_MEMORY_MAX_WORKERS=4
export MCP_MEMORY_BATCH_SIZE=100

# Logging
export MCP_LOG_LEVEL=INFO
export MCP_LOG_FILE=./logs/memory.log
```

### Custom Configuration File

Create a `.env` file in the project root:

```bash
# .env file
MCP_MEMORY_STORAGE_BACKEND=sqlite_vec
MCP_HTTP_ENABLED=true
MCP_HTTP_PORT=8000
MCP_API_KEY=your-secure-api-key
MCP_MEMORY_MAX_WORKERS=4
```

## Troubleshooting Common Issues

### First-Time Setup Warnings

On first run, you might see warnings that are completely normal:

```
WARNING: Failed to load from cache: No snapshots directory
WARNING: Using TRANSFORMERS_CACHE is deprecated
```

These warnings disappear after the first successful run. The service downloads a ~25MB embedding model on first use.

### Python Version Issues

```bash
# If you have multiple Python versions
python3.11 -m venv venv
source venv/bin/activate
python --version  # Should show 3.11.x
```

### Permission Issues

```bash
# On Linux/macOS, ensure proper permissions
chmod +x venv/bin/python
chmod +x venv/bin/uv
```

### Port Conflicts

```bash
# Check if port 8000 is available
lsof -i :8000

# Change port if needed
export MCP_HTTP_PORT=8001
```

### Memory Issues

```bash
# For systems with limited RAM
export MCP_MEMORY_MAX_WORKERS=2
export MCP_MEMORY_BATCH_SIZE=50
```

## Usage Examples

### Basic Memory Operations

```bash
# Store a memory with tags
uv run memory store "Fixed authentication bug by adding proper validation" --tags python,security,bugfix

# Search by natural language
uv run memory recall "authentication validation issues"

# Search by tags
uv run memory search --tags python security

# Search by time
uv run memory recall "yesterday afternoon"

# List all memories
uv run memory list --limit 10
```

### Advanced Queries

```bash
# Complex search with multiple criteria
uv run memory recall "database optimization" --tags performance --since "2024-01-01"

# Export memories
uv run memory export --format json --output memories_backup.json

# Import memories
uv run memory import --file memories_backup.json
```

## Performance Optimization

### For Local Development

```bash
# Use SQLite-vec for fastest local performance
export MCP_MEMORY_STORAGE_BACKEND=sqlite_vec

# Optimize worker count for your CPU
export MCP_MEMORY_MAX_WORKERS=$(nproc)
```

### For Production Use

```bash
# Enable HTTP API for web dashboard
export MCP_HTTP_ENABLED=true

# Set up proper logging
export MCP_LOG_LEVEL=WARNING
export MCP_LOG_FILE=/var/log/mcp-memory.log

# Configure backup
export MCP_BACKUP_ENABLED=true
export MCP_BACKUP_INTERVAL=3600  # 1 hour
```

## Security Considerations

### API Key Protection

```bash
# Generate a secure API key
openssl rand -hex 32

# Set the key
export MCP_API_KEY="your-generated-secure-key"
```

### Network Security

```bash
# Bind to localhost only
export MCP_HTTP_HOST=127.0.0.1

# Use HTTPS in production
export MCP_SSL_CERT=/path/to/cert.pem
export MCP_SSL_KEY=/path/to/key.pem
```

### Data Encryption

```bash
# Enable database encryption
export MCP_ENCRYPTION_ENABLED=true
export MCP_ENCRYPTION_KEY="your-encryption-key"
```

## Maintenance and Updates

### Regular Maintenance

```bash
# Check service health
uv run memory health

# Clean up old memories (optional)
uv run memory cleanup --older-than 90days

# Optimize database
uv run memory optimize
```

### Updating the Service

```bash
# Pull latest changes
git pull origin main

# Update dependencies
uv pip install -e . --upgrade

# Restart the service
# If running as service, restart it
# If running manually, stop and restart
```

### Backup and Restore

```bash
# Create backup
uv run memory backup --output backup_$(date +%Y%m%d).tar.gz

# Restore from backup
uv run memory restore --input backup_20240908.tar.gz
```

## Integration Examples

### With Claude Code

```bash
# Claude Code integration
export MCP_CLAUDE_CODE_ENABLED=true
export MCP_CLAUDE_CODE_HOOKS_DIR=./claude-hooks
```

### With Development Workflows

```bash
# Git integration
uv run memory store "Merged feature branch with conflict resolution" --tags git,merge,conflict

# Code review notes
uv run memory store "Code review: Add input validation to API endpoints" --tags review,api,security
```

## Monitoring and Analytics

### Service Metrics

```bash
# View service statistics
uv run memory stats

# Monitor memory usage
uv run memory usage

# Check query performance
uv run memory performance
```

### Logging Configuration

```bash
# Detailed logging
export MCP_LOG_LEVEL=DEBUG
export MCP_LOG_FORMAT=json

# Log rotation
export MCP_LOG_MAX_SIZE=100MB
export MCP_LOG_BACKUP_COUNT=5
```

## Conclusion

Installing MCP Memory Service with Python venv provides a clean, isolated environment for running this powerful AI memory service locally. The virtual environment approach ensures that dependencies are managed separately from your system Python, preventing conflicts and making maintenance easier.

### Key Benefits of This Installation Method

- **Isolation**: Dependencies don't conflict with system Python
- **Reproducibility**: Same environment across different machines
- **Easy Updates**: Simple to upgrade or modify
- **Clean Removal**: Easy to uninstall if needed
- **Development Friendly**: Great for testing and development

### Next Steps

1. **Explore the Web Dashboard**: Access `http://localhost:8000` for the web interface
2. **Integrate with Your AI Tools**: Connect Claude Desktop, VS Code, or other supported applications
3. **Customize Configuration**: Adjust settings for your specific use case
4. **Set Up Backups**: Configure regular backups for important memories
5. **Monitor Performance**: Use the built-in monitoring tools to optimize usage

The MCP Memory Service transforms how AI assistants work by providing persistent, searchable memory across conversations and sessions. With semantic search capabilities and multi-client support, it significantly enhances productivity for developers, researchers, and anyone working with AI tools.

## Further Reading

### Official Documentation
- **[MCP Memory Service Wiki](https://github.com/doobidoo/mcp-memory-service/wiki)** - Complete documentation and guides
- **[Installation Guide](https://github.com/doobidoo/mcp-memory-service/wiki/01-Installation-Guide)** - Detailed installation instructions
- **[Configuration Guide](https://github.com/doobidoo/mcp-memory-service/wiki/04-Advanced-Configuration)** - Advanced setup options

### Related Resources
- **[Model Context Protocol](https://modelcontextprotocol.io/)** - Learn about MCP protocol
- **[Claude Desktop](https://claude.ai/desktop)** - Official Claude Desktop application
- **[uv Package Manager](https://docs.astral.sh/uv/)** - Modern Python package management

### Community and Support
- **[GitHub Issues](https://github.com/doobidoo/mcp-memory-service/issues)** - Bug reports and feature requests
- **[GitHub Discussions](https://github.com/doobidoo/mcp-memory-service/discussions)** - Community discussions and help
- **[Troubleshooting Guide](https://github.com/doobidoo/mcp-memory-service/wiki/07-TROUBLESHOOTING)** - Solutions for common issues

For the latest updates and community contributions, follow the [MCP Memory Service repository](https://github.com/doobidoo/mcp-memory-service) and join the growing community of users leveraging AI memory capabilities.