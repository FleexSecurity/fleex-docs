This command allows you to see all the VPS you have generated from a provider 

Usage:

```
List running boxes

Usage:
  fleex ls [flags]

Flags:
  -h, --help              help for ls
  -p, --provider string   Service provider (Supported: linode)

Global Flags:
      --config string     config file
  -l, --loglevel string   Set log level. Available: debug, info, warn, error, fatal (default "info")
      --proxy string      HTTP Proxy (Useful for debugging. Example: http://127.0.0.1:8080)
```

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