# status

Display detailed status of fleets and instances with grouping and cost tracking.

## Usage

```
fleex status [fleet-name] [flags]
```

## Flags

| Flag | Short | Description | Default |
|------|-------|-------------|---------|
| `--summary` | `-s` | Show summary only | `false` |
| `--provider` | `-p` | Cloud provider | config default |

## Basic Usage

Show all instances grouped by fleet:

```bash
fleex status
```

Output:

```
=== FLEET STATUS ===
Provider: linode

Fleet: nuclei (5/5 running)
  LABEL       STATUS   IP
  nuclei-1    RUNNING  192.168.1.1
  nuclei-2    RUNNING  192.168.1.2
  nuclei-3    RUNNING  192.168.1.3
  nuclei-4    RUNNING  192.168.1.4
  nuclei-5    RUNNING  192.168.1.5

Fleet: scan (3/3 running)
  LABEL     STATUS   IP
  scan-1    RUNNING  192.168.2.1
  scan-2    RUNNING  192.168.2.2
  scan-3    RUNNING  192.168.2.3

=== SUMMARY ===
Total Fleets:    2
Total Instances: 8
Running:         8
Other:           0

Estimated hourly cost: $0.0600
```

## Filter by Fleet

Show status for a specific fleet:

```bash
fleex status nuclei
```

Output:

```
=== FLEET STATUS ===
Provider: linode

Fleet: nuclei (5/5 running)
  LABEL       STATUS   IP
  nuclei-1    RUNNING  192.168.1.1
  nuclei-2    RUNNING  192.168.1.2
  nuclei-3    RUNNING  192.168.1.3
  nuclei-4    RUNNING  192.168.1.4
  nuclei-5    RUNNING  192.168.1.5

=== SUMMARY ===
Total Fleets:    1
Total Instances: 5
Running:         5
Other:           0

Estimated hourly cost: $0.0375
```

## Summary Only

Show only the summary without instance details:

```bash
fleex status --summary
```

Output:

```
=== SUMMARY ===
Total Fleets:    2
Total Instances: 8
Running:         8
Other:           0

Estimated hourly cost: $0.0600
```

## Instance States

| Status | Description |
|--------|-------------|
| `RUNNING` | Instance is active and ready |
| `PROVISIONING` | Instance is being created |
| `BOOTING` | Instance is starting up |
| `SHUTTING_DOWN` | Instance is being stopped |
| `OFFLINE` | Instance is stopped |

## Examples

### Check Fleet Health

Before running a scan, verify all instances are ready:

```bash
fleex status myfleet
```

### Monitor Costs

Check current hourly cost for running instances:

```bash
fleex status --summary
```

### Different Provider

Check status on a specific provider:

```bash
fleex status -p digitalocean
```

## Workflow

Typical usage in a workflow:

1. **Spawn fleet**: `fleex spawn -n scan -c 10`
2. **Check status**: `fleex status scan` (wait for all RUNNING)
3. **Run scan**: `fleex scan -n scan -i targets.txt -o results.txt`
4. **Check status**: `fleex status scan` (verify completion)
5. **Delete fleet**: `fleex delete -n scan`

## Notes

- Cost estimates are based on running instances only
- Fleet names are extracted from instance labels (e.g., `scan-1` belongs to fleet `scan`)
- Use `fleex ls` for a simpler list view
- Use `fleex status {fleet}` to filter by specific fleet
