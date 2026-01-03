# scan

Distribute and execute scans across a fleet. Automatically splits input files, distributes chunks to instances, executes commands, and aggregates results.

## Usage

```
fleex scan [flags]
fleex scan [command]
```

## Subcommands

| Command | Description |
|---------|-------------|
| `list` | List available workflows |
| `show [name]` | Show workflow details |

## Flags

| Flag | Short | Description | Default |
|------|-------|-------------|---------|
| `--name` | `-n` | Fleet name | `pwn` |
| `--command` | `-c` | Command to execute (supports `{INPUT}` and `{OUTPUT}` placeholders) | |
| `--input` | `-i` | Input file to split and distribute | |
| `--output` | `-o` | Output file path (aggregated results) | |
| `--workflow` | `-w` | Workflow name (multi-step mode) | |
| `--workflow-file` | | Custom workflow file path | |
| `--module` | | YAML module file path | |
| `--params` | | Parameters in KEY:VALUE format | |
| `--chunks-folder` | | Output folder for chunks (default: /tmp/{timestamp}) | |
| `--delete` | `-d` | Delete instances after completion | `false` |
| `--dry-run` | | Show what would be executed | `false` |
| `--verbose` | `-v` | Show detailed output | `false` |
| `--provider` | `-p` | Cloud provider | config default |
| `--username` | `-U` | SSH username | config default |
| `--password` | `-P` | SSH password | |
| `--port` | | SSH port | config default |

## Single Command Mode

Execute a single command across the fleet:

```bash
fleex scan -n myfleet -c "nuclei -l {INPUT} -o {OUTPUT}" -i targets.txt -o results.txt
```

How it works:

1. Splits `targets.txt` into chunks (one per instance)
2. Uploads each chunk to its assigned instance
3. Executes the command with `{INPUT}` and `{OUTPUT}` replaced
4. Downloads results from each instance
5. Concatenates all results into `results.txt`

## Workflow Mode

Execute multi-step pipelines using predefined workflows:

```bash
fleex scan -n myfleet --workflow full-recon -i targets.txt -o results.txt
```

Each instance:

1. Receives one chunk of the input
2. Executes ALL workflow steps in sequence
3. Returns the final output

### List Available Workflows

```bash
fleex scan list
```

Output:

```
=== AVAILABLE WORKFLOWS ===

NAME                      DESCRIPTION                                        STEPS
-------------------------------------------------------------------------
quick-scan                Quick vulnerability scan with nuclei               1
full-recon                Complete recon pipeline (httpx -> nuclei)          2
subdomain-enum            Subdomain enumeration and probing                  2
port-scan                 Fast port scanning with masscan                    1
meg-scan                  Fast endpoint discovery with meg                   1
ffuf-fuzz                 Directory fuzzing with ffuf                        1
```

### Show Workflow Details

```bash
fleex scan show full-recon
```

Output:

```
=== FULL-RECON ===

Description: Complete recon pipeline (httpx -> nuclei)
Author:      fleex

Setup (runs on all boxes first):
  $ nuclei -update-templates -silent

Steps (run sequentially on each chunk):
  1. httpx
     $ httpx -l {INPUT} -silent -o {OUTPUT}
  2. nuclei
     $ nuclei -l {INPUT} -severity medium,high,critical -o {OUTPUT}
     timeout: 2h

Output:
  aggregate: concat
  deduplicate: true
```

## Module Mode

Use YAML modules for reusable scan configurations:

```bash
fleex scan -n myfleet --module nuclei-scan.yaml -i targets.txt -o results.txt
```

Module file (`nuclei-scan.yaml`):

```yaml
name: nuclei-scan
author: user
description: Custom nuclei scan

vars:
  SEVERITY: high,critical
  TEMPLATES: /root/nuclei-templates/

commands:
  - nuclei -l {INPUT} -severity {vars.SEVERITY} -t {vars.TEMPLATES} -o {OUTPUT}
```

Override module variables:

```bash
fleex scan -n myfleet --module nuclei-scan.yaml \
  --params SEVERITY:low,medium,high,critical \
  --params TEMPLATES:/custom/templates/ \
  -i targets.txt -o results.txt
```

