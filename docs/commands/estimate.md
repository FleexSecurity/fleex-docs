# estimate

Calculate estimated cost and duration for distributed scans before running them.

## Usage

```
fleex estimate [flags]
```

## Flags

| Flag | Short | Description | Default |
|------|-------|-------------|---------|
| `--targets` | `-t` | File with targets to scan | |
| `--instances` | `-i` | Number of instances to use | `10` |
| `--tool` | | Tool to use for time estimation | `nuclei` |
| `--provider` | `-p` | Cloud provider | config default |
| `--duration` | `-d` | Override estimated duration (hours) | auto |

## Basic Usage

Estimate cost for scanning a target list:

```bash
fleex estimate -t targets.txt -i 10
```

Output:

```
=== COST ESTIMATE ===

Targets:     5000
Instances:   10
Tool:        nuclei
Provider:    linode
Instance:    g6-nanode-1 @ $0.00750/hour

Duration:    ~25.0 minutes
Rate:        ~200 targets/min

Base cost:   $0.31
With buffer: $0.35 (+12%)

Cost level: LOW

To proceed, run:
  fleex spawn -n scan -c 10 && fleex scan -n scan -i targets.txt -o results.txt
```

## Tool-Specific Estimation

Different tools have different processing speeds:

```bash
fleex estimate -t targets.txt -i 20 --tool masscan
```

### Supported Tools

| Tool | Estimated Speed |
|------|----------------|
| `nuclei` | ~120 targets/hour/instance |
| `httpx` | ~300 targets/hour/instance |
| `subfinder` | ~200 targets/hour/instance |
| `masscan` | ~600 targets/hour/instance |
| `nmap` | ~60 targets/hour/instance |
| `ffuf` | ~75 targets/hour/instance |
| `puredns` | ~150 targets/hour/instance |
| `amass` | ~30 targets/hour/instance |

## Provider Comparison

Compare costs across providers:

```bash
fleex estimate -t targets.txt -i 10 -p linode
fleex estimate -t targets.txt -i 10 -p digitalocean
fleex estimate -t targets.txt -i 10 -p vultr
```

## Examples

### Large-Scale Nuclei Scan

Estimate cost for scanning 100,000 URLs:

```bash
fleex estimate -t 100k-urls.txt -i 50 --tool nuclei
```

Output:

```
=== COST ESTIMATE ===

Targets:     100000
Instances:   50
Tool:        nuclei
Provider:    linode
Instance:    g6-nanode-1 @ $0.00750/hour

Duration:    ~16.7 minutes
Rate:        ~6000 targets/min

Base cost:   $0.10
With buffer: $0.11 (+12%)

Cost level: LOW
```

### Port Scanning Campaign

Estimate cost for scanning IP ranges:

```bash
fleex estimate -t ip-ranges.txt -i 100 --tool masscan
```

### Custom Duration Override

If you know how long your scan takes:

```bash
fleex estimate -t targets.txt -i 20 -d 2.5
```

This overrides the automatic estimation with 2.5 hours.

## Cost Levels

The estimate provides a cost level indicator:

| Level | Cost Range |
|-------|-----------|
| LOW | < $1 |
| MODERATE | $1 - $10 |
| HIGH | $10 - $50 |
| VERY HIGH | > $50 |

## Instance Pricing

### Linode

| Size | RAM | Price/hour |
|------|-----|------------|
| `g6-nanode-1` | 1GB | $0.0075 |
| `g6-standard-1` | 2GB | $0.018 |
| `g6-standard-2` | 4GB | $0.036 |

### DigitalOcean

| Size | RAM | Price/hour |
|------|-----|------------|
| `s-1vcpu-1gb` | 1GB | $0.00744 |
| `s-1vcpu-2gb` | 2GB | $0.01488 |
| `s-2vcpu-2gb` | 2GB | $0.02679 |

### Vultr

| Size | RAM | Price/hour |
|------|-----|------------|
| `vc2-1c-1gb` | 1GB | $0.006 |
| `vc2-1c-2gb` | 2GB | $0.012 |
| `vc2-2c-4gb` | 4GB | $0.024 |

## Workflow

Typical workflow:

1. **Estimate**: `fleex estimate -t targets.txt -i 10`
2. **Adjust instances** if needed
3. **Spawn**: `fleex spawn -n scan -c 10 --build security-tools`
4. **Run scan**: `fleex scan -n scan -i targets.txt -o results.txt`
5. **Delete**: `fleex delete -n scan`

## Notes

- Estimates include a 12% buffer for overhead (SSH, file transfer, etc.)
- Actual costs may vary based on scan complexity and network conditions
- Empty lines and comments in target files are not counted
- Use `--duration` to override if you have historical data
