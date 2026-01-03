# Linode

Linode is the default and recommended provider for Fleex.

## Getting Started

### 1. Create Linode Account

Sign up at [linode.com](https://www.linode.com/) if you don't have an account.

### 2. Generate API Token

1. Go to [Linode API Tokens](https://cloud.linode.com/profile/tokens)
2. Click "Create a Personal Access Token"
3. Set label (e.g., "fleex")
4. Select expiry or "Never"
5. Enable these scopes:
   - Linodes: Read/Write
   - Images: Read/Write
6. Click "Create Token"
7. Copy the token (shown only once)

### 3. Configure Fleex

Run the initialization wizard:

```bash
fleex init --wizard
```

Or add Linode to existing config:

```bash
fleex init --add-provider linode
```

### 4. Manual Configuration

Edit `~/.config/fleex/config.json`:

```json
{
  "settings": {
    "provider": "linode"
  },
  "providers": {
    "linode": {
      "token": "YOUR_LINODE_API_TOKEN",
      "region": "us-east",
      "size": "g6-nanode-1",
      "image": "linode/ubuntu22.04",
      "port": 22,
      "username": "root"
    }
  }
}
```

## Regions

| Region ID | Location |
|-----------|----------|
| `us-east` | Newark, NJ |
| `us-central` | Dallas, TX |
| `us-west` | Fremont, CA |
| `us-southeast` | Atlanta, GA |
| `ca-central` | Toronto, ON |
| `eu-west` | London, UK |
| `eu-central` | Frankfurt, DE |
| `ap-south` | Singapore, SG |
| `ap-northeast` | Tokyo, JP |
| `ap-west` | Mumbai, IN |
| `ap-southeast` | Sydney, AU |

!!! info
    Full list: [Linode Regions API](https://api.linode.com/v4/regions)

## Instance Sizes

| Size ID | Name | RAM | vCPUs | Price/hour | Price/month |
|---------|------|-----|-------|------------|-------------|
| `g6-nanode-1` | Nanode 1GB | 1GB | 1 | $0.0075 | $5 |
| `g6-standard-1` | Linode 2GB | 2GB | 1 | $0.018 | $12 |
| `g6-standard-2` | Linode 4GB | 4GB | 2 | $0.036 | $24 |
| `g6-standard-4` | Linode 8GB | 8GB | 4 | $0.072 | $48 |
| `g6-standard-6` | Linode 16GB | 16GB | 6 | $0.144 | $96 |
| `g6-standard-8` | Linode 32GB | 32GB | 8 | $0.288 | $192 |

!!! info
    Full list: [Linode Types API](https://api.linode.com/v4/linode/types)

## Images

### Default Images

| Image ID | Description |
|----------|-------------|
| `linode/ubuntu22.04` | Ubuntu 22.04 LTS |
| `linode/ubuntu20.04` | Ubuntu 20.04 LTS |
| `linode/debian11` | Debian 11 |
| `linode/debian12` | Debian 12 |

### Custom Images

After building with `--snapshot`:

```bash
fleex build run -r security-tools -n build --snapshot
```

List your images:

```bash
fleex images ls
```

Use custom image ID in config:

```json
"image": "private/12345678"
```

## Example Configuration

Recommended configuration for bug bounty:

```json
{
  "settings": {
    "provider": "linode"
  },
  "providers": {
    "linode": {
      "token": "YOUR_TOKEN",
      "region": "us-east",
      "size": "g6-nanode-1",
      "image": "linode/ubuntu22.04",
      "port": 22,
      "username": "root"
    }
  }
}
```

## Cost Examples

| Fleet Size | Instance | Hourly Cost | Daily Cost |
|------------|----------|-------------|------------|
| 10 | g6-nanode-1 | $0.075 | $1.80 |
| 20 | g6-nanode-1 | $0.15 | $3.60 |
| 50 | g6-nanode-1 | $0.375 | $9.00 |
| 10 | g6-standard-2 | $0.36 | $8.64 |

Use `fleex estimate` to calculate costs before spawning:

```bash
fleex estimate -t targets.txt -i 10 -p linode
```

## Workflow Example

Complete workflow using Linode:

```bash
# Initialize
fleex init --wizard

# Estimate cost
fleex estimate -t targets.txt -i 20 --tool nuclei -p linode

# Spawn and build
fleex spawn -n scan -c 20 --build security-tools

# Verify fleet
fleex status scan

# Run scan
fleex scan -n scan --workflow quick-scan -i targets.txt -o results.txt

# Clean up
fleex delete -n scan
```

## Troubleshooting

### API Rate Limits

Linode has API rate limits. For large fleets (50+), use `--skipwait`:

```bash
fleex spawn -n bigfleet -c 100 --skipwait
```

### SSH Connection Issues

If SSH fails:

1. Check instance status: `fleex status`
2. Verify SSH key in config
3. Try with explicit port: `fleex ssh -n instance-1 --port 22`

### Image Not Found

If custom image fails:

1. List images: `fleex images ls`
2. Verify image ID format: `private/12345678`
3. Check image is in same region