## Examples

### Nuclei Vulnerability Scan

Distributed nuclei scan across 10 instances:

```bash
fleex spawn -n nuclei -c 10 --build security-tools
fleex scan -n nuclei -c "nuclei -l {INPUT} -o {OUTPUT}" -i urls.txt -o vulns.txt
fleex delete -n nuclei
```

### Quick Scan Workflow

Use the built-in quick-scan workflow:

```bash
fleex scan -n myfleet --workflow quick-scan -i targets.txt -o results.txt
```

### Full Recon Pipeline

Multi-step recon with httpx and nuclei:

```bash
fleex scan -n myfleet --workflow full-recon -i domains.txt -o findings.txt
```

### Subdomain Enumeration

Distributed subdomain discovery:

```bash
fleex scan -n myfleet --workflow subdomain-enum -i domains.txt -o subdomains.txt
```

### Port Scanning

Fast distributed port scan with masscan:

```bash
fleex scan -n myfleet --workflow port-scan -i ips.txt -o ports.txt
```

### Directory Fuzzing

Distributed ffuf fuzzing:

```bash
fleex scan -n myfleet -c "ffuf -u FUZZ -w {INPUT} -of csv -o {OUTPUT}" \
  -i urls.txt -o ffuf-results.csv
```

### httpx Probing

Probe for live hosts:

```bash
fleex scan -n myfleet -c "httpx -l {INPUT} -silent -o {OUTPUT}" \
  -i domains.txt -o live-hosts.txt
```

### Auto-Delete After Scan

Delete instances automatically when scan completes:

```bash
fleex scan -n tempfleet -w quick-scan -i targets.txt -o results.txt --delete
```

### Dry Run

Preview what would be executed:

```bash
fleex scan -n myfleet -w full-recon -i targets.txt -o results.txt --dry-run
```

## Custom Workflows

Create custom workflows in `~/.config/fleex/workflows/`:

```yaml
# ~/.config/fleex/workflows/custom-recon.yaml
name: custom-recon
description: Custom reconnaissance workflow
author: user

vars:
  RESOLVERS: /tmp/resolvers.txt

setup:
  - "wget -q https://raw.githubusercontent.com/janmasarik/resolvers/master/resolvers.txt -O /tmp/resolvers.txt"

steps:
  - name: subfinder
    command: "subfinder -dL {INPUT} -silent -o {OUTPUT}"
    timeout: "30m"

  - name: puredns
    command: "puredns resolve {INPUT} -r {vars.RESOLVERS} -w {OUTPUT}"
    timeout: "1h"

  - name: httpx
    command: "httpx -l {INPUT} -silent -threads 50 -o {OUTPUT}"
    timeout: "30m"

output:
  aggregate: sort-unique
  deduplicate: true
```

Use it:

```bash
fleex scan -n myfleet --workflow custom-recon -i domains.txt -o live-hosts.txt
```

## Workflow Schema

```yaml
name: workflow-name          # Required: workflow identifier
description: Description     # Optional: what this workflow does
author: author-name          # Optional: workflow author

vars:                        # Optional: default variables
  KEY: value

setup:                       # Optional: commands run once on all instances
  - "command1"
  - "command2"

steps:                       # Required: pipeline steps
  - name: step-name          # Required: step identifier
    command: "cmd {INPUT}"   # Required: command with placeholders
    timeout: "1h"            # Optional: step timeout

output:                      # Optional: output processing
  aggregate: concat          # concat | sort-unique
  deduplicate: true          # Remove duplicate lines
```

## Placeholders

| Placeholder | Description |
|-------------|-------------|
| `{INPUT}` | Path to input chunk on remote instance |
| `{OUTPUT}` | Path to output file on remote instance |
| `{vars.KEY}` | Variable value from module/workflow |

## Notes

- Input file is automatically split into chunks based on fleet size
- Results are downloaded and aggregated into the specified output file
- Use `--verbose` to see detailed progress
- Use `--dry-run` to preview execution without running
- Workflows execute setup commands once, then run steps on each chunk
