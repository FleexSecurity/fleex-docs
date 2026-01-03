![Fleex](img/Fleex_banner.png){ align=left }

## Welcome to Fleex

Fleex is a distributed computing tool designed for penetration testing and bug bounty hunting. It enables you to execute commands across multiple virtual private servers (VPS) with ease and efficiency.

### Key Features

- **Multi-Cloud Support**: Seamlessly work with Linode, DigitalOcean, Vultr, and custom VMs
- **Build System**: Provision fleets with pre-configured tools using YAML recipes
- **Workflow Pipelines**: Execute multi-step scan workflows across distributed instances
- **Cost Estimation**: Calculate scan costs before running
- **Automatic Scaling**: Split input files and aggregate results automatically

### Quick Start

```bash
# Install
go install github.com/FleexSecurity/fleex@latest

# Initialize
fleex init --wizard

# Spawn fleet with tools
fleex spawn -n scan -c 10 --build security-tools

# Run distributed scan
fleex scan -n scan --workflow quick-scan -i targets.txt -o results.txt

# Clean up
fleex delete -n scan
```

### Commands Overview

| Command | Description |
|---------|-------------|
| `init` | Initialize configuration with wizard |
| `spawn` | Create VPS instances |
| `build` | Provision instances with tools |
| `scan` | Distributed scanning with workflows |
| `run` | Execute commands on fleet |
| `status` | Fleet status and cost tracking |
| `estimate` | Cost estimation |
| `delete` | Remove instances |
| `ls` | List instances |
| `ssh` | Connect to instance |
| `scp` | Transfer files |
| `images` | Manage snapshots |

### Supported Providers

| Provider | Status |
|----------|--------|
| Linode | Fully supported |
| DigitalOcean | Fully supported |
| Vultr | Fully supported |
| Custom VMs | Manual configuration |

### Example Workflows

**Vulnerability Scanning**

```bash
fleex spawn -n nuclei -c 20 --build security-tools
fleex scan -n nuclei --workflow quick-scan -i urls.txt -o vulnerabilities.txt
fleex delete -n nuclei
```

**Subdomain Enumeration**

```bash
fleex spawn -n recon -c 10 --build recon-tools
fleex scan -n recon --workflow subdomain-enum -i domains.txt -o subdomains.txt
fleex delete -n recon
```

**Port Scanning**

```bash
fleex spawn -n portscan -c 50
fleex scan -n portscan --workflow port-scan -i ips.txt -o ports.txt
fleex delete -n portscan
```

### Documentation

- [Installation](get-started/install.md)
- [Configuration](get-started/config.md)
- [Commands Reference](commands/init.md)
- [Changelog](releases.md)
