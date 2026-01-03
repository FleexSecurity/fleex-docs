## Installation

Fleex is written in Go. Install it with:

```bash
go install github.com/FleexSecurity/fleex@latest
```

Make sure `$GOPATH/bin` is in your PATH environment variable.

Verify the installation:

```bash
fleex --help
```

Output:

```
███████╗██╗     ███████╗███████╗██╗  ██╗
██╔════╝██║     ██╔════╝██╔════╝╚██╗██╔╝
█████╗  ██║     █████╗  █████╗   ╚███╔╝
██╔══╝  ██║     ██╔══╝  ██╔══╝   ██╔██╗
██║     ███████╗███████╗███████╗██╔╝ ██╗
╚═╝     ╚══════╝╚══════╝╚══════╝╚═╝  ╚═╝

Distributed computing using cloud VPS providers.
https://fleexsecurity.github.io/fleex-docs/

Usage:
  fleex [command]

Available Commands:
  build       Build and provision fleet instances with tools
  delete      Delete an existing fleet or even a single box
  estimate    Estimate scan cost before running
  help        Help about any command
  images      Show image options
  init        Fleex initialization wizard
  ls          List running boxes
  run         Send a command to a fleet
  scan        Send a command to a fleet, but also with files upload & chunks splitting
  scp         Send a file/folder to a fleet using SCP
  spawn       Spawn a fleet or even a single box
  ssh         Start SSH terminal for a box
  status      Show detailed fleet status

Flags:
      --config string     config file
  -h, --help              help for fleex
  -l, --loglevel string   Set log level. Available: debug, info, warn, error, fatal (default "info")
      --proxy string      HTTP Proxy (Useful for debugging. Example: http://127.0.0.1:8080)

Use "fleex [command] --help" for more information about a command.
```

## Quick Start

After installation, run the initialization wizard:

```bash
fleex init --wizard
```

This will:

1. Prompt you to select your use case (Bug Bounty, Penetration Testing, etc.)
2. Configure your cloud provider(s) with API tokens
3. Generate SSH key pairs automatically
4. Create default build recipes and workflows

For a faster setup with defaults:

```bash
fleex init
```

See the [init command](../commands/init.md) for detailed options.
