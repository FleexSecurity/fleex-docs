# build

Provision fleet instances with tools and configurations using build recipes. Create reusable provisioning scripts and snapshots for faster deployments.

## Usage

```
fleex build [command]
```

## Subcommands

| Command | Description |
|---------|-------------|
| `list` | List available build recipes |
| `show [name]` | Show recipe details |
| `run` | Run build on a fleet |
| `verify` | Verify build installation |
| `create [name]` | Create a new build recipe |

## build list

List all available build recipes:

```bash
fleex build list
```

Output:

```
=== AVAILABLE BUILD RECIPES ===

NAME                      DESCRIPTION
-------------------------------------------------------------------------
security-tools            Common security tools for bug bounty
recon-tools               Reconnaissance and enumeration tools
base-setup                Base system setup with essential packages
```

## build show

Display detailed information about a recipe:

```bash
fleex build show security-tools
```

Output:

```
=== SECURITY-TOOLS ===

Description: Common security tools for bug bounty
Author:      fleex
Version:     1.0.0
OS:          ubuntu, debian

Variables:
  GO_VERSION: 1.21.5
  USERNAME: root

Steps:
  1. System Update
     $ DEBIAN_FRONTEND=noninteractive apt-get update -qq
     $ DEBIAN_FRONTEND=noninteractive apt-get -o Dpkg::Options::=...
  2. Install Base Packages
     $ DEBIAN_FRONTEND=noninteractive apt-get install -y -qq git ...
  3. Install Go
     $ wget -q https://go.dev/dl/go{vars.GO_VERSION}.linux-amd64....
     $ rm -rf /usr/local/go && tar -C /usr/local -xzf /tmp/go.tar...
  4. Install ProjectDiscovery Tools
     $ /usr/local/go/bin/go install -v github.com/projectdiscove...
     $ /usr/local/go/bin/go install -v github.com/projectdiscove...
  5. Install Masscan
     $ git clone --depth 1 https://github.com/robertdavidgraham/m...

Verification:
  - Check nuclei: /root/go/bin/nuclei --version
  - Check httpx: /root/go/bin/httpx --version
  - Check masscan: masscan --version
```

## build run

Execute a build recipe on an existing fleet:

```bash
fleex build run -r security-tools -n myfleet
```

### Flags

| Flag | Short | Description | Default |
|------|-------|-------------|---------|
| `--recipe` | `-r` | Build recipe name | |
| `--file` | `-f` | Custom recipe file path | |
| `--name` | `-n` | Fleet name to build | |
| `--snapshot` | `-s` | Create snapshot after successful build | `false` |
| `--parallel` | `-p` | Number of parallel builds | `5` |
| `--no-verify` | | Skip verification step | `false` |
| `--continue` | | Continue on step failure | `false` |
| `--dry-run` | | Show what would be executed | `false` |
| `--verbose` | `-v` | Show detailed output | `false` |

### Examples

Build an existing fleet:

```bash
fleex build run -r security-tools -n myfleet
```

Build with snapshot creation:

```bash
fleex build run -r security-tools -n myfleet --snapshot
```

Output:

```
Building fleet 'myfleet' (5 instances) with recipe 'security-tools'...
[myfleet-1] Step 1/5: System Update
[myfleet-2] Step 1/5: System Update
[myfleet-3] Step 1/5: System Update
[myfleet-1] Step 2/5: Install Base Packages
[...]
Build complete: 5/5 successful
Creating snapshot 'fleex-security-tools-03-01-2026-14-30' from myfleet-1...
Snapshot 'fleex-security-tools-03-01-2026-14-30' created successfully
```

Build with custom recipe file:

```bash
fleex build run -f /path/to/custom-recipe.yaml -n myfleet
```

Dry run to preview:

```bash
fleex build run -r security-tools -n myfleet --dry-run
```

## build verify

Verify that tools are correctly installed on a fleet:

```bash
fleex build verify -r security-tools -n myfleet
```

### Flags

| Flag | Short | Description |
|------|-------|-------------|
| `--recipe` | `-r` | Build recipe name |
| `--name` | `-n` | Fleet name to verify |
| `--verbose` | `-v` | Show detailed output |

Output:

```
Verifying fleet 'myfleet' with recipe 'security-tools'...
[myfleet-1] Check nuclei: PASSED
[myfleet-1] Check httpx: PASSED
[myfleet-1] Check masscan: PASSED
[myfleet-2] Check nuclei: PASSED
[...]
Verification complete: 5/5 passed
```

## build create

Create a new build recipe:

```bash
fleex build create my-custom-tools
```

### Flags

| Flag | Description |
|------|-------------|
| `--description` | Recipe description |
| `--from` | Copy from existing recipe |

### Examples

Create blank recipe:

```bash
fleex build create my-tools --description "My custom toolset"
```

