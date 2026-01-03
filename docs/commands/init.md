# init

Initialize Fleex configuration with SSH keys, provider settings, default workflows, and build recipes.

## Usage

```
fleex init [flags]
```

## Flags

| Flag | Short | Description |
|------|-------|-------------|
| `--wizard` | `-w` | Run interactive setup wizard |
| `--overwrite` | `-o` | Overwrite existing configuration |
| `--add-provider` | | Add a new provider to existing config (linode, digitalocean, vultr) |
| `--email` | `-e` | Email for SSH key generation |

## Quick Setup

Basic initialization with prompts for provider and API token:

```bash
fleex init
```

Output:

```
=== FLEEX QUICK SETUP ===

For interactive wizard, use: fleex init --wizard

Enter your preferred provider (linode/digitalocean/vultr) [linode]:
Enter API token:

Setup complete!
Configuration: /home/user/.config/fleex/config.json
```

## Interactive Wizard

Full guided setup with use case selection and multiple providers:

```bash
fleex init --wizard
```

Output:

```
=== FLEEX SETUP WIZARD ===

Select your primary use case:
  1. Bug Bounty Hunting
  2. Penetration Testing
  3. Security Research
  4. Continuous Scanning

Choice [1-4]: 1

Which cloud providers do you have accounts with?
(Enter comma-separated numbers, e.g., 1,2)
  1. Linode
  2. DigitalOcean
  3. Vultr
  4. Custom VMs only

Choice: 1,2

--- Linode Configuration ---
API Token:
Region [us-east]:
Instance size [g6-nanode-1]:
SSH Port [22]:
SSH Username [root]:

--- DigitalOcean Configuration ---
API Token:
Region [nyc1]:
Instance size [s-1vcpu-1gb]:
SSH Port [22]:
SSH Username [root]:

--- SSH Key Generation ---
Generating SSH key pair with email: user@hostname

=== SETUP COMPLETE ===
Configuration saved to: /home/user/.config/fleex/config.json
SSH keys generated in: /home/user/.config/fleex/ssh

Quick start commands:
  fleex ls                           # List running instances
  fleex spawn -n myfleet -c 5        # Spawn 5 instances
  fleex spawn -n myfleet -c 5 --build security-tools  # Spawn and provision
  fleex build list                   # View available build recipes
  fleex workflow list                # View available workflows
```

## Add Provider

Add a new provider to existing configuration without overwriting:

```bash
fleex init --add-provider digitalocean
```

Output:

```
--- Adding digitalocean Provider ---
API Token:
Region [nyc1]:
Instance size [s-1vcpu-1gb]:
SSH Port [22]:
SSH Username [root]:

Provider 'digitalocean' added successfully
```

## Overwrite Configuration

Replace existing configuration:

```bash
fleex init --overwrite
```

!!! warning
    This will delete existing SSH keys and configuration. Back up any custom settings first.

## Custom VMs Setup

When selecting "Custom VMs only" in the wizard:

```
--- Custom VMs Configuration ---
Enter custom VMs (empty IP to finish):

VM #1
  Public IP: 192.168.1.100
  SSH Port [22]: 22
  Username [root]: ubuntu
  Instance ID (e.g., vm-1): my-server

VM #2
  Public IP:
```

## What Gets Created

After initialization, Fleex creates:

```
~/.config/fleex/
├── config.json        # Main configuration
├── ssh/
│   ├── id_rsa         # Private SSH key
│   └── id_rsa.pub     # Public SSH key
├── builds/            # Build recipes
│   ├── security-tools.yaml
│   ├── recon-tools.yaml
│   └── base-setup.yaml
└── workflows/         # Scan workflows
    ├── quick-scan.yaml
    ├── full-recon.yaml
    ├── subdomain-enum.yaml
    ├── port-scan.yaml
    ├── meg-scan.yaml
    └── ffuf-fuzz.yaml
```

## Default Build Recipes

| Recipe | Description |
|--------|-------------|
| `security-tools` | Nuclei, httpx, subfinder, masscan |
| `recon-tools` | Amass, ffuf, puredns, massdns |
| `base-setup` | System updates, essential packages, swap |

## Default Workflows

| Workflow | Description |
|----------|-------------|
| `quick-scan` | Nuclei high/critical vulnerabilities |
| `full-recon` | httpx probe + nuclei scan pipeline |
| `subdomain-enum` | Subfinder + httpx enumeration |
| `port-scan` | Masscan full port scan |
| `meg-scan` | Fast endpoint discovery with meg |
| `ffuf-fuzz` | Directory fuzzing with ffuf |

## Next Steps

After initialization:

1. Verify configuration: `cat ~/.config/fleex/config.json`
2. Test provider connection: `fleex ls`
3. Spawn your first fleet: `fleex spawn -n test -c 2`
4. View available builds: `fleex build list`
5. View available workflows: `fleex scan list`
