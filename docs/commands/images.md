This command allows you to see all the images you have generated from a provider 

Usage:

| Flag | Name         | Description                                        | Default |
| ---- | ------------ | -------------------------------------------------- | ------- |
| `-p` | `--provider` | Service provider (Supported: linode, digitalocean) |         |

Examples:
```
fleex images
```
Or
```
fleex images -p PROVIDER-NAME
```

Result:
```
<ID>     <NAME>                 <STATUS>  <SIZE (GB)>
12345678 Fleex-build-1624990254 available 10.52
23456789 Fleex-build-1624994814 available 5.42
34567890 Fleex-build-1625839588 available 5.86

```

!!! warning
    The displayed data is for example only