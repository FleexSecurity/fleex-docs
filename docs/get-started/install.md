## Installation

Fleex is written in Go, so you can easily install it with the following command:

```
go install github.com/FleexSecurity/fleex@latest
```

Don't forget to set the `GOPATH` environment variable if you want to run fleex from any folder. 

```
fleex --help
```

```

███████╗██╗     ███████╗███████╗██╗  ██╗
██╔════╝██║     ██╔════╝██╔════╝╚██╗██╔╝
█████╗  ██║     █████╗  █████╗   ╚███╔╝ 
██╔══╝  ██║     ██╔══╝  ██╔══╝   ██╔██╗ 
██║     ███████╗███████╗███████╗██╔╝ ██╗
╚═╝     ╚══════╝╚══════╝╚══════╝╚═╝  ╚═╝

Distributed computing using Linode boxes.
Check out our docs at https://fleexsecurity.github.io/fleex-docs/

Usage:
  fleex [command]

Available Commands:
  delete      Delete an existing fleet or even a single box
  help        Help about any command
  images      Show image options
  init        Fleex initialization command. Run this the first time.
  ls          List running boxes
  run         Send a command to a fleet
  scan        Send a command to a fleet, but also with files upload & chunks splitting
  scp         Send a file/folder to a fleet using SCP
  spawn       Spawn a fleet or even a single box
  ssh         Start SSH terminal for a box

Flags:
      --config string     config file
  -h, --help              help for fleex
  -l, --loglevel string   Set log level. Available: debug, info, warn, error, fatal (default "info")
      --proxy string      HTTP Proxy (Useful for debugging. Example: http://127.0.0.1:8080)
  -t, --toggle            Help message for toggle

Use "fleex [command] --help" for more information about a command.
```