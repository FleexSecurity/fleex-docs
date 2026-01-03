# DigitalOcean

DigitalOcean is fully supported by Fleex.

## Getting Started

### 1. Create DigitalOcean Account

Sign up at [digitalocean.com](https://www.digitalocean.com/) if you don't have an account.

### 2. Generate API Token

1. Go to [API Tokens](https://cloud.digitalocean.com/account/api/tokens)
2. Click "Generate New Token"
3. Set token name (e.g., "fleex")
4. Select "Read" and "Write" scopes
5. Click "Generate Token"
6. Copy the token (shown only once)

### 3. Configure Fleex

Run the initialization wizard:

```bash
fleex init --wizard
```

Or add DigitalOcean to existing config:

```bash
fleex init --add-provider digitalocean
```

### 4. Manual Configuration

Edit `~/.config/fleex/config.json`:

```json
{
  "settings": {
    "provider": "digitalocean"
  },
  "providers": {
    "digitalocean": {
      "token": "YOUR_DIGITALOCEAN_API_TOKEN",
      "region": "nyc1",
      "size": "s-1vcpu-1gb",
      "image": "ubuntu-22-04-x64",
      "port": 22,
      "username": "root"
    }
  }
}
```

## Regions

| Region ID | Location |
|-----------|----------|
| `nyc1` | New York 1 |
| `nyc2` | New York 2 |
| `nyc3` | New York 3 |
| `sfo1` | San Francisco 1 |
| `sfo2` | San Francisco 2 |
| `sfo3` | San Francisco 3 |
| `ams2` | Amsterdam 2 |
| `ams3` | Amsterdam 3 |
| `sgp1` | Singapore 1 |
| `lon1` | London 1 |
| `fra1` | Frankfurt 1 |
| `tor1` | Toronto 1 |
| `blr1` | Bangalore 1 |
| `syd1` | Sydney 1 |

!!! info
    Full list: [DigitalOcean Regions](https://docs.digitalocean.com/products/platform/availability-matrix/)

## Droplet Sizes

| Size ID | Name | RAM | vCPUs | Price/hour | Price/month |
|---------|------|-----|-------|------------|-------------|
| `s-1vcpu-1gb` | Basic | 1GB | 1 | $0.00744 | $5 |
| `s-1vcpu-2gb` | Basic | 2GB | 1 | $0.01488 | $10 |
| `s-2vcpu-2gb` | Basic | 2GB | 2 | $0.02679 | $18 |
| `s-2vcpu-4gb` | Basic | 4GB | 2 | $0.02976 | $20 |
| `s-4vcpu-8gb` | Basic | 8GB | 4 | $0.05952 | $40 |
| `s-6vcpu-16gb` | Basic | 16GB | 6 | $0.11905 | $80 |
| `s-8vcpu-32gb` | Basic | 32GB | 8 | $0.2381 | $160 |

!!! info
    Full list: [DigitalOcean Droplet Plans](https://slugs.do-api.dev/)

## Images

### Default Images

| Image ID | Description |
|----------|-------------|
| `ubuntu-22-04-x64` | Ubuntu 22.04 LTS |
| `ubuntu-20-04-x64` | Ubuntu 20.04 LTS |
| `debian-11-x64` | Debian 11 |
| `debian-12-x64` | Debian 12 |

### Custom Images (Snapshots)

After building with `--snapshot`:

```bash
fleex build run -r security-tools -n build --snapshot
```

List your snapshots:

```bash
fleex images ls -p digitalocean
```

Use snapshot ID in config:

```json
"image": "12345678"
```

## Example Configuration

Recommended configuration for bug bounty:

```json
{
  "settings": {
    "provider": "digitalocean"
  },
  "providers": {
    "digitalocean": {
      "token": "YOUR_TOKEN",
      "region": "nyc1",
      "size": "s-1vcpu-1gb",
      "image": "ubuntu-22-04-x64",
      "port": 22,
      "username": "root"
    }
  }
}
```

## Cost Examples

| Fleet Size | Droplet | Hourly Cost | Daily Cost |
|------------|---------|-------------|------------|
| 10 | s-1vcpu-1gb | $0.074 | $1.79 |
| 20 | s-1vcpu-1gb | $0.149 | $3.57 |
| 50 | s-1vcpu-1gb | $0.372 | $8.93 |
| 10 | s-2vcpu-2gb | $0.268 | $6.43 |

Use `fleex estimate` to calculate costs:

```bash
fleex estimate -t targets.txt -i 10 -p digitalocean
```

## Workflow Example

Complete workflow using DigitalOcean:

```bash
# Initialize
fleex init --wizard

# Estimate cost
fleex estimate -t targets.txt -i 20 --tool nuclei -p digitalocean

# Spawn and build
fleex spawn -n scan -c 20 -p digitalocean --build security-tools

# Verify fleet
fleex status scan

# Run scan
fleex scan -n scan --workflow quick-scan -i targets.txt -o results.txt

# Clean up
fleex delete -n scan
```

## Troubleshooting

### Droplet Limit

Default limit is 10 droplets. To increase:

1. Go to [Account Settings](https://cloud.digitalocean.com/account)
2. Click "Request Increase"
3. Request higher droplet limit

### SSH Connection Issues

If SSH fails:

1. Check droplet status: `fleex status`
2. Verify SSH key was added
3. Try explicit port: `fleex ssh -n instance-1 --port 22`

### Snapshot Not Found

If custom snapshot fails:

1. List snapshots: `fleex images ls -p digitalocean`
2. Verify snapshot ID is numeric (not `private/`)
3. Check snapshot is in same region
