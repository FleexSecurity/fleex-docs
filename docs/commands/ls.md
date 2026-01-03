# ls

List all running instances on your cloud provider.

## Usage

```
fleex ls [flags]
```

## Flags

| Flag | Short | Description | Default |
|------|-------|-------------|---------|
| `--provider` | `-p` | Cloud provider | config default |

## Basic Usage

List all instances:

```bash
fleex ls
```

Output:

```
ID         NAME       STATUS   IP
123456789  pwn-1      running  192.168.1.1
123456790  pwn-2      running  192.168.1.2
123456791  scan-1     running  192.168.2.1
123456792  scan-2     running  192.168.2.2
123456793  scan-3     running  192.168.2.3
```

## List by Provider

List instances on a specific provider:

```bash
fleex ls -p digitalocean
```

## Output Columns

| Column | Description |
|--------|-------------|
| ID | Provider's instance ID |
| NAME | Instance label/name |
| STATUS | Current state (running, provisioning, etc.) |
| IP | Public IP address |

## Instance States

| Status | Description |
|--------|-------------|
| `running` / `active` | Instance is ready |
| `provisioning` | Being created |
| `booting` | Starting up |
| `shutting_down` | Being stopped |
| `offline` | Stopped |

## Examples

### Quick Check

Verify fleet is running before scan:

```bash
fleex ls
```

### Count Instances

Count running instances:

```bash
fleex ls | wc -l
```

### Filter by Fleet

Use grep to filter:

```bash
fleex ls | grep scan
```

### Get IPs

Extract just IP addresses:

```bash
fleex ls | awk '{print $4}'
```

## Comparison: ls vs status

| Feature | `fleex ls` | `fleex status` |
|---------|------------|----------------|
| Output format | Simple table | Grouped by fleet |
| Fleet grouping | No | Yes |
| Cost estimate | No | Yes |
| Summary stats | No | Yes |

Use `ls` for:

- Quick overview of all instances
- Scripting and automation
- Simple instance lookup

Use `status` for:

- Detailed fleet information
- Cost monitoring
- Fleet health checks

## Notes

- Shows all instances, not filtered by fleet
- Use `fleex status {fleet}` for fleet-specific view
- Empty output means no instances are running
