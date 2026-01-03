# scp

Transfer files and folders to fleet instances using SCP.

## Usage

```
fleex scp [flags]
```

## Flags

| Flag | Short | Description | Default |
|------|-------|-------------|---------|
| `--name` | `-n` | Fleet name or instance name | `pwn` |
| `--source` | `-s` | Source file/folder (required) | |
| `--destination` | `-d` | Destination path (required) | |
| `--provider` | `-p` | Cloud provider | config default |
| `--username` | `-U` | SSH username | config default |
| `--port` | | SSH port | config default |

## Transfer to Fleet

Send file to all instances in a fleet:

```bash
fleex scp -n myfleet -s ~/wordlist.txt -d /tmp/
```

Output:

```
INFO SCP completed, you can find your files in /tmp/wordlist.txt
```

## Transfer to Single Instance

Send file to a specific instance:

```bash
fleex scp -n myfleet-1 -s ~/targets.txt -d /tmp/
```

## Examples

### Upload Wordlist

```bash
fleex scp -n myfleet -s ~/SecLists/Discovery/Web-Content/common.txt -d /tmp/wordlist.txt
```

### Upload Configuration

```bash
fleex scp -n myfleet -s ~/.config/nuclei/config.yaml -d /root/.config/nuclei/
```

### Upload Folder

Transfer entire directory:

```bash
fleex scp -n myfleet -s ~/templates/ -d /root/custom-templates/
```

### Upload Targets

Prepare targets for distributed scan:

```bash
fleex scp -n myfleet -s ~/targets.txt -d /tmp/targets.txt
```

### Upload Script

Deploy custom script:

```bash
fleex scp -n myfleet -s ~/scan-script.sh -d /tmp/
fleex run -n myfleet -c "chmod +x /tmp/scan-script.sh"
```

### Custom SSH Settings

```bash
fleex scp -n myfleet -s ~/file.txt -d /tmp/ -U ubuntu --port 2222
```

## Common Destinations

| Path | Purpose |
|------|---------|
| `/tmp/` | Temporary files |
| `/root/` | Root home directory |
| `/opt/` | Optional software |
| `/root/.config/` | Tool configurations |

## Path Handling

Home directory paths are automatically adjusted:

- Local `~/` is converted to appropriate remote path
- If local user is not root, paths are adjusted to `/home/{username}/`

## Use Cases

### Pre-Scan File Distribution

Upload resolvers for DNS enumeration:

```bash
fleex scp -n myfleet -s ~/resolvers.txt -d /tmp/resolvers.txt
fleex scan -n myfleet -c "puredns resolve {INPUT} -r /tmp/resolvers.txt -w {OUTPUT}" -i domains.txt -o resolved.txt
```

### Deploy Custom Templates

Upload nuclei templates:

```bash
fleex scp -n myfleet -s ~/custom-nuclei-templates/ -d /root/nuclei-templates/custom/
```

### Upload Configuration Files

Deploy tool configurations:

```bash
fleex scp -n myfleet -s ~/.config/subfinder/provider-config.yaml -d /root/.config/subfinder/
```

## Comparison: scp vs scan

| Feature | `fleex scp` | `fleex scan` |
|---------|-------------|--------------|
| Uploads same file to all | Yes | No (splits input) |
| Downloads results | No | Yes |
| Auto file splitting | No | Yes |
| Manual control | Yes | Automated |

Use `scp` for:

- Uploading wordlists and configurations
- Deploying scripts and tools
- Pre-scan file distribution

Use `scan` for:

- Distributed scanning with automatic input splitting
- Automated result aggregation

## Notes

- Both files and folders are supported
- Transfers happen sequentially to each instance
- Use `fleex run` to execute commands after file transfer
- For downloading files from instances, use manual SSH/SCP
