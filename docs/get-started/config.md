# Configuration

Fleex stores all configuration in `~/.config/fleex/`. Run `fleex init` to generate the initial configuration.

## Directory Structure

```
~/.config/fleex/
├── config.json       # Main configuration file
├── ssh/
│   ├── id_rsa        # Private SSH key (auto-generated)
│   └── id_rsa.pub    # Public SSH key (auto-generated)
├── builds/           # Build recipes for provisioning
│   ├── security-tools.yaml
│   ├── recon-tools.yaml
│   └── base-setup.yaml
└── workflows/        # Scan workflows
    ├── quick-scan.yaml
    ├── full-recon.yaml
    ├── subdomain-enum.yaml
    ├── port-scan.yaml
    ├── meg-scan.yaml
    └── ffuf-fuzz.yaml
```

## config.json

The main configuration file defines providers, SSH keys, and settings.

```json
{
  "settings": {
    "provider": "linode"
  },
  "ssh_keys": {
    "public_file": "/home/user/.config/fleex/ssh/id_rsa.pub",
    "private_file": "/home/user/.config/fleex/ssh/id_rsa"
  },
  "providers": {
    "linode": {
      "token": "YOUR_LINODE_API_TOKEN",
      "region": "us-east",
      "size": "g6-nanode-1",
      "image": "linode/ubuntu22.04",
      "port": 22,
      "username": "root"
    },
    "digitalocean": {
      "token": "YOUR_DO_API_TOKEN",
      "region": "nyc1",
      "size": "s-1vcpu-1gb",
      "image": "ubuntu-22-04-x64",
      "port": 22,
      "username": "root"
    },
    "vultr": {
      "token": "YOUR_VULTR_API_TOKEN",
      "region": "ewr",
      "size": "vc2-1c-1gb",
      "image": "387",
      "port": 22,
      "username": "root"
    }
  },
  "custom_vms": [
    {
      "provider": "custom",
      "instance_id": "my-server-1",
      "public_ip": "192.168.1.100",
      "ssh_port": 22,
      "username": "root",
      "key_path": "/path/to/private-key.pem",
      "tags": ["production"]
    }
  ]
}
```

## Provider Configuration

Each provider requires:

| Field | Description |
|-------|-------------|
| `token` | API token from your cloud provider |
| `region` | Data center region |
| `size` | Instance type/size |
| `image` | OS image ID or name |
| `port` | SSH port (default: 22) |
| `username` | SSH username (default: root) |
| `password` | SSH password (optional, key-based auth preferred) |

### Provider Defaults

**Linode:**

- Region: `us-east`
- Size: `g6-nanode-1` (1GB RAM, $5/month)
- Image: `linode/ubuntu22.04`

**DigitalOcean:**

- Region: `nyc1`
- Size: `s-1vcpu-1gb` (1GB RAM, $6/month)
- Image: `ubuntu-22-04-x64`

**Vultr:**

- Region: `ewr`
- Size: `vc2-1c-1gb` (1GB RAM, $5/month)
- Image: `387` (Ubuntu 22.04)

## Custom VMs

Use custom VMs to integrate existing servers or VMs from unsupported providers:

```json
"custom_vms": [
  {
    "provider": "custom",
    "instance_id": "aws-server-1",
    "public_ip": "54.123.45.67",
    "ssh_port": 22,
    "username": "ubuntu",
    "key_path": "/home/user/.ssh/aws-key.pem",
    "tags": ["aws", "production"]
  }
]
```

Custom VMs can be used with all Fleex commands by setting the provider to `custom`.

## Adding Providers

Add a new provider to existing configuration:

```bash
fleex init --add-provider digitalocean
```

This prompts for API token and settings without overwriting existing configuration.

## Using Custom Images

For faster deployments, use pre-configured images instead of installing tools on each spawn:

1. Spawn a single instance:
   ```bash
   fleex spawn -n build -c 1
   ```

2. Build with a recipe:
   ```bash
   fleex build run -r security-tools -n build --snapshot
   ```

3. Use the new image in future spawns by updating `config.json`:
   ```json
   "image": "private/12345678"
   ```

List available images:

```bash
fleex images ls
```

## Environment Variables

Override configuration with flags:

```bash
fleex spawn -n myfleet -c 5 \
  -p linode \
  -R us-west \
  -S g6-standard-1 \
  -I linode/ubuntu22.04
```
