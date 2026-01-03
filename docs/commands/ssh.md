# ssh

Open an interactive SSH terminal to a specific instance.

## Usage

```
fleex ssh [flags]
```

## Flags

| Flag | Short | Description | Default |
|------|-------|-------------|---------|
| `--name` | `-n` | Instance name | `pwn` |
| `--provider` | `-p` | Cloud provider | config default |
| `--username` | `-U` | SSH username | config default |
| `--port` | | SSH port | config default |

## Basic Usage

Connect to an instance:

```bash
fleex ssh -n myfleet-1
```

Output:

```
Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 5.15.0-91-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

Last login: Fri Jan  3 14:30:00 2026 from 1.2.3.4
root@myfleet-1:~#
```

## Examples

### Connect to Specific Instance

```bash
fleex ssh -n scan-3
```

### Custom SSH Settings

Use custom username and port:

```bash
fleex ssh -n myfleet-1 -U ubuntu --port 2222
```

### Connect to First Instance

Connect to first instance in default fleet:

```bash
fleex ssh -n pwn-1
```

### Different Provider

Connect to instance on specific provider:

```bash
fleex ssh -n myfleet-1 -p digitalocean
```

## Use Cases

### Debug Scan Issues

SSH into an instance to debug:

```bash
fleex ssh -n scan-1
# Check logs, verify tool installation, etc.
```

### Manual Tool Testing

Test commands before running distributed scan:

```bash
fleex ssh -n test-1
# Run nuclei manually to verify it works
```

### Check Resources

Monitor instance resources:

```bash
fleex ssh -n myfleet-1
# htop, df -h, free -m, etc.
```

### File Inspection

Inspect scan output files:

```bash
fleex ssh -n scan-1
# cat /tmp/fleex-*/output.txt
```

## Notes

- Uses SSH keys configured in `~/.config/fleex/config.json`
- Connection details are retrieved from the provider API
- Instance must be in running state
- Use `fleex ls` to find instance names
- For running commands without interactive session, use `fleex run`
