This command allows you to generate custom VPS dynamically on a specific provider.


!!! note
    The data is taken from $HOME/fleex/config.yaml,
    
    you can also overwrite it via the flags

Usage:

| Flag | Name         | Description                                        | Default |
| ---- | ------------ | -------------------------------------------------- | ------- |
| `-c` | `--count`    | How many box to spawn                              | 2       |
| `-I` | `--image`    | Image                                              |         |
| `-n` | `--name`     | Fleet name. Boxes will be named [name]-[number]    | pwn     |
| `-p` | `--provider` | Service provider (Supported: linode, digitalocean) |         |
| `-R` | `--region`   | Region                                             |         |
| `-S` | `--size`     | Size                                               |         |
|      | `--skipwait` | Skip waiting until all boxes are running           | false   |


To spawn a new fleet (aka, many boxes with starting with fleetname-box_id) run:
```
fleex spawn -n FLEET_NAME -c FLEET_COUNT
```
Remember that for any fleex subcommand, you can use the `-h` flag to get help regarding its usage.

!!! info
    Fleex will automatically wait until all boxes are ready before exiting.