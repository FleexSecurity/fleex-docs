# Releases

### Fleex v3.0.0

**Major Release - New Features**

#### New Commands

* **`build`** - Provision fleet instances with tools using YAML recipes
  - `build list` - List available build recipes
  - `build show` - Show recipe details
  - `build run` - Run build on a fleet
  - `build verify` - Verify installation
  - `build create` - Create new recipes
  - Supports `--snapshot` flag to create images after build

* **`estimate`** - Cost estimation before running scans
  - Calculates estimated duration and cost
  - Supports multiple tools (nuclei, httpx, masscan, etc.)
  - Shows cost level indicator (LOW, MODERATE, HIGH)

* **`status`** - Detailed fleet status with grouping
  - Groups instances by fleet
  - Shows running/total counts
  - Estimated hourly cost display
  - Summary statistics

#### Enhanced Commands

* **`init`** - Interactive setup wizard
  - `--wizard` flag for guided setup
  - Use case selection (Bug Bounty, Pentesting, Research)
  - Multi-provider configuration
  - `--add-provider` to add providers to existing config
  - Auto-generates SSH keys
  - Creates default workflows and build recipes

* **`spawn`** - Build integration
  - `--build` flag to provision after spawn
  - `--no-verify` to skip verification

* **`scan`** - Workflow support
  - `--workflow` flag for multi-step pipelines
  - `scan list` - List available workflows
  - `scan show` - Show workflow details
  - `--dry-run` to preview execution
  - `--verbose` for detailed output

#### New Systems

* **Build Recipes** - YAML-based provisioning scripts
  - Default recipes: `security-tools`, `recon-tools`, `base-setup`
  - Variable substitution with `{vars.KEY}`
  - Step retries and timeouts
  - Verification checks
  - Stored in `~/.config/fleex/builds/`

* **Workflows** - Multi-step scan pipelines
  - Default workflows: `quick-scan`, `full-recon`, `subdomain-enum`, `port-scan`, `meg-scan`, `ffuf-fuzz`
  - Setup commands
  - Output aggregation options
  - Stored in `~/.config/fleex/workflows/`

#### Provider Support

* Linode (fully supported)
* DigitalOcean (fully supported)
* Vultr (fully supported)
* Custom VMs (manual configuration)

---

### Fleex v2.1.0

**What's Changed**

* Init command ssh keypair by @xm1k3 in https://github.com/FleexSecurity/fleex/pull/45
* Space on init command ssh pub key creation by @xm1k3 in https://github.com/FleexSecurity/fleex/pull/48
* Better scp command by @xm1k3 in https://github.com/FleexSecurity/fleex/pull/49
* run command by @xm1k3 in https://github.com/FleexSecurity/fleex/pull/50
* Fixes on linode cloud provider by @xm1k3 in https://github.com/FleexSecurity/fleex/pull/51
* Scan command by @xm1k3 in https://github.com/FleexSecurity/fleex/pull/55


**Full Changelog**: [https://github.com/FleexSecurity/fleex/compare/v2.0...v2.1.0](https://github.com/FleexSecurity/fleex/compare/v2.0...v2.1.0)

---

### Fleex v2.0

**What's Changed**

* Update scp.go by @pradeepch99 in https://github.com/FleexSecurity/fleex/pull/16
* Add vultr by @pradeepch99 in https://github.com/FleexSecurity/fleex/pull/17
* vultr integration by @xm1k3 in https://github.com/FleexSecurity/fleex/pull/18
* Code refactor providers by @xm1k3 in https://github.com/FleexSecurity/fleex/pull/23
* replaced go get with go install by @remonsec in https://github.com/FleexSecurity/fleex/pull/26
* Update README.md by @b1bek in https://github.com/FleexSecurity/fleex/pull/31
* imp: packer implementation by @xm1k3 in https://github.com/FleexSecurity/fleex/pull/32
* remove image function by @xm1k3 in https://github.com/FleexSecurity/fleex/pull/33
* code refactoring linode by @xm1k3 in https://github.com/FleexSecurity/fleex/pull/35
* remove viper refs and replace with json config by @xm1k3 in https://github.com/FleexSecurity/fleex/pull/36
* New Features related to custom-vps && Fixes by @xm1k3 in https://github.com/FleexSecurity/fleex/pull/38
* added release ci by @xm1k3 in https://github.com/FleexSecurity/fleex/pull/41
* ssh logic also with custom vps by @xm1k3 in https://github.com/FleexSecurity/fleex/pull/43

New Contributors

* @pradeepch99 made their first contribution in https://github.com/FleexSecurity/fleex/pull/16
* @remonsec made their first contribution in https://github.com/FleexSecurity/fleex/pull/26
* @b1bek made their first contribution in https://github.com/FleexSecurity/fleex/pull/31

**Full Changelog**: [https://github.com/FleexSecurity/fleex/compare/v1.1...v2.0](https://github.com/FleexSecurity/fleex/compare/v1.1...v2.0)
