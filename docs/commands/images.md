# images

Manage cloud provider images (snapshots).

## Usage

```
fleex images [command]
```

## Subcommands

| Command | Description |
|---------|-------------|
| `ls` | List available images |
| `rm` | Remove an image |

## images ls

List all images on your cloud provider:

```bash
fleex images ls
```

Output:

```
ID         NAME                              STATUS     SIZE (GB)
12345678   fleex-security-tools-03-01-2026   available  10.52
23456789   fleex-recon-tools-02-15-2026      available  8.34
34567890   ubuntu-22-04-backup               available  5.86
```

### Flags

| Flag | Short | Description | Default |
|------|-------|-------------|---------|
| `--provider` | `-p` | Cloud provider | config default |

### Filter by Provider

```bash
fleex images ls -p digitalocean
```

## images rm

Remove an image:

```bash
fleex images rm -n fleex-security-tools-03-01-2026
```

### Flags

| Flag | Short | Description | Default |
|------|-------|-------------|---------|
| `--name` | `-n` | Image name to remove | |
| `--provider` | `-p` | Cloud provider | config default |

## Creating Images

Images are created using the `build` command with `--snapshot`:

```bash
fleex spawn -n build -c 1
fleex build run -r security-tools -n build --snapshot
fleex delete -n build
```

Or during spawn with build:

```bash
fleex spawn -n build -c 1 --build security-tools
fleex build run -r security-tools -n build --snapshot
```

The snapshot is named: `fleex-{recipe-name}-{date-time}`

## Using Images

After creating an image, update your configuration to use it:

1. List images to get the ID:
   ```bash
   fleex images ls
   ```

2. Update `~/.config/fleex/config.json`:
   ```json
   {
     "providers": {
       "linode": {
         "image": "private/12345678"
       }
     }
   }
   ```

3. Future spawns will use this image:
   ```bash
   fleex spawn -n myfleet -c 10
   ```

## Image Naming Convention

Fleex-created images follow the pattern:

```
fleex-{recipe-name}-{DD-MM-YYYY-HH-MM}
```

Examples:

- `fleex-security-tools-03-01-2026-14-30`
- `fleex-recon-tools-02-28-2026-09-15`

## Examples

### List All Images

```bash
fleex images ls
```

### Remove Old Image

```bash
fleex images rm -n fleex-security-tools-01-01-2026
```

### Image Workflow

Complete workflow for creating and using custom images:

```bash
# 1. Spawn single instance
fleex spawn -n build -c 1

# 2. Build with tools
fleex build run -r security-tools -n build --snapshot

# 3. Get image ID
fleex images ls
# Note the ID: private/12345678

# 4. Update config.json with new image ID

# 5. Delete build instance
fleex delete -n build

# 6. Spawn fleet with pre-built image
fleex spawn -n myfleet -c 10
```

## Notes

- Image IDs vary by provider format
- Linode: `private/12345678`
- DigitalOcean: `12345678`
- Vultr: `uuid-format`
- Images incur storage costs when not attached to instances
- Delete unused images to reduce costs
- Creating images takes several minutes depending on disk size
