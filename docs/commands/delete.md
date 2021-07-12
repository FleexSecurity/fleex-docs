This command allows you to delete the vps you have generated on your provider.

Usage:

| Flag | Name         | Description                                        | Default |
| ---- | ------------ | -------------------------------------------------- | ------- |
| `-n` | `--name`     | Fleet name. Boxes will be named [name]-[number]    | pwn     |
| `-p` | `--provider` | Service provider (Supported: linode, digitalocean) |         |


Examples:
```
fleex delete
```
Or
```
fleex delete -p PROVIDER-NAME -n FLEET-NAME
```

!!! note
    You can delete the vps individually by entering the full name