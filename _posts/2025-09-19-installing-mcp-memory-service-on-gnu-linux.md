---
layout: single
title: "Installing MCP Memory Service on GNU/Linux"
date: 2025-09-19
tags:
  - linux
  - mcp
  - memory
  - installation
  - tutorial
  - ubuntu
  - debian
categories:
  - linux
  - tools
excerpt: "A comprehensive guide to installing and configuring MCP Memory Service on GNU/Linux systems, including Ubuntu and Debian variants."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/08/robotic-ai.png
  overlay_image: /assets/2025/08/robotic-ai.png
---

**TL;DR**:

MCP Memory Service is a powerful memory management system that integrates with Claude Desktop and Claude Code. This tutorial covers the complete installation process specifically for GNU/Linux systems (Ubuntu 18.04+, Debian, and derivatives), including prerequisites, installation steps, configuration, and troubleshooting.

## Prerequisites

Before installing MCP Memory Service, ensure your system meets these requirements:

### System Requirements
- **Operating System**: Ubuntu 18.04 or newer, Debian 10+, or equivalent GNU/Linux distributions
- **Python**: Version 3.10 or newer (3.11+ recommended for optimal performance)
- **Memory**: Minimum 4GB RAM (8GB+ recommended for better performance)
- **Storage**: At least 2GB free disk space
- **Architecture**: x86_64 (AMD64) or ARM64

### Required Dependencies
The installer will handle most dependencies automatically, but you'll need basic system packages:

```bash
sudo apt update
sudo apt install python3.10 python3.10-venv python3-pip git curl
```

## Installation

### Method 1: Universal Installer (Recommended)

The fastest way to install MCP Memory Service on GNU/Linux:

```bash
# Clone the repository
git clone https://github.com/doobidoo/mcp-memory-service.git
cd mcp-memory-service

# Run the universal installer
python3 install.py
```

The installer will:
- Detect your GNU/Linux distribution
- Install optimal dependencies
- Configure SQLite-vec backend (recommended for Linux)
- Set up environment variables
- Generate a secure API key
- Optionally install as a system service

### Method 2: Manual Installation

If you prefer manual control:

```bash
# Install system dependencies
sudo apt update
sudo apt install python3.10 python3.10-venv python3-pip git build-essential

# Clone and setup
git clone https://github.com/doobidoo/mcp-memory-service.git
cd mcp-memory-service

# Create virtual environment (optional but recommended)
python3 -m venv venv
source venv/bin/activate

# Install Python dependencies
pip install -e .
```

## Advanced Installation Options

### GPU Acceleration (Optional)

For systems with NVIDIA GPUs:

```bash
python3 install.py --cuda
```

For AMD GPUs with ROCm support:

```bash
python3 install.py --rocm
```

### System Service Installation

To run MCP Memory Service as a background service that starts automatically:

```bash
# Install as system service
sudo python3 install.py --service

# Or install manually after regular installation
sudo python3 scripts/install_service.py
```

## Configuration

### Storage Backend Selection

MCP Memory Service supports multiple storage backends. For GNU/Linux, SQLite-vec is recommended:

```bash
# Set environment variable
export MCP_MEMORY_STORAGE_BACKEND=sqlite_vec
```

Other options:
- **ChromaDB**: For multi-client setups
- **Cloudflare**: For production deployments

### Essential Environment Variables

Create a `.env` file or export these variables:

```bash
# Storage configuration
export MCP_MEMORY_STORAGE_BACKEND=sqlite_vec

# HTTP API (optional)
export MCP_HTTP_ENABLED=true
export MCP_HTTP_PORT=8000

# Security
export MCP_API_KEY="$(openssl rand -base64 32)"

# Performance tuning
export MAX_RESULTS_PER_QUERY=10
export SIMILARITY_THRESHOLD=0.7
```

### Configuration Files

Configuration is stored in `~/.mcp_memory_service/`:
- **Service config**: `service_config.json`
- **Logs**: `logs/` directory
- **Data**: Platform-specific storage location

## Service Management

### Systemd Service Commands

```bash
# Start the service
systemctl --user start mcp-memory

# Stop the service
systemctl --user stop mcp-memory

# Check status
systemctl --user status mcp-memory

# Restart the service
systemctl --user restart mcp-memory

# Enable auto-start on boot
systemctl --user enable mcp-memory

# View logs
journalctl --user -u mcp-memory -f
```

### Manual Service Management

If not using systemd:

```bash
# Start manually
uv run memory server

# Or with HTTP API
uv run memory server --http
```

## Verification and Testing

### Basic Functionality Test

```bash
# Verify environment
python3 scripts/verify_environment.py

# Test memory operations
uv run memory store "Test memory for GNU/Linux installation"
uv run memory recall "test installation"
uv run memory health
```

### HTTP API Testing (if enabled)

```bash
# Health check
curl -k https://localhost:8000/api/health

# Store a memory via API
curl -k -X POST https://localhost:8000/api/memories \
  -H "Content-Type: application/json" \
  -d '{"content": "API test memory from GNU/Linux"}'

# Search memories
curl -k -X POST https://localhost:8000/api/search \
  -H "Content-Type: application/json" \
  -d '{"query": "GNU/Linux"}'
```

## Integration with Claude

### Claude Desktop Integration

After installation, configure Claude Desktop by editing `~/.claude/config.json`:

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

Replace `/path/to/mcp-memory-service` with your actual installation path.

### Claude Code Integration

For developers using Claude Code:

```bash
# Install with Claude Code support
python3 install.py --claude-code

# Install memory awareness hooks
python3 scripts/install_claude_hooks.py
```

## Troubleshooting

### Common Issues

**Permission Errors During Installation:**

```bash
# Use virtual environment
python3 -m venv venv
source venv/bin/activate
python3 install.py
```

**Port Already in Use:**

```bash
# Change the default port
export MCP_HTTP_PORT=8001
```

**Service Won't Start:**

```bash
# Check systemd logs
journalctl --user -u mcp-memory -n 50

# Verify configuration
python3 scripts/verify_environment.py
```

**Python Version Issues:**

```bash
# Check Python version
python3 --version

# Install specific version if needed
sudo apt install python3.11 python3.11-venv
```

**Dependency Installation Failures:**

```bash
# Update pip and reinstall
pip install --upgrade pip
pip install --force-reinstall -e .
```

### Performance Optimization

For better performance on GNU/Linux:

```bash
# Enable mDNS service discovery
export MCP_MDNS_ENABLED=true

# Adjust similarity threshold
export SIMILARITY_THRESHOLD=0.8

# Increase results per query
export MAX_RESULTS_PER_QUERY=20
```

## Next Steps

After successful installation:

1. **Test Integration**: Verify MCP Memory Service works with Claude Desktop
2. **Explore Features**: Try different storage backends and configurations
3. **Monitor Performance**: Use the health check endpoint regularly
4. **Backup Data**: Set up regular backups of your memory data
5. **Update Regularly**: Keep MCP Memory Service updated with `git pull && pip install -e .`

## Getting Help

- **Documentation**: [MCP Memory Service Wiki](https://github.com/doobidoo/mcp-memory-service/wiki)
- **Issues**: [GitHub Issues](https://github.com/doobidoo/mcp-memory-service/issues)
- **Discussions**: [GitHub Discussions](https://github.com/doobidoo/mcp-memory-service/discussions)

This tutorial focuses exclusively on GNU/Linux installations while covering all essential aspects from the official guide.