Copy from existing recipe:

```bash
fleex build create my-recon --from security-tools
```

Output:

```
Build recipe 'my-tools' created at: /home/user/.config/fleex/builds/my-tools.yaml
```

## Build Recipe Schema

```yaml
name: recipe-name              # Required: recipe identifier
description: Description       # Required: what this recipe installs
author: author-name            # Optional: recipe author
version: "1.0.0"               # Optional: recipe version

os:                            # Optional: OS requirements
  supported:
    - ubuntu
    - debian

vars:                          # Optional: variables for templating
  GO_VERSION: "1.21.5"
  USERNAME: root

files:                         # Optional: files to transfer
  - source: /local/path
    destination: /remote/path
    mode: "0755"

steps:                         # Required: installation steps
  - name: Step Name            # Required: step identifier
    commands:                  # Required: commands to execute
      - "command1"
      - "command2 {vars.GO_VERSION}"
    retries: 2                 # Optional: retry count on failure
    timeout: 600               # Optional: timeout in seconds
    continue_on: error         # Optional: continue on error

verify:                        # Optional: verification checks
  - name: Check tool
    command: "tool --version"
    expect: "tool"             # Optional: expected output substring
```

## Default Recipes

### security-tools

Installs common security tools:

- Go programming language
- Nuclei (vulnerability scanner)
- httpx (HTTP probe)
- Subfinder (subdomain discovery)
- Masscan (port scanner)

### recon-tools

Installs reconnaissance tools:

- Amass (attack surface mapping)
- ffuf (fuzzer)
- puredns (DNS resolver)
- massdns (DNS bruteforcer)

### base-setup

Basic system setup:

- System updates
- Essential packages (git, curl, wget, jq, htop, tmux, vim)
- 2GB swap file

## Custom Recipe Example

Create a custom recipe for your workflow:

```yaml
# ~/.config/fleex/builds/full-arsenal.yaml
name: full-arsenal
description: Complete bug bounty toolkit
author: user
version: "1.0.0"

os:
  supported:
    - ubuntu
    - debian

vars:
  GO_VERSION: "1.21.5"
  USERNAME: root

steps:
  - name: System Update
    commands:
      - DEBIAN_FRONTEND=noninteractive apt-get update -qq
      - DEBIAN_FRONTEND=noninteractive apt-get upgrade -y -qq
    retries: 2
    timeout: 600

  - name: Install Dependencies
    commands:
      - apt-get install -y -qq git curl wget jq make gcc libpcap-dev chromium-browser
    retries: 2

  - name: Install Go
    commands:
      - wget -q https://go.dev/dl/go{vars.GO_VERSION}.linux-amd64.tar.gz -O /tmp/go.tar.gz
      - rm -rf /usr/local/go && tar -C /usr/local -xzf /tmp/go.tar.gz
      - echo 'export PATH=$PATH:/usr/local/go/bin:/root/go/bin' >> ~/.bashrc

  - name: Install ProjectDiscovery Suite
    commands:
      - /usr/local/go/bin/go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
      - /usr/local/go/bin/go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest
      - /usr/local/go/bin/go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
      - /usr/local/go/bin/go install -v github.com/projectdiscovery/katana/cmd/katana@latest
      - /usr/local/go/bin/go install -v github.com/projectdiscovery/dnsx/cmd/dnsx@latest
    retries: 1

  - name: Install Additional Tools
    commands:
      - /usr/local/go/bin/go install -v github.com/tomnomnom/waybackurls@latest
      - /usr/local/go/bin/go install -v github.com/ffuf/ffuf/v2@latest
      - /usr/local/go/bin/go install -v github.com/lc/gau/v2/cmd/gau@latest

  - name: Update Nuclei Templates
    commands:
      - /root/go/bin/nuclei -update-templates -silent
    continue_on: error

verify:
  - name: Check nuclei
    command: /root/go/bin/nuclei --version
  - name: Check httpx
    command: /root/go/bin/httpx --version
  - name: Check ffuf
    command: /root/go/bin/ffuf --version
```

## Workflow

Typical build workflow:

1. **Spawn single instance**: `fleex spawn -n build -c 1`
2. **Run build**: `fleex build run -r security-tools -n build`
3. **Verify installation**: `fleex build verify -r security-tools -n build`
4. **Create snapshot**: `fleex build run -r security-tools -n build --snapshot`
5. **Update config** to use the new image
6. **Delete build instance**: `fleex delete -n build`

Or combine spawn and build:

```bash
fleex spawn -n myfleet -c 10 --build security-tools
```

## Notes

- Build recipes are stored in `~/.config/fleex/builds/`
- Use `{vars.KEY}` syntax for variable substitution
- Steps with `continue_on: error` won't fail the build
- Verification runs after all steps complete
- Snapshots are created from the first instance in the fleet
