The fleex spawn command empowers you to dynamically create custom virtual private servers (VPS) on a specific provider. 

Ensure you have your configuration data set up in `$HOME/.config/fleex/config.json`. You also have the option to override this data using the available flags.


Usage:

```
Spawn a fleet or even a single box

Usage:
  fleex spawn [flags]

Flags:
  -c, --count int         How many box to spawn (default 2)
  -h, --help              help for spawn
  -I, --image string      Image
  -n, --name string       Fleet name. Boxes will be named [name]-[number] (default "pwn")
  -p, --provider string   Service provider
  -R, --region string     Region
  -S, --size string       Size
      --skipwait          Skip waiting until all boxes are running

Global Flags:
      --config string     config file
  -l, --loglevel string   Set log level. Available: debug, info, warn, error, fatal (default "info")
      --proxy string      HTTP Proxy (Useful for debugging. Example: http://127.0.0.1:8080
```

To spawn a new fleet (aka, many boxes with starting with fleetname-box_id) run:
```
fleex spawn -n FLEET_NAME -c FLEET_COUNT
```
Remember that for any fleex subcommand, you can use the `-h` flag to get help regarding its usage.

!!! info
    Fleex will automatically wait until all boxes are ready before exiting.