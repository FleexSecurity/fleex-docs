# delete

Remove fleet instances or individual boxes from your cloud provider.

## Usage

```
fleex delete [flags]
```

## Flags

| Flag | Short | Description | Default |
|------|-------|-------------|---------|
| `--name` | `-n` | Fleet name or instance name | `pwn` |
| `--provider` | `-p` | Cloud provider | config default |

## Delete Fleet

Delete all instances in a fleet:

```bash
fleex delete -n myfleet
```

This deletes all instances matching the pattern `myfleet-*` (e.g., myfleet-1, myfleet-2, myfleet-3).

## Delete Single Instance

Delete a specific instance by its full name:

```bash
fleex delete -n myfleet-2
```

## Examples

### Delete Default Fleet

Delete the default `pwn` fleet:

```bash
fleex delete
```

### Delete Named Fleet

Delete a named fleet:

```bash
fleex delete -n scan
```

### Delete on Specific Provider

Delete from a specific provider:

```bash
fleex delete -n myfleet -p digitalocean
```

### Delete After Scan

Typical workflow after completing a scan:

```bash
fleex scan -n tempfleet -c "nuclei -l {INPUT} -o {OUTPUT}" -i targets.txt -o results.txt
fleex delete -n tempfleet
```

Or use the `--delete` flag with scan:

```bash
fleex scan -n tempfleet -c "nuclei -l {INPUT} -o {OUTPUT}" -i targets.txt -o results.txt --delete
```

## Safety

Before deleting, verify which instances will be affected:

```bash
fleex ls
```

Or check specific fleet:

```bash
fleex status myfleet
```

## Notes

- Delete matches by prefix: `-n myfleet` deletes myfleet-1, myfleet-2, etc.
- Use exact name to delete a single instance: `-n myfleet-2`
- Deletion is permanent and cannot be undone
- Billing stops immediately after instance deletion
- Use `fleex ls` to verify instances were deleted
