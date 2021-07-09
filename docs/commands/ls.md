This command allows you to see all the VPS you have generated from a provider 

Usage:

| Flag | Name         | Description                                        | Default |
| ---- | ------------ | -------------------------------------------------- | ------- |
| `-p` | `--provider` | Service provider (Supported: linode, digitalocean) |         |

Examples:
```
fleex ls
```
Or
```
fleex ls -p PROVIDER-NAME
```

Result:
```
<ID>      <NAME> <STATUS> <IP>
123456789 pwn-1  active   192.168.1.1
123456789 pwn-2  active   192.168.1.2
123456789 pwn-4  active   192.168.1.3
```

!!! warning
    The displayed data is for example only