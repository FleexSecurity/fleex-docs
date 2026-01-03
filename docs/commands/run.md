# run

Execute a command simultaneously across all instances in a fleet.

## Usage

```
fleex run [flags]
```

## Flags

| Flag | Short | Description | Default |
|------|-------|-------------|---------|
| `--name` | `-n` | Fleet name | `pwn` |
| `--command` | `-c` | Command to execute (required) | |
| `--provider` | `-P` | Cloud provider | config default |
| `--username` | `-U` | SSH username | config default |
| `--port` | `-p` | SSH port | config default |

## Basic Usage

Execute a command on all instances in a fleet:

```bash
fleex run -n myfleet -c "whoami"
```

Output (for a 3-instance fleet):

```
root
root
root
```

## Examples

### System Information

Check disk usage on all instances:

```bash
fleex run -n myfleet -c "df -h /"
```

### Update Packages

Update apt packages on all instances:

```bash
fleex run -n myfleet -c "apt-get update && apt-get upgrade -y"
```

### Install Tool

Install a tool on all instances:

```bash
fleex run -n myfleet -c "go install github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest"
```

### Check Running Processes

List running processes:

```bash
fleex run -n myfleet -c "ps aux | grep nuclei"
```

### Download File

Download a file to all instances:

```bash
fleex run -n myfleet -c "wget -O /tmp/resolvers.txt https://raw.githubusercontent.com/janmasarik/resolvers/master/resolvers.txt"
```

### Check Tool Versions

Verify tool installation:

```bash
fleex run -n myfleet -c "nuclei --version"
```

### Custom SSH Settings

Use custom SSH username and port:

```bash
fleex run -n myfleet -c "ls -la" -U ubuntu -p 2222
```

### Multiple Commands

Chain multiple commands:

```bash
fleex run -n myfleet -c "cd /tmp && ls -la && pwd"
```

## Use Cases

### Pre-Scan Setup

Prepare instances before running scans:

```bash
fleex run -n myfleet -c "nuclei -update-templates"
```

### Post-Scan Cleanup

Clean up temporary files:

```bash
fleex run -n myfleet -c "rm -rf /tmp/fleex-*"
```

### Monitor Resource Usage

Check memory and CPU:

```bash
fleex run -n myfleet -c "free -h && uptime"
```

### Verify Connectivity

Test network connectivity:

```bash
fleex run -n myfleet -c "curl -s https://api.ipify.org"
```

## Comparison: run vs scan

| Feature | `fleex run` | `fleex scan` |
|---------|-------------|--------------|
| Input splitting | No | Yes |
| Output aggregation | No | Yes |
| File transfer | No | Yes |
| Same command on all | Yes | Yes |
| Different data per instance | No | Yes |

Use `run` for:

- Quick commands across all instances
- System administration tasks
- Tool installation/updates
- Checking instance status

Use `scan` for:

- Distributed scanning with input splitting
- When you need aggregated output
- Multi-step workflows

## Notes

- Commands execute in parallel on all instances
- Output is displayed as it arrives from each instance
- Use `fleex ssh` to connect to a single instance interactively
- Requires instances to be running (check with `fleex status`)
