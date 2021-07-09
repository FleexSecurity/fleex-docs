Fleex takes care of creating a new image if you need one.

It will install tools that are usually needed by infosec researchers/bug bounty hunters.

!!! Warning
    If you want to edit the tools that will be installed, manually edit the `~/fleex/build/common.yaml` file before proceeding.

Usage:

| Flag | Name         | Description                                        | Default                   |
| ---- | ------------ | -------------------------------------------------- | ------------------------- |
| `-d` | `--delete`   | Delete box after image creation                    |                           |
| `-f` | `--file`     | Build file                                         | ~/fleex/build/common.yaml |
| `-p` | `--provider` | Service provider (Supported: linode, digitalocean) |                           |
| `-R` | `--region`   | Region                                             |                           |
| `-S` | `--size`     | Size                                               |                           |


Examples:
```
fleex build
```

This will take some time, around 20 minutes on average.
The good news is that you will only have to do this once for every cloud provider you wish to use.

When it's done, run `fleex images`, get the image id and manually put it into the configuration file, then save it.