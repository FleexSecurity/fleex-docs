# Vultr

Vultr is fully supported by Fleex with competitive pricing.

## Getting Started

### 1. Create Vultr Account

Sign up at [vultr.com](https://www.vultr.com/) if you don't have an account.

### 2. Generate API Key

1. Go to [API Settings](https://my.vultr.com/settings/#settingsapi)
2. Click "Enable API"
3. Copy the API Key
4. Add your IP to the Access Control list (or allow all IPs)

### 3. Configure Fleex

Run the initialization wizard:

```bash
fleex init --wizard
```

Or add Vultr to existing config:

```bash
fleex init --add-provider vultr
```

### 4. Manual Configuration

Edit `~/.config/fleex/config.json`:

```json
{
  "settings": {
    "provider": "vultr"
  },
  "providers": {
    "vultr": {
      "token": "YOUR_VULTR_API_KEY",
      "region": "ewr",
      "size": "vc2-1c-1gb",
      "image": "387",
      "port": 22,
      "username": "root"
    }
  }
}
```

## Regions

| Region ID | Location |
|-----------|----------|
| `ewr` | New Jersey |
| `ord` | Chicago |
| `dfw` | Dallas |
| `sea` | Seattle |
| `lax` | Los Angeles |
| `atl` | Atlanta |
| `ams` | Amsterdam |
| `lhr` | London |
| `fra` | Frankfurt |
| `par` | Paris |
| `sgp` | Singapore |
| `nrt` | Tokyo |
| `icn` | Seoul |
| `syd` | Sydney |
| `mia` | Miami |

!!! info
    Full list: [Vultr Regions API](https://api.vultr.com/v2/regions)

## Instance Sizes

| Size ID | Name | RAM | vCPUs | Price/hour | Price/month |
|---------|------|-----|-------|------------|-------------|
| `vc2-1c-1gb` | Cloud Compute | 1GB | 1 | $0.006 | $4 |
| `vc2-1c-2gb` | Cloud Compute | 2GB | 1 | $0.012 | $8 |
| `vc2-2c-4gb` | Cloud Compute | 4GB | 2 | $0.024 | $16 |
| `vc2-4c-8gb` | Cloud Compute | 8GB | 4 | $0.048 | $32 |
| `vc2-6c-16gb` | Cloud Compute | 16GB | 6 | $0.095 | $64 |
| `vc2-8c-32gb` | Cloud Compute | 32GB | 8 | $0.190 | $128 |

!!! info
    Full list: [Vultr Plans API](https://api.vultr.com/v2/plans)

## Images

### Default Images (OS IDs)

| Image ID | Description |
|----------|-------------|
| `387` | Ubuntu 22.04 LTS |
| `1743` | Ubuntu 24.04 LTS |
| `477` | Debian 11 |
| `1946` | Debian 12 |

!!! note
    Vultr uses numeric OS IDs. Get the full list from [Vultr OS API](https://api.vultr.com/v2/os)

### Custom Images (Snapshots)

After building with `--snapshot`:

```bash
fleex build run -r security-tools -n build --snapshot
```

List your snapshots:

```bash
fleex images ls -p vultr
```

Use snapshot ID in config:

```json
"image": "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
```

## Example Configuration

Recommended configuration for bug bounty:

```json
{
  "settings": {
    "provider": "vultr"
  },
  "providers": {
    "vultr": {
      "token": "YOUR_TOKEN",
      "region": "ewr",
      "size": "vc2-1c-1gb",
      "image": "387",
      "port": 22,
      "username": "root"
    }
  }
}
```

## Cost Examples

Vultr offers the lowest prices among supported providers:

| Fleet Size | Instance | Hourly Cost | Daily Cost |
|------------|----------|-------------|------------|
| 10 | vc2-1c-1gb | $0.06 | $1.44 |
| 20 | vc2-1c-1gb | $0.12 | $2.88 |
| 50 | vc2-1c-1gb | $0.30 | $7.20 |
| 10 | vc2-2c-4gb | $0.24 | $5.76 |

Use `fleex estimate` to calculate costs:

```bash
fleex estimate -t targets.txt -i 10 -p vultr
```

## Workflow Example

Complete workflow using Vultr:

```bash
# Initialize
fleex init --wizard

# Estimate cost
fleex estimate -t targets.txt -i 20 --tool nuclei -p vultr

# Spawn and build
fleex spawn -n scan -c 20 -p vultr --build security-tools

# Verify fleet
fleex status scan

# Run scan
fleex scan -n scan --workflow quick-scan -i targets.txt -o results.txt

# Clean up
fleex delete -n scan
```

## API Access Control

Vultr requires IP whitelisting for API access:

1. Go to [API Settings](https://my.vultr.com/settings/#settingsapi)
2. Under "Access Control", add your IP address
3. Or select "Allow All IPv4" (less secure)

!!! warning
    If you get API authentication errors, check that your IP is whitelisted.

## Troubleshooting

### Instance Limit

Default limit varies by account age and billing history. To increase:

1. Open a support ticket at [Vultr Support](https://my.vultr.com/support/)
2. Request higher instance limit

### API Authentication Failed

If API calls fail:

1. Verify API key is correct
2. Check IP is in Access Control list
3. Ensure API is enabled in settings

### SSH Connection Issues

If SSH fails:

1. Check instance status: `fleex status`
2. Vultr instances may take longer to boot
3. Try explicit port: `fleex ssh -n instance-1 --port 22`

### Snapshot Not Found

If custom snapshot fails:

1. List snapshots: `fleex images ls -p vultr`
2. Verify snapshot ID is UUID format
3. Check snapshot status is "complete"

## Provider Comparison

| Feature | Vultr | Linode | DigitalOcean |
|---------|-------|--------|--------------|
| Min Price/hour | $0.006 | $0.0075 | $0.00744 |
| API Rate Limit | Generous | Moderate | Moderate |
| Snapshot Storage | Free (limited) | $0.06/GB | $0.05/GB |
| Regions | 25+ | 11 | 14 |
