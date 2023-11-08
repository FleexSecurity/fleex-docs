The `fleex init` command is your gateway to an effortless setup process. Use this command for the initial configuration, which automates the generation of SSH keys for seamless authentication with your machines. Additionally, it downloads a default JSON config file, ready for your customization with essential tokens and cloud provider details.

Usage:

```
Fleex initialization command. Run this the first time.

Usage:
  fleex init [flags]

Flags:
  -e, --email string   Email for the ssh key pair creation
  -h, --help           help for init
  -o, --overwrite      If the fleex folder exists overwrite it
  -u, --url string     Config folder url

Global Flags:
      --config string     config file
  -l, --loglevel string   Set log level. Available: debug, info, warn, error, fatal (default "info")
      --proxy string      HTTP Proxy (Useful for debugging. Example: http://127.0.0.1:8080)
```

!!! warning
    Fleex will initialize all configuration files in `$HOME/.config/fleex/`

Examples:
```
fleex init
```

Output:

```
INFO[0002] Fleex initialized successfully, see $HOME/.config/fleex 
```