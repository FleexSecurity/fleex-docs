# spawn

Create VPS instances on your configured cloud provider. Instances are named with the pattern `{fleet-name}-{number}`.

## Usage

```
fleex spawn [flags]
```

## Flags

| Flag | Short | Description | Default |
|------|-------|-------------|---------|
| `--name` | `-n` | Fleet name | `pwn` |
| `--count` | `-c` | Number of instances to spawn | `2` |
| `--provider` | `-p` | Cloud provider (linode, digitalocean, vultr) | config default |
| `--region` | `-R` | Data center region | config default |
| `--size` | `-S` | Instance size/type | config default |
| `--image` | `-I` | OS image | config default |
| `--build` | `-b` | Build recipe to run after spawn | |
| `--no-verify` | | Skip build verification | `false` |
| `--skipwait` | | Skip waiting for instances to be ready | `false` |

## Basic Usage

Spawn a fleet of 5 instances:

```bash
fleex spawn -n myfleet -c 5
```

Output:

```
Spawning 5 instances for fleet 'myfleet'...
myfleet-1 spawning...
myfleet-2 spawning...
myfleet-3 spawning...
myfleet-4 spawning...
myfleet-5 spawning...
Waiting for instances to be ready...
myfleet-1 ready (192.168.1.1)
myfleet-2 ready (192.168.1.2)
myfleet-3 ready (192.168.1.3)
myfleet-4 ready (192.168.1.4)
myfleet-5 ready (192.168.1.5)
```

## Spawn with Build Recipe

Spawn instances and automatically provision them with tools:

```bash
fleex spawn -n scanfleet -c 10 --build security-tools
```

This spawns 10 instances and runs the `security-tools` build recipe on each, installing nuclei, httpx, subfinder, and masscan.

Output:

```
Spawning 10 instances for fleet 'scanfleet'...
[...]
Building fleet 'scanfleet' with recipe 'security-tools'...
[scanfleet-1] Step 1/5: System Update
[scanfleet-2] Step 1/5: System Update
[...]
Build complete: 10/10 successful
```

## Provider Override

Use a different provider than configured default:

```bash
fleex spawn -n dofleet -c 3 -p digitalocean
```

## Custom Instance Configuration

Override region, size, and image:

```bash
fleex spawn -n bigfleet -c 5 \
  -p linode \
  -R us-west \
  -S g6-standard-2 \
  -I linode/ubuntu22.04
```

## Skip Wait

Spawn instances without waiting for them to be ready:

```bash
fleex spawn -n fastfleet -c 20 --skipwait
```

Use this for large fleets when you'll run commands later.

## Fleet Naming

Instances follow the pattern `{name}-{number}`:

| Fleet Name | Count | Instance Names |
|------------|-------|----------------|
| `pwn` | 3 | pwn-1, pwn-2, pwn-3 |
| `scan` | 5 | scan-1, scan-2, scan-3, scan-4, scan-5 |
| `nuclei-fleet` | 2 | nuclei-fleet-1, nuclei-fleet-2 |

## Examples

### Bug Bounty Fleet

Spawn 10 instances with security tools for bug bounty:

```bash
fleex spawn -n bugbounty -c 10 --build security-tools
```

### Port Scanning Fleet

Spawn 20 small instances for distributed port scanning:

```bash
fleex spawn -n portscan -c 20 -S g6-nanode-1
```

### Temporary Test Fleet

Spawn 2 instances for testing:

```bash
fleex spawn -n test -c 2
```

### Multi-Region Fleet

Spawn instances in different regions (run separately):

```bash
fleex spawn -n us-fleet -c 5 -R us-east
fleex spawn -n eu-fleet -c 5 -R eu-west
```

## Instance Sizes

### Linode

| Size | RAM | vCPUs | Price/hour |
|------|-----|-------|------------|
| `g6-nanode-1` | 1GB | 1 | $0.0075 |
| `g6-standard-1` | 2GB | 1 | $0.018 |
| `g6-standard-2` | 4GB | 2 | $0.036 |

### DigitalOcean

| Size | RAM | vCPUs | Price/hour |
|------|-----|-------|------------|
| `s-1vcpu-1gb` | 1GB | 1 | $0.00744 |
| `s-1vcpu-2gb` | 2GB | 1 | $0.01488 |
| `s-2vcpu-2gb` | 2GB | 2 | $0.02679 |

### Vultr

| Size | RAM | vCPUs | Price/hour |
|------|-----|-------|------------|
| `vc2-1c-1gb` | 1GB | 1 | $0.006 |
| `vc2-1c-2gb` | 2GB | 1 | $0.012 |
| `vc2-2c-4gb` | 4GB | 2 | $0.024 |

## Workflow

Typical workflow:

1. **Estimate costs**: `fleex estimate -t targets.txt -i 10`
2. **Spawn fleet**: `fleex spawn -n scan -c 10 --build security-tools`
3. **Verify fleet**: `fleex status scan`
4. **Run scan**: `fleex scan -n scan -w quick-scan -i targets.txt -o results.txt`
5. **Delete fleet**: `fleex delete -n scan`

## Notes

- Fleex waits for all instances to be ready before completing (unless `--skipwait` is used)
- SSH keys from configuration are automatically added to instances
- Use `fleex ls` to list running instances
- Use `fleex status` for detailed fleet information
- Use `fleex delete -n {name}` to remove a fleet